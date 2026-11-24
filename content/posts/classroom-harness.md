---
title: "Bringing the Learning Back to AI-Assisted School Work"
date: 2026-11-24
image: "/images/classroom-harness-og.png"
tags: ["software-engineering", "ai-agents", "harness", "privacy", "education"]
---

I've been increasingly thinking about where the 'learning' happens when AI does most of the work, across many domains like tech and education, and specifically in schools. Schools also turned out to be one of the best-suited places to try out the idea behind this post. Nowadays, a student can hand in an AI-written essay and the teacher can't even tell if it's actually written by the student or an AI.

I also care a lot about privacy. When a school uses AI, its students' work ends up with whichever company runs the model, and the school has little say over what gets sent or where the work is kept.

So I started thinking about what a 'harness' that helps with all of these might look like:

- keeping the learning with the student
- putting the school and the teacher back in control of that learning
- keeping the students' data private

I then ideated, designed and built a working prototype of such a harness, together with Claude.

## What I Mean by a Harness

I'd been coming across the word 'harness' a lot lately. Companies like [Anthropic](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) and [OpenAI](https://openai.com/index/harness-engineering/) use the word for the software that sits around an AI model and helps the model carry out a task on its own, like writing code.

The harness I had in mind is pretty much the same kind of software. The difference is that a student uses the AI, and the teacher and the school set the rules the AI has to work within.

{{< figure size="narrow" src="/images/classroom/harness_between.svg" alt="The harness sits between the student and the AI, and the teacher sets its rules" >}}

So every question a student asks the AI, and every reply that comes back, goes through the harness. The teacher decides what kind of help is allowed, and the harness makes sure the AI sticks to those rules.

## The Teacher Defines the Assignment

Let's take a fictional example of a school science task: a one-page explanation of how plants make their food from sunlight.

Today, a teacher might hand out an assignment sheet with steps to follow, such as noting what you already know, reading up and writing a draft. Nothing prevents the student from skipping the steps and just submitting an AI generated final explanation directly.

So in the harness, the teacher turns those steps into five *stages*. The student has to go through them one at a time and in order, so the explanation gets built up bit by bit from the student's own thinking.

{{< figure size="narrow" src="/images/classroom/task_stages.svg" alt="The task as five stages: Recall, Research, Draft, Improve, Submit" >}}

An assignment written out as stages like this is called a *workflow*.

The first stage is called Recall. The teacher sets the stage's goal as "Write what you already know", so the student starts from their own thinking.

In each stage, the student can ask the AI for help, and the teacher decides what kind of help the AI may give:

- In the Recall stage, the AI can ask a question back to get the student thinking, or give a hint that points the right way without giving the answer.
- In the Research stage, the AI can give hints, explain an idea or show an example.
- In no stage can the AI write the explanation.

### How the Stages Are Defined and Run

The photosynthesis assignment is a single file written in *YAML*, which is a plain-text format often used for settings. Here is the start of the file.

```yaml {hl_lines="8-9 11-12 14 16"}
kind: Workflow
metadata:
  name: photosynthesis-explainer
spec:
  deliverable: "One-page explanation of photosynthesis with a labelled diagram"
  stages:
    - id: recall
      goal: "Write what you already know"
      help: [ask-question, give-hint]
      steps:
        - { type: student-input, target: notes, minWords: 40 }
        - { type: decision, ask: on_topic, onLow: nudge }
    - id: research
      goal: "Find how light, water and carbon dioxide are used"
      help: [explain-concept, give-example, give-hint]
      never: [write-deliverable]
    # draft, improve and submit follow the same pattern
```

The highlighted lines are the teacher's choices:

- For the Recall stage, the goal, the help the AI may give, and two steps that run when the student finishes the stage.
- For the Research stage, the goal, and a rule that the AI never writes the explanation.

{{< figure size="narrow" src="/images/classroom/finish_recall.svg" alt="When a student finishes Recall, its two steps run in order before Research begins" >}}

The first step needs at least 40 words of notes. The second asks a smaller AI model whether the notes are on topic, and if they aren't, the student gets a gentle reminder.

Today these files are written by hand. A future implementation could provide a UI to help build such workflow files.

## When a Student Asks the AI

Inside a stage, the student asks the AI questions in their own words.

Before the AI replies, a second, smaller AI model reads the student's question and answers a few simple questions:

- Is the question safe for a child to ask?
- Is the question about the topic?
- Is the student asking for the finished work?

The harness uses those answers to decide what kind of reply the student gets.

Here is what happens to three different questions in the Research stage, where the student reads up on the topic.

{{< figure size="narrow" src="/images/classroom/three_questions.svg" alt="Three questions and what each one gets" >}}

The first question is about an idea, so the student gets the explanation of the idea. The second asks for the finished work, so the student gets a hint instead. The third has nothing to do with the topic, so the student is reminded what the stage is for.

### How Each Question Is Checked

{{< figure size="narrow" src="/images/classroom/question_checks.svg" alt="One student question, step by step" >}}

For every message, the smaller model is asked five questions, one at a time:

- Is the message unsafe?
- Does the message use bad language?
- Is the message off topic?
- What kind of request is the message?
- How much of the student's own thinking does the message show?

The smaller model only gives answers. The harness then follows the school's rules to decide what to do. If the smaller model isn't sure a message is safe, the larger model is asked too.

## When a Student Gets Stuck

Sometimes one hint isn't enough. Giving a stuck student the full answer straight away prevents any learning, so the AI's help gets more detailed as the conversation continues.

Say a student in the Recall stage asks the AI for a hint. The first reply is a question back to the student, just to trigger the thought process.

The student asks again without changing their notes. After two replies like this, the help gets a bit more detailed, and this time the student gets a hint.

If the student keeps asking without changing anything, the AI keeps giving hints, because the teacher didn't allow examples in the Recall stage.

If the student is still stuck after all that, the AI stops helping and tells the teacher what the student has tried so far, so the teacher can step in.

The moment the student changes their notes, the help starts again with a fresh question back, since the student is moving forward again.

In a stage where the teacher allows examples, there are two more kinds of help in between, so all together the help looks like this.

{{< figure size="narrow" src="/images/classroom/hint_ladder.svg" alt="Five kinds of help, in order, from a question back to telling the teacher" >}}

## When a Student Pastes a Lot of Text

When a student writes the work themselves, their notes usually grow a little at a time. A big block of text arriving in one go might mean the text came from somewhere else, like a chatbot on another device.

{{< figure size="narrow" src="/images/classroom/paste_check.svg" alt="Pasting 60 words brings up two quick questions" >}}

So when a student pastes 60 words or more at once, the harness asks them two quick questions about what they just added. The questions are based on the pasted text, so the student needs to understand that text to answer them well.

The student also sees why the questions came up, for example "60 words arrived in one step, so two questions about it." If they don't know an answer, they can ask for a hint instead.

Every now and then, the harness also asks a student a quick question at random, so the questions don't only come up when something looks wrong.

## A Record of Every Step the Student Takes

Everything a student does in the harness is written down in order. That includes every note they write, every question they ask, every reply from the AI and every quick question they answer. This record is called the *trail*.

{{< figure size="narrow" src="/images/classroom/trail.svg" alt="Part of a trail: five things that happened, in order" >}}

With the trail, the teacher can see how the explanation came together, from the first notes to the finished version.

In this trail, the teacher can see that the student pasted 60 words and then answered two questions about the paste.

### Keeping the Trail Immutable

The teacher's decision rests on the trail, so nobody should be able to change a step once the step is written. That includes students, staff and bugs in the code.

The trail is kept immutable in three ways:

- The database refuses to change or delete any step on the trail.
- Each step gets a fingerprint, called a *hash*, made from the step itself and the fingerprint of the step before. Changing, removing or swapping any step breaks the fingerprints from that step on, and a check names the first step that doesn't match.
- When the student hands in the explanation, the last fingerprint is signed with the school's own key. So even rewriting every fingerprint afterwards is caught.

{{< figure size="narrow" src="/images/classroom/hash_chain.svg" alt="Each step's fingerprint includes the one before, and the last is signed with the school's key at hand-in" >}}

The fingerprints use SHA-256, and the signature uses ECDSA. On AWS, the school's signing key never leaves AWS's Key Management Service.

## How the Trail Is Reviewed

A teacher cannot be realistically expected to read every student's trail line by line. So when the student hands in their explanation, the harness goes through the trail first. This step is called the *review*.

{{< figure size="narrow" src="/images/classroom/review_steps.svg" alt="The review works out numbers from the trail, then an AI writes notes" >}}

First, the harness works out a few simple numbers from the trail on its own, such as:

- how big each change to the notes and draft was
- how much of the AI's wording ended up in the final explanation
- whether the student answered the quick questions after a big paste

An AI then reads a short summary of the trail along with those numbers, and writes down what the AI noticed. Each note points to the exact moments in the trail behind that note, so the teacher can go and check.

## The Teacher Decides

Each assignment comes with the teacher's *grading or marking criteria*, which is the list of things the explanation is judged on. Based on those criteria, the review suggests one of three outcomes:

- *Accept*. The trail shows the student building the explanation step by step.
- *Accept with notes*. The explanation is fine overall, with a few moments in the trail the teacher may want to look at.
- *Talk to the student first*. Something in the trail, such as a large paste or a trail that has been changed, needs explaining before the teacher decides.

{{< figure size="narrow" src="/images/classroom/review.svg" alt="The review reads the trail and suggests an outcome; the teacher decides" >}}

The review can't reject an explanation. The teacher reads the review's notes, talks to the student if needed, and makes the final decision.

## What Gets Sent to the AI Provider

Every time the harness asks an AI model for something, the request goes to the AI provider that runs the model.

A student's message can easily include their own name, a friend's name or a phone number. To protect the students' privacy, none of these personal details should ever leave the school.

So before any request goes out, the harness swaps each student's name for an ID such as s_7b01, and replaces phone numbers and email addresses with a plain label. Swapping names for IDs like this is usually called *pseudonymization*.

{{< figure size="narrow" src="/images/classroom/redaction.svg" alt="Names become IDs and the phone number becomes a label before the request leaves" >}}

The AI provider still gets the student's question, and never learns who asked the question.

The list of which ID belongs to which student stays inside the school. So when the review of the student's trail comes back talking about s_7b01, the harness puts the name back in, and the teacher reads "Asha".

### How Requests Reach the AI Provider

Every AI call in the harness goes through a *gateway*. No other part of the harness talks to an AI provider directly.

{{< figure size="narrow" src="/images/classroom/gateway.svg" alt="Checks, hints and reviews all reach the AI provider through one gateway" >}}

For every call, the gateway:

- swaps names for IDs, and phone numbers and email addresses for labels
- picks the smaller model for checks and the larger model for writing
- checks the spending against the budget for the school, the class or the assignment
- records the call, including which model was used, what the call was for and how many tokens the call used

Because every call passes through the gateway, adding another AI provider is mostly a change to the gateway. Each model in the routing settings names its provider:

```yaml {hl_lines=[3]}
models:
  anthropic: { small: anthropic/claude-sonnet-5-5, large: anthropic/claude-sonnet-5-5, decision: anthropic/claude-haiku-5-5 }
  sarvam:    { small: sarvam/sarvam-105b, large: sarvam/sarvam-105b, decision: sarvam/sarvam-105b }
```

and the gateway looks that provider up in one table:

```python {hl_lines=[1]}
PROVIDERS = {"anthropic": _claude, "bedrock": _bedrock_converse, "sarvam": _sarvam}

def converse(model_id, system, user, json_out=False, max_tokens=800, schema=None):
    provider, _, model = model_id.partition("/")
    if provider not in PROVIDERS or not model:
        raise ValueError(f"model id {model_id!r} should be <provider>/<model>")
    return PROVIDERS[provider](model, system, user, json_out, max_tokens, schema)
```

Adding Sarvam meant one line in the routing settings, one entry in the table, and the function that calls Sarvam's API.

## Keeping Stored Data Private

The harness encrypts each student's notes, drafts and trail, along with their quick questions and reviews. Even someone with a copy of the database can't read them without the right key.

{{< figure size="narrow" src="/images/classroom/class_keys.svg" alt="Each class has its own key, which locks only that class's data" >}}

Each class has its own key, so one class's key can't open another class's notes, drafts or trails.

### How the Data Is Encrypted

Each class's data is encrypted with that class's key, and each class key is in turn encrypted with the school's main key. This is a common pattern called *envelope encryption*. Each class keeps a key of its own, while only the one main key needs protecting.

{{< figure size="narrow" src="/images/classroom/keys_envelope.svg" alt="Each class's data sits inside its class key, and every class key sits inside the school's main key" >}}

The encryption uses *Fernet*, an authenticated symmetric encryption scheme from Python's cryptography library. Fernet encrypts with AES-128 and signs the result with HMAC-SHA256, so any change to the stored data is detected when the data is read back.

On AWS, the main key lives in AWS's Key Management Service and never leaves the service. Every request to unlock a class key names the school, so a class key locked for one school can't be unlocked for another.

The next step is for each school to hold its own main key, in a key service the school controls, so the people running the platform can't read stored data without the school's permission.

A school might instead keep its main key on a hardware key in the school's office. A key like that could be lost, and losing the main key would make everything unreadable.

So a backup of the main key can be split into three parts for three different people, such as the principal, the IT head and a trustee. Any two of them together can rebuild the main key, and one part on its own reveals nothing about the main key. This method is called *Shamir's secret sharing*, and the prototype already supports the method.

Getting each part to its person safely is still an open problem. Right now, one command prints all three parts on the same screen. A password manager, with a separate vault for each person, could solve this.

## How the Harness Is Organized

Looking back at the pieces so far, most of them have nothing to do with schools in particular. Stages, limits on what the AI may do, a record of every step and keeping data private would matter just as much in another domain, say a law firm or a medical clinic.

So the harness is organized in three layers.

{{< figure size="narrow" src="/images/classroom/layers.svg" alt="Three layers: the school layer on top, the core in the middle, the platform underneath" >}}

- **For schools.** Everything specific to schools: assignments, the workflows for them, grading criteria, teachers and students.
- **The core.** The same in every domain. The core runs the workflows, checks every request and reply, keeps the trail and swaps names for IDs before anything goes to an AI provider.
- **The platform.** Runs the core for each school and keeps each school's data separate.

Only the top layer needs to change for a new domain.

## The Same Core in a Law Firm

The school design started with design thinking. I listed the five people who decide whether a school starts using AI for school work, and the worry each one has:

- **The student** already uses chatbots.
- **The teacher** sees only the finished work.
- **The principal** has to answer parents.
- **The IT in-charge** asks where the data lives.
- **The parent** asks what is recorded.

Each part of the harness answers one of those worries.

A law firm would start the same way, with its own people:

- a junior lawyer who drafts a contract with help from AI
- a senior lawyer who signs the contract off
- the client
- whoever looks after the firm's data

Their worries would shape the top layer, while the core stays the same.

| | School | Law firm |
|---|---|---|
| The workflow | an assignment in stages | the steps for drafting a contract |
| Who sets the rules | the teacher and the school | the senior lawyer and the firm |
| Main limits on the AI | safe for children, and on topic | client data never leaves the firm |
| The trail shows | how the student reached the answer | who or what wrote and checked each clause |
| The review checks against | grading criteria | a quality and compliance checklist |

## Making the Harness Simple to Use

For the harness to be useful, setting up a new school has to be easy, and nobody at the school should have to look after servers.

{{< figure size="narrow" src="/images/classroom/one_file.svg" alt="One file describes the school, and the platform reads it and sets up everything the school needs" >}}

So adding a school means writing one file that describes the school: where the school's data must stay, which AI models the school uses, the school's monthly budget, and how separate the school is from other schools. The platform reads that file and sets up everything else. The school then signs in and starts using the harness.

A school that already has an account with an AI provider can connect that account, so the harness's AI calls go through the school's own account. This needs *programmatic access*, meaning an account that software can call. A typical subscription to a chatbot's website or app doesn't include that.

### How a School Is Set Up

In the design, each school is described in one file, like the one below. The school and the numbers are illustrative.

```yaml {hl_lines=[5,7,9,10,12,14]}
kind: School
metadata:
  name: greenfield-high
spec:
  tier: shared                    # how separate from other schools: shared, virtual-cluster or dedicated
  placement:
    dataResidency: in             # the school's data stays in India
  models:
    decision: anthropic/claude-haiku-5-5            # the smaller model, for checks
    monthlyBudget: { currency: INR, amount: 60000 } # the most the school spends on AI in a month
  sessions:
    warmPool: { fromCalendar: true, max: 300 }      # sessions started ahead of time, from the timetable
  data:
    encryption: { keyOwner: school }                # only the school holds its main key
```

A *Kubernetes operator* is a program that watches files like this one. The operator compares what the file asks for with what exists, and fixes the difference. That way the operator sets up the school's own space, sign-in, key, budget and computers for student sessions.

- **Separation.** A school picks how separate the school is from other schools: a shared cluster, a virtual cluster of its own, or a dedicated cluster. The school's data, keys and network are separate at every level.
- **Timetable.** School use follows the timetable, so computers for student sessions start ahead of lessons and deadlines, and scale to zero at night.

The operator and these files are still a design, and so is connecting a school's own AI provider account. What runs today is the harness on one machine with Docker, plus a Terraform setup for AWS that has been tested on an AWS emulator.

## Measuring Correctness of AI Checks

Before the AI replies to a student, a smaller AI model answers a few questions about the student's message, such as "Is the message aligned to the topic?". I wanted to know how often the smaller AI model gets those answers right.

Here is how the test went:

- I collected example student messages from the photosynthesis assignment's Research stage, and wrote down the right answers beforehand.
- Each test ran several times, so each result is a range.
- Both models got exactly the same instructions, which were written and checked with Claude first, and not changed for Sarvam.

What the two models did:

- **Claude Haiku 5.5** from Anthropic got about 95 in 100 answers right. One of Claude Haiku's misses was calling "Is it dangerous to look at the sun to see how much light plants get?" unsafe every time, even though the question is a fair one about the topic.
- **Sarvam 105B** from the Indian company Sarvam AI got about 90 in 100 answers right. Sarvam's models run in India, which matters to any school that cares where student data goes. One of Sarvam's misses was treating "Who won the cricket match yesterday?" as on topic in most runs.

| | Claude Haiku 5.5 | Sarvam 105B |
|---|---|---|
| Right answers | 94 to 97 in 100 | 86 to 93 in 100 |
| Where the model runs | outside India | in India |

These are small tests, so a few answers either way would change the results.

## Measuring Whether Hints Give the Answer Away

The next thing I wanted to know was whether the hints actually hold back the answer, because a hint that hands over the answer stops the student from doing the thinking.

Hints are written by a larger AI model than the one doing the checks. Claude Sonnet 5.5 writes the hints on the Claude side, and Sarvam 105B writes the hints on the Sarvam side.

I gave both models the same student questions and had both models judge every hint:

- Every one of Claude Sonnet 5.5's hints held back the answer.
- About 1 in 4 of Sarvam 105B's hints gave the answer away, usually by stating a fact outright.
- The judges didn't agree. Sarvam 105B flagged those hints, but Claude didn't flag a single hint from either model.

So whether a hint gives the answer away depends partly on which model is doing the judging.

| | Claude Sonnet 5.5 | Sarvam 105B |
|---|---|---|
| Hints that held back the answer | all of them | about 3 in 4 |

### How the Tests Were Run

{{< figure size="narrow" src="/images/classroom/test_design.svg" alt="How one check is tested: the right answer is written beforehand, the model answers several times, and the two are compared" >}}

- **Answers first.** The right answers were written down before any model ran, so the models are compared with a person and never with themselves.
- **Repeats.** Each test ran several times, because a model doesn't always answer the same way twice.
- **Same instructions.** Both models got the same instructions, which were first tuned with Claude.
- **Two judges.** Both models judged every hint, because a model may go easy on its own replies.
- **Real moments only.** A review passed only if every note pointed to a real moment in the trail, so the teacher can check every note.

## Measuring Cost

I also measured what one student doing the whole photosynthesis assignment costs in AI, from the first question to the review.

| | Claude | Sarvam 105B |
|---|---|---|
| One student, one whole assignment | about ₹7 | under ₹1 |

Claude costs more partly because Claude Sonnet 5.5 thinks the problem through before replying, while Sarvam 105B ran with thinking turned off. Sarvam's prices are also introductory for now.

Choosing between the models means weighing accuracy, cost and where the data is processed, and that choice is the school's to make. The rules, the trail and the keys stay the same whichever model the school picks.

## What the Harness Can't Stop

A determined student can still copy from a chatbot on another device or account, such as a phone, a second laptop or a personal account.

If the student then types the copied answer into the harness a few words at a time, the student's notes grow slowly, just like steady work, and no quick questions are triggered.

Copying slowly like this takes longer than doing the work, and the quick questions at the end of each stage still ask the student to explain the steps. Even so, the harness can't fully stop a student who copies this way.

The tests used *simulated students*, each with a known way of working. The simulated student who copied slowly often got an "accept" from the review.

## Where This Stands

The harness runs end to end on a fictional school, from the teacher's workflow to the teacher's final decision.

All the testing so far has used simulated students and small sets of example messages. Whether the harness actually keeps the learning with the student can only be measured with real students in real classrooms, and that trial is the next step.

If you work with schools, or have thoughts on any of this, I'd love to hear from you.
