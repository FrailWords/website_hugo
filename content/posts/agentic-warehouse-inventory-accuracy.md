---
title: "Productionizing a Warehouse Anomaly Detector"
date: 2026-08-27
image: "/images/agentic-warehouse-inventory-accuracy-og.png"
tags: ["software-engineering", "ai-agents", "agentic", "temporal", "warehouse", "logistics"]
---

{{< figure src="/images/warehouse/warehouse_cover.svg" alt="Warehouse anomaly detector" >}}

I've been meaning to learn 'agentic' software development, specially learning what a 'harness' means and what it provides out of the box and what it doesn't.

For most of my career, I've been working with what are called 'frameworks' - spring-boot, bootstrap, nextjs, react etc. In my mind, I thought that a 'harness' is very close to what a 'framework' is i.e. to provide a set of pre-built abstractions that help build a more robust and production-ready solution.

Recent job search has lead me to looking at one of these 'harness' like things, if you will, called 'Temporal'.  The website describes it as a 'workflow/execution engine/platform for long-running and durable jobs' (something around those lines). That is a different shape to the frameworks I knew - not something you build your app *in*, but a platform your app talks to, with a server of its own that keeps state.

So, I wanted to do two things - first figure out a simple domain where there's a specific practical problem to be solved, that can be 'agentic'ally solved, and then secondly, see how something like 'Temporal' helps in making this solution 'production-ready' (more on this later).

The code is on GitHub: [FrailWords/warehouse_anomaly_finder_temporal](https://github.com/FrailWords/warehouse_anomaly_finder_temporal). Clone-and-run instructions are in the README.

Most of what follows is warehouse domain, not tech. That is on purpose - modeling the domain is what decides whether the thing works, and it is the part that does not transfer. The Temporal and agent pieces sit on top, and those do.

## Where This Came From

Recently, I had a chance at an interview (didn't get the job though sadly) with a company thats using drones to detect inventory anomaly inside a warehouse, where having a clear and consistent view of the inventory is of paramount importance. This company used their drones to photograph the shelves, and then check what those photos show against what the inventory records claim.

## TLDR - what i learnt building this

Two things stood out. First, once the domain was modeled, almost every decision the system makes is a fixed rule. The model does its work at one point: when a flagged discrepancy is ambiguous, it runs an investigation of its own, calling a few tools and working out from their results what actually happened. That is the part you cannot write as rules ahead of time.

Second, what makes the system trustworthy is not the model. It is ordinary durable-execution plumbing - a workflow that resumes after a restart, typed contracts between steps, a log of every run - and most of that came from 'Temporal'.

## The Domain

A warehouse is a grid, and the Warehouse Management System (WMS) is the database that tracks what should be where.

{{< figure src="/images/warehouse/warehouse_storage_hierarchy.svg" alt="Warehouse storage hierarchy: rack, bay, slot, pallet, case, unit" >}}

For this blog's scope, we need to understand the 'hierarchy' of terms in a warehouse. A slot is the smallest unit the WMS tracks. A pallet sits in a slot, one or more per slot, and carries its own id, the LPN (license plate number). A full address reads Zone B → Aisle 07 → Bay 04 → Slot 2 → LPN-884211, and the drone scans that final LPN.

## Why the Records Drift

The WMS is only as accurate as the people operating it, and every unscanned move pushes it further from reality. To catch that drift, a drone photographs one slot at a time and a vision model reads the photos into counts, labels, and a fullness estimate. That is what we can call the 'observed state'.

## Expected State vs. Observed State

The WMS holds our 'expected state'. The photo gives the observed state. The problem is the gap between them.

{{< figure src="/images/warehouse/anomaly_expected_vs_observed.svg" alt="Slot B-07-04: the WMS expects 3 pallets, the drone photo shows 2" >}}

If the WMS expects 3 pallets in a slot and the drone photo shows 2, that gap is an anomaly. It could be an unscanned pick, a pallet misplaced one bay over, or a bad photo. Working out which, and what to do about it, is the job.

## What the Drone Captures

Each scan is one slot. The drone photographs it and reads the license-plate label on each pallet.

{{< figure src="/images/warehouse/pallet_label.svg" alt="A pallet's license-plate label: a 1D barcode over the LPN, with SKU, lot code, and quantity fields" >}}

A vision model turns the photos into a small bundle of structured data:

- how many pallets it sees
- which labels it could read
- how full the slot looks
- a confidence score for the read

That bundle is the observed state. Everything downstream works off it, not the photos.

## How the Data Is Modeled

A scan carries a count, the labels, a confidence score, and the gaps against the WMS. Confidence is kept apart from the count: one slot read the right count at 0.55 confidence against 0.94–0.97 elsewhere, so it was marked a bad scan and its count ignored.

```json
{ "slot_id": "B-07-04", "type": "missing_pallet", "lpn": "LPN-884212",
  "zone_type": "pick_active", "hours_since_movement": 43.0,
  "sku_velocity": "high", "unit_value_usd": 189.99 }
```

The rest of the fields in that record are what the pipeline weighs for urgency: zone, hours since the last move, how fast the item sells, unit value. A cheap, long-idle item barely matters; a fast-selling, expensive one in a busy zone does.

Two more inputs sit outside the record:

- **WMS row** — the exact pallets expected, so a swap shows even when the count matches
- **movement log** — every move in the warehouse, so the first question can be "did this pallet move after the scan?"

## Four Mismatches, Two Real Problems

The sample zone has four slots where the count did not match, and only two turn out to be real. One of the others was a pallet that had already moved, with the move sitting in the log once someone checked. The last was a bad photo.

{{< figure src="/images/warehouse/anomaly_walkthrough_4_slots.svg" alt="Four count mismatches: two are real anomalies, two already have an explanation" >}}

That is why the pipeline exists. A count that does not match is a reason to look closer, not a reason to send anyone to a shelf. Each slot gets two lookups and a comparison; a gap then runs a chain of cheap checks before any expensive one, and only a confirmed, serious anomaly reaches the floor, where a person still has to approve it.

## Where Temporal Fits

A single run walks a whole zone, slot by slot. Some slots need a database lookup, a few need a model call, and when one produces a ticket, the run stops and waits for a person to approve it. That wait has no fixed length, and in that time, the worker process can be redeployed or crash.

If it restarts and begins again from the first slot, it re-runs every model call it already paid for and re-files tickets someone may have already acted on. The run has to come back exactly where it left off.

{{< figure src="/images/warehouse/durable_replay.svg" alt="Durable replay: each completed step writes its result to a log; after a restart the log replays and the run resumes" >}}

Temporal runs the pipeline start to finish and guarantees it completes, even across a restart days later. It manages that by replay: on a restart, it executes the workflow function again from the top, feeding back the results it already recorded instead of redoing them. That only works if the function is deterministic, so the same inputs always take the same path. Anything that reads the outside world breaks that, because the outside world moves: a row changes, or the model answers differently.

So those calls go into activities. An activity is a function Temporal runs once and writes the result into the run's history; on replay, it reads that result back rather than calling out again, and it comes with retries and a timeout for free. A pure step has a result too; it just is not recorded, because re-running it on replay is free and safe.

- **side-effecting work** (DB reads, the model call) → an activity: run once, recorded, replayed from the record
- **pure rule logic** → stays inline, re-run from scratch on replay
- a run can die at any point and resume without repeating a paid call or a floor action

## Mapping the Pipeline to Components

{{< figure src="/images/warehouse/pipeline_components.svg" alt="Who runs what: Claude Sonnet drives the loop, fixed rules classify, and the tools it calls; a workflow holds state and a worker runs the I/O" >}}

Four moving parts:

- **Orchestrator** — Claude Sonnet. Walks the six-step prompt for one slot, reading each tool result before choosing the next call. For most of those steps, the prompt already fixes what comes next, so the verify step is the only one where it has a real choice.
- **Verifier** — a cheaper model that runs a short loop of its own. Given a flagged gap, it calls two read-only tools (a scan of the adjacent slots, and a pull of the pallet's full movement history) in whatever order the evidence points to, and returns a verdict: is the pallet missing, misplaced, or is the record just out of date?
- **Classifier** — no model, just a rule. It takes the four urgency facts (the zone, how long since the pallet moved, how fast it sells, what it is worth) and turns them into a severity level.
- **Remediation** — the only part that touches the floor. For a confirmed high-severity anomaly, it writes a move-and-recount ticket into an approval queue; a person signs off before anyone walks to the shelf.

Underneath, the workflow holds the run's state, the I/O tools run as activities on a worker, and the classifier runs inline with a fixed id, so replay regenerates it exactly.

## Inside the Pipeline

There is no pipeline class and no state machine. The prompt is the pipeline: six numbered steps, with the branch conditions written out in plain English.

{{< figure src="/images/warehouse/prompt_six_steps.svg" alt="The prompt's six steps in order: plan, compare, classify, movement check, verify, report. The movement check settles most anomalies; the model runs only at the verify step, and only if the gap is still unexplained." >}}

The steps go cheapest first, so most slots stop early:

1. **Plan** — read the slot's data, lay out the checks to run.
2. **Compare** — expected pallets against what the scan saw. A match ends here.
3. **Classify** — the rule turns the four urgency facts into a severity level.
4. **Movement check** — look in the movement log. A recorded move explains the gap, and the slot closes as a false positive.
5. **Verify** — reached only if the gap is still unexplained. The verifier model investigates with its own tools and returns a verdict.
6. **Report** — write the outcome: a note, a dropped flag, or a recount ticket for approval.

The model acts only at step 5, and not always. If its confidence is low, or the next step is expensive or would send a worker to a shelf, the run stops and waits for a person instead.

## Where the Agent Earns Its Place

Every other step runs the same path each time. Verify cannot. Confirming a missing pallet is an investigation, and each check's result decides the next one. So the verifier is a smaller model with two tools of its own. It can scan the adjacent slots, and it can pull the pallet's full movement history. It calls them in whatever order the evidence points to.

{{< figure src="/images/warehouse/verify_agent_loop.svg" alt="The verify loop: the model scans the adjacent slots, finds LPN-884212 next door, checks its movement history, and concludes the pallet is misplaced rather than missing" >}}

For B-07-04 it runs like this. The cheap movement check came back empty: nothing in the log explains the missing pallet. So the model starts from the claim and works outward:

1. Scan the neighbors. B-07-05 is holding an extra pallet, LPN-884212.
2. Check that pallet's history. It was placed in B-07-04 and never moved after that.
3. So the pallet was misplaced, not lost. Write a move-and-recount ticket. Nothing is actually gone.

That is one path. A slot flagged the same way but with clean neighbors goes the other way, and the movement history decides it. If the only record is the pallet being placed there, it really is missing. If there is a record of it being placed and then taken out, the WMS is just out of date. The model picks as it goes.

On a real run against the sample zone, the model worked this out itself. The pallet's history had one entry: it arrived in B-07-04 weeks earlier. Nothing was recorded after that, no move to B-07-05. So it decided the pallet had been put in the wrong slot rather than lost, and it was confident of that (0.95). The run created two tickets, one to recount B-07-04 and one to move the pallet back, and paused for a person to approve them.

### Why Not a Decision Tree

You could write this as rules: if the neighbor scan is clean, check the movement history; if the pallet turns up next door, check whether a move explains it; and so on. It holds until you count the branches. The state of an investigation is everything seen so far: the neighbor scans, the movement history, the WMS record. Each of those has several outcomes, so you would be naming the right next move for every combination and keeping that tree correct as the warehouse keeps finding new ways to be wrong.

{{< figure src="/images/warehouse/verify_rules_vs_model.svg" alt="A decision tree fans out into every branch you must write and maintain by hand; a model with tools runs a short pick-run-read-repeat loop instead." >}}

The rules approach has its own cost. You would write out every branch by hand, then keep all of it correct as the warehouse turns up new cases. That is a lot of code to maintain for a step that runs only now and then. Giving the model a few tools and a goal is far less to write, and a case nobody planned for is just another run rather than a change to the code.

This loop is the one place in the project where an agent does something a fixed pipeline cannot. It runs inside a single Temporal activity, so from the workflow's side it is still one step. That also means its internal tool calls do not show in the event history, and if the activity fails partway, it retries from the start.

The price of a model here is the usual one: it will not always reach the same verdict, and each run is several model calls, not one. That is the next section.

## Running It in Production

Most of what it takes to run this in production has nothing to do with the model. Deployment works as before. A failed run resumes from the workflow log instead of starting over. That log is also a full trace of every run.

Three things change because a model sits in the loop.

- **Change failure rate** — a prompt cannot be unit-tested. Catching a regression takes an eval: labeled anomalies with known answers, run through the pipeline. The model picks its own path, so the same case can land on different verdicts. The eval runs each case a few times and measures how often the verdict is right. The example has one covering three cases: a misplaced pallet, a genuinely missing one, and a stale record.
- **Cost** — every model call costs money, and the verify loop runs several per anomaly. A real service meters token use per run and caps it. The example trusts a single "be economical" line in the prompt.
- **Security** — the evidence text handed to the model could be written to steer its verdict, so keep it structured. Keep operator names out of the prompt.

The data feeds are separate work. Here the three inputs arrive clean. A real deployment builds each one: the expected state from a scheduled WMS export that can be hours stale, the movement log from a nightly export that runs a day behind, the urgency facts from other systems on their own schedules. This is the integration work any system pulling from several sources has always needed.

## What Hasn't Changed

Set the model aside, and nothing in this system is new. Each piece is a standard engineering pattern with a warehouse name on it:

| In this post | What it is |
| --- | --- |
| The task queue Temporal manages | Producer/consumer decoupling |
| Typed tools with fixed shapes | Interface contracts between stages |
| The verifier cascade: heuristic, then model, then human | A cheap check before an expensive one |
| The human review gate and approval policy | A manual approval queue, enforced structurally |
| Durable replay from a recorded log | A restartable, checkpointed job |
| The pure classification rules | The unit-testable core |

Once the domain is modeled, the "agentic" system is a fixed pipeline of rules with a model at one step, the same work I have always done. It matters more here because that one non-deterministic step can open a real ticket and send a person onto the floor.

So the takeaway is modest: use a model only where the next move genuinely cannot be known in advance, keep the rest boring, and put your care into that one uncertain step.
