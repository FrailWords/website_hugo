---
title: "Privacy - 10 - Google Privacy Sandbox"
date: "2023-05-30"
tags: ['sandbox', 'google', 'privacy', 'privacy sandbox']
---
<br />

### Lets talk Ads 

The advertising/ad-tech ecosystem (or _programmatic_ ad ecosystem as it is called nowadays) is what underpins the revenue model for most websites, including the biggest tech companies (e.g. Google, Meta, Twitter etc.).

Of these companies, the one company that dominates the ad networks market by far is Google. Current stats show that Google Ads is used by almost `48.7%` of _all_ websites worldwide, with a market share of `98.8%`[[source]][1].

That is staggering by anyone's imagination.  Note: there's a big difference between 'ad networks' and the revenue earned from ads themselves. Here are we are only talking about the reach of Google as an ad network i.e. as a mediator 
in the process of serving ads.

### Lets talk browsers

By far, the most common browser is `Chrome` and this is shown in the browser market share [[source]][2], where nearly `63%` of the world browser market is Google Chrome. The web browser is our gateway of sorts to the internet, making it important 
to understand that our privacy and security on the web hugely dependent on how Chrome protects us from harm.  

### Personalization Paradox

We live in a world of what researchers in the field of human-computer-interaction(HCI) term as _personalization paradox_ [[tech-crunch]][3] - to quote - 

```text
Personalization promises to modify your digital experience based on your personal interests and preferences. 
Simultaneously, personalization is used to shape you, to influence you and guide your everyday choices and actions. 
Inaccessible and incomprehensible algorithms make autonomous decisions on your behalf. 
They reduce the amount of visible choices, thus restricting your personal agency.
```

This points, as the original article points out as well, towards an _unfulfilling_ and _incomplete_ online experience, which leaves us with a feeling that such online personalization 
is based more on someone else's interest, not our own. Our fulfillment lies in a personalized web. And there's lies the conflict/paradox.

### Holy grail of ad-tech - getting the right level of personalized ads for _you_ 

Everyone involved in the ad-tech ecosystem makes more money if they can get you to _interact_ with their ads more and this is directly correlated to how _personalized_ an ad can they show you.
To get to this holy grail, what these companies need is data, about you and your online self and a constant feed of it - not just at one point in time. This is so that they can build a _profile_ of you 
as an individual, of your likes and dislikes and shopping interests etc. and use that to _target_ you with the right ads. This sounds simple, given that we spend more and more time online and how we 
share each and every aspect of our lives online. 

At the same time, people are growing more and more aware of how they want to be _in control_ of what they share online, and the increasing amount of regulation cropping up around the world to support transparency and control.

### Google - our saviour ?  

Google, being the largest market player in this battle for _personalization_, has taken it up on its own to save the world and has come up with what is called the _Privacy Sandbox_.

"Privacy Sandbox"[[privacy-sandbox-web]][4] essentially is a set of proposals and associated implementations, built on Google Chrome as the center-piece for its deployment. Since Chrome has the widest user base, these proposals 
are then of huge consequence for the privacy of users on the web. Point to note: this is NOT a Chrome specific set of proposals - they are open web (W3C) proposals - its just that Chrome is being used as a _vehicle_ for its
_trial_ [[privacy-sandbox-web]][4] and implementation. No other browser manufacturer has this level of power, to unilaterally propose something and then push it for wider adoption. As they say, with great power, comes great complexity (sorry, responsibility).

### Why is Google proposing this ? - the changing ad landscape

Apple broke the mobile ad market in late 2021 with the introduction of what they call _App Tracking Transparency_(ATT) - putting the control of whether an iOS app can track you or not in the hands of the user. This was huge and shifted the 
balance of power towards the user, from the hands of the app companies. This included Google, Meta and everyone who was relying on tracking users on iOS devices to collect personalized data and then use that to do ads on other websites/devices.
This was definitely unilateral from Apple, as it serves as the gatekeeper for everything on the iOS platform. Even then, ATT as a solution was a step in the right direction, improving on the status-quo, even if it is not considered perfect.

Google felt left behind and so _Privacy Sandbox_ was born on the Android platform - the goal being a more _private internet_ (Google's words, not mine). The main difference though, is that main proposals for Android [[privacy-sandbox-android]][5] and 
the ones for the Web [[privacy-sandbox-web]][4] are same - they are trying to address two platforms (Android and the Web) with the same solutions, which is smart (what else would you expect from Google).

While the move for Android is motivated partly by Apple's move, the move towards for the Web is motivated by what can be simply called the death of third-party cookies. The ad-industry is bracing itself for a world where none of 
the browsers will support tracking a user using third-party cookies and thus ending an era of free-for-all tracking. Google wants to lead the ad-tech industry into this next era of what is being called cookie-less or non-cookie signals 
and Privacy Sandbox is being proposed as the solution that will save all the ad-tech companies while making the internet more _private_, which in itself seems like a paradox but let's move on.

### What's in the Privacy Sand-box ? 

Even for an experienced techie like me, the Privacy Sandbox is a hugely complex set of proposals, each proposal claiming to address/solve a part of the problem that gets the right ad to you, while preserving your privacy.
There are 3 main foundational proposals under the _Privacy Sandbox_ - 

* Protected Audience API (previously FLEDGE) [[protected-audience-api]][6] - all about _interest groups_ and how _sellers_ and _buyers_ are matched in the _bidding_ market based on the _interest groups_
* Topics API (follow up to FLoC) [[topics-api]][7] - all about Chrome _categorizes_ every user into _topics_, based on browser history
* Attribution Reporting API [[attribution-reporting-api]][8] - _post-click_ and _post-impression_ attribution of an ad shown to the user - did they actually buy the product _eventually_ ?

Each of the references pointed above go into a lot more detail about what these proposals are and what they mean to a demand-side-platform (DSP)(people buying ad-impressions) as well as to supply-side-platform (SSP)(people who sell ad space), and lastly, 
to publishers i.e. the websites that want to advertise. In the next sections, I'll try to summarize my view in a crude/simple way.

### Protected Audience API or FLEDGE

This is a 30-page-long proposal [[protected-audience-api]][6], with mainly 3 parties involved - the browser, the _seller_ and the _buyer_. The _seller_ is the one selling the _ad space_ - they work with _publishers_ (websites) to manage an _ad inventory_ and finally, 
run the _ad auction_ based on the _bids_ that it/they have received. The _buyer_ is the one _placing the bid_ - they choose whether to _participate_ in an action. These can be an _advertiser_ or a DSP that acts on an advertiser's behalf.

Ok, lets take a deep breath and see the problem with the previous explanation I gave you - you are already asking - 'why do I need to understand all this jargon' and you are absolutely right - no one except the people implementing these things ought to 
need to know these details. But (and you knew there was a but coming up), as they say, the devil is in the details and the details of this proposal is truly mind-boggling for a non-ad-tech person.

Google talks about all the 3 parties as mentioned before in great detail and what data they can exchange and how this exchange is done in such a way that _preserves privacy_, using privacy-enhancing technologies/schemes like _k-anonymity_, 
_differential privacy_ and _trusted execution environments_. The proposal is sprinkled with these claims, interleaved with the ad-tech domain heavy terms and the tech details and thus making this proposal hugely challenging to read and understand. 
It would be really cool if they could separate out the audiences and address each from a separate point of view - the privacy focus, the ad-tech focus and, the tech implementation focus. 

The proposal also mentions numbers like 100 users (for k-anonymity).  Why 100 ? No idea.

### Topics API

Topics API [[topics-api]][7] talks about categorizing the user's browsing history into _topics_. These topics, along with other information, are used in the bidding process for _interest-based advertising_ (another new name invented to confuse us, after behavioural-advertising, targeted-advertising).
Every epoch(week) and up-to a maximum of 3 weeks, the browser will provide anyone in the ad-tech ecosystem with a list of top 5 topics for a user, based on their browsing history. Why 5 topics ? No idea.  Why 3 weeks and 1 week ? No idea.
Also, the current taxonomy of topics contains 349 topics. Why 349 ? ...you guessed it, no idea. Can they not change all these numbers ? Yes, they made up these numbers, so of course they can change them.

### Attribution Reporting API (ARA)

Attribution Reporting API [[attribution-reporting-api]][8] is about how an ad-tech company measures ad _performance_ - did a person _eventually_ buy a product based on an ad that was shown ? How can attribute a _relevant event_, post-click/post-impression of the ad, to a user's action 
on the advertised product website ? ARA, implemented in the Browser (Chrome), comes to the rescue for this problem. It talks about what is called _event-level reports_ and _aggregate/summary reports_ for _relevant events_ on the browser, based on 
the ads that the user has seen and clicked on and taking actions after that point.

This proposal again confuses the actual implementation, ad-tech focus and privacy focus in one single giant ball. It talks about using _differential privacy_ to add noise to attribution data (fixed noise level), talks about a _fixed number of reports_ per _click_, 
talks about a _fixed delay_ in reporting these reports. All these _fixed_ values are there to _preserve_ the _privacy_ of the user - can they change these _fixed_ values ? I am sure they can.

They also talk about limited _bit length_ (3 bits) for post-click attribution/conversion. Why 3 bits ?...

### Right motives - ambiguity in expression

There's a serious conflict of interest at the heart of this proposal, and it is expected - Google is trying to self-regulate its own source of revenue, by proposing this mammoth set of proposals. Don't get me wrong - Google is actually 
the only company that is innovating in this field right now - everyone else is either too small to make any difference or they are trying to build their own life-boats. Google is proposing something for everyone to get on-board, which is 
the right thing to do. 

### Conclusion

There's something to be said about getting buy-ins from as wide an audience as possible and this is hampered by how wide and complex and open-ended these proposals are at the moment.
Adding to this, a lack of sufficiently focused explanations for various audience makes this set of proposals really hard to comprehend, let alone try out. I hope that such proposals become more _accessible_ in the near future. 


[1]: <https://w3techs.com/technologies/overview/advertising> "Google Ads Market Share"

[2]: <https://gs.statcounter.com/browser-market-share> "Google Chrome Market Share"

[3]: <https://techcrunch.com/2015/06/25/the-future-of-algorithmic-personalization/> "TechCrunch - Future of algorithmic personalization"

[4]: <https://privacysandbox.com/open-web/#the-privacy-sandbox-timeline> "Privacy Sandbox for the Web"

[5]: <https://privacysandbox.com/android/> "Privacy Sandbox for Android"

[6]: <https://github.com/WICG/turtledove/blob/main/FLEDGE.md> "WICG - FLEDGE Proposal"

[7]: <https://medium.com/criteo-engineering/is-googles-topics-api-a-viable-replacement-for-interest-based-advertising-297076192bd> "Criteo R&D - Topics API"

[8]: <https://medium.com/criteo-engineering/criteos-first-look-at-the-privacy-sandbox-attribution-reporting-api-event-level-f96f42537b9c> "Criteo R&D - Attribution Reporting API (ARA"

[9]: <https://medium.com/criteo-engineering/an-update-on-fledge-chrome-testing-d0046430a3ec> "Criteo R&D - FLEDGE/Protected Audience API"
