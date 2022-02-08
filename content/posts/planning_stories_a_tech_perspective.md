---
title: "Agile Story Slicing - Horizontal or Vertical ?"
date: "2022-02-09"
tags: ['agile', 'story', 'story-slicing', 'planning']
---
**Disclaimer:** There are probably a lot of cake related metaphors in this post, you might feel like ordering one by the 
end of this post, which is an unintentional side effect :)

### A well-defined story

Creating well-defined stories is an essential part of software development nowadays, specially if you are following any 
of the "Agile" methodologies where the key motivation is to incrementally deliver value and at the same time building 
towards a goal or a product release.

One of the main ways to define a _well-defined/good story_ is through the I.N.V.E.S.T. principle - 

* **I** - independent - story should NOT be dependent on another story
* **N** - negotiable - scope of the story should be adjustable or negotiate how its implemented
* **V** - valuable - should provide business value
* **E** - estimable - enough clarity to be estimable
* **S** - small - small enough to provide quick feedback
* **T** - testable - independently verifiable and testable 

From my experience, it is super-hard for every story to satisfy each of these properties and the main reason that I see 
consistently is how a software product is developed from scratch. I will try to talk about this through comparisons on 
how stories span through iterations of development. 

**NOTE:** To simplify the discussion, I am going to make the assumption that the technology(s) we are going to use are known to 
the team already.

### Preface - Iteration 0

At a very abstract level, majority software product development (from scratch) starts with -

* Infrastructure setup - cloud provisioning, infra-as-code ...
* Skeleton/scaffolding code setup - using best practices for the language/framework ...
* CI/CD - path-to-production - including steps like code analysis, testing, building and deployment  ...
* Observability/monitoring/alerting/logging setup ...

In brief, we are setting the foundation of the project so that it is _production ready_ from day one.

So, we've not built any _actual_ software for the problem statement which _delivers value_.

Usually what I've seen is that teams spend a lot of time refining these steps as the project goes along and with increasing 
infrastructure complexity (viz. microservices, kubernetes, cross-platform apps), the effort spent in this is significant.

The clarity that this begs is how the _business value_ is or might be perceived differently by a tech-savvy vs a non-tech-savvy 
business or customer and how much are we willing to shed in terms of delivering the best-in-class software engineering. 

### An Alternative View - Story Slicing

Given that we do not want to compromise on engineering excellence and what is considered best-in-class, the question is how 
these tasks are then tied into the whole "deliver business value" theme.

One way of looking at this is how we _slice_ a story (the whole project/product/scope being the _cake_ metaphor).

There are 2 ways of _slicing_ a story - 

* vertically - like majority of folks would normally eat a cake, by slicing vertically into individual pieces, or 
* horizontally - maybe not so majority who like the icing layer or the fruit only layer :D

### Right Way To Start - Vertical or Horizontal ?

In an ideal world, vertical slicing would be the _right_ way to _slice_ stories as they provide the complete value (think eating 
all the layers of the cake together, not just a single layer of the icing). This is how we deliver _business value_ at the end of 
the day.

But let's flip the cake metaphor the other way around - do we build cakes vertically ? or horizontally ?

When you think of it this way, the point that becomes clear - 

_Enough Horizontal Layers_ need to be built before we can start to _Vertically_ start building something

This idea ties back to our I-0 tasks - if we try to incorporate those horizontal layers into the stories directly and try to 
make it a _vertical slice_, we will end up having really large stories that incorporate too many things and violate the INVEST 
principle.

### Business Value - Horizontal Slicing

But you might be thinking - doesn't the horizontal _slicing_ also violate the INVEST principle ?

Delivering fast business value is the promise of Agile but if we keep building these _horizontal_ layers for a long time, there 
is really no actual business value in the eyes of the stakeholder who is paying for the project - all they are seeing is dashboards and 
pipelines.

The key point that I find here is to how to show all the stakeholders - tech and non-tech - that this is actually _value_, not necessarily 
something that they can see as a web page or an app feature but as a foundational step to building a production ready application.

To build a software system/product of good quality, the expectation needs to be set amongst all stakeholders about how long it
will take to build the _horizontal_ slices and also articulate how those slices add up to form the final picture.

### Trade-Offs - Speed, Quality, Scope

There's this famous saying - that you can only 2 of 3 things - speed, quality, scope - you'll have to trade-off on one point.

* Speed or pace of development is usually directly constrained by the problem space - in how _streams_ of work it can be parallelized 
into.
* Scope & Quality - these two directly translate to _prioritization_ - what's important _right now_ and what can be done _later_.

### Increasing Detail In Horizontal Slicing
When we define our horizontal slices, I've usually seen only _business stories_ being subject to these constraints. Increasingly, I feel 
that the foundational and cross-cutting stories should also be subject to these constraints of speed, quality and scope.

On the basis of these constraints, we should prioritize the horizontal slices in a way that aligns with how want to deliver 
business value.

### Summary

There is no _right_ answer here as you might expect - it's always a constant learning experience on how to define, scope and prioritize,
and I hope this gives the reader a new view point on how a story could be sliced.








