---
title: "Privacy - 9 - Security vs Privacy"
date: "2023-05-22"
tags: ['privacy', 'security']
---
<br />

### Preface

If I mention the word _security_, the usual connotations that come to mind are - confidentiality, integrity and availability (CIA). This is _usually_ the first definition anyone in the security domain would come up with.

Ask the same question but now about _privacy_, usually people would talk about things like online tracking, surveillance, how they are being shown these _creepy_ ads etc. 

The difference is stark in the sense that one (security) has its definition rooted in strictly measurable/testable terms and the other maybe not so much.

In this post, I will try to shed some light on how these can and should be seen in a different lens and become more privacy-aware. 

### What are we protecting ? and from whom ?  

Before we focus on either security or privacy, we should first make clear what we are _protecting_ and from _whom_ ?.

Usually, we are talking about protecting _data_ that's stored somewhere - sensitive information, that if disclosed in an _unauthorized_ way, could lead some sort of _harm_.

### If disclosure is _unauthorized_, who is the _authorizing_ person ?

Now, you must ask - who is the person that _authorizes_ data access/disclosure ? Since data is in constant flow, being converted from one form to another, the _ownership_ of any piece of data 
keeps changing hands and hence the person who _owns_ the data also changes. This _owner_ has the responsibility of now _making sure_ that the data is handled in an _authorized_ way.

### What does _authorized_ mean ?

Usually used from a security perspective, it means that someone has been _given_ or _provided_ with _explicit_ access to certain piece of data. This is usually black and white - either someone has access or they 
do not. By access, I mean 'read', 'write', 'update' or 'delete' operations. Security, then, is like a set of keys that provides a granular way to restrict data access.  

### Implicit vs Explicit Authorization

When data flows through a system consisting of a series of components that take data as input, process it and output data, these authorizations can change from one component to the next in the flow and thus either restricting 
access to the data or widening access to it. Every change in the scope of authorization/"who's authorized" affects how the data would be seen in a new light or used for a new purpose or restricted in terms of its use/purpose.

Such a system can _enforce_ such authorizations either _implicitly_ i.e. as part of the system's property (e.g. using cryptography) or _explicitly_ i.e. as part of assigned controls (e.g. access-controls).

### Online data market & Explicit Authorizations

The online data market is a complex market-place with all sorts of participants like data-brokers, media companies, social networks, ad-tech companies etc. All these participants collect/buy data through direct or indirect means.

You might ask - who _authorized_ them to access all this data ? We all did - unknowingly/knowingly - we all use online services that are provided to us for _free_ in-exchange for the data they collect.

### What happened to all the talk about Security then ?

Security is something is _intentionally_ put _in place_ - it is part of the design of a system or a component that makes up the system. This _intentionality_ can take the form of many levels of _security_ - it is always a balance, 
how _secure_ is _enough_ for the system under consideration ? By _enough_, we want to say what is sufficient to protect the data from plausible threat vectors to the system. This plausible list of threat vectors/actors is also something that 
is a balancing act in terms of prioritization - what is a more plausible threat vs what is highly implausible ?. The _intentionality_ of the security measures in place reflects these priorities.

### But then, Privacy ? 

Historically, privacy as a concern has taken a back-seat and this is a proved by how much personal/sensitive data is out there for sale/marketing/advertising etc. 

The previously mentioned _intentionality_ remained restricted to isolated systems and their internal components and did not focus on the data that makes it way from one system to another in this online data market.

Also, the _authorization_ given to these data market participants was so _wide_ that anyone could use it for any purpose without any legal consequences.

### How do I talk about Privacy then the next time someone starts mixing it up with Security ?

Talk to them about - 

* Who is the original _owner_ of this data ? 
* What _purposes_ is the data being used for and is there clear _authorization_ for collection and use of such data and for said purpose ?
* How would an _unauthorized disclosure_ of this data (even in a transformed format) _affect_ the person or maybe a group that the person belongs to ?

### Final Thoughts

Even with all good intentioned security measures, privacy can take a back seat - we don't know how our data is being collected, transformed and used across a wide variety of online systems.

As long as there's a lack of transparency in how the online data marketplace works, we will just have to rely on awareness as a way to talk about steps that we can all take individually to reduce 
our digital footprint while being a privacy conscious netizen.


