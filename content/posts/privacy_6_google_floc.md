---
title: "Privacy - 6 - Google FLoC and what you should know"
date: "2021-06-23"
tags: ['privacy', 'FLoC', 'privacy-preserving', 'google']
---
#### Context

Google's main revenue stream is Ads and thus collecting the data that enables this business.

Everyone is online nowadays, and it (in many ways) defines *who we are* - specially what can be considered *private*.

Our window into this online world is our web browser, and we put a lot of *trust* in the companies that build these browsers to do the right thing by us and protect what is private to each of us as individuals.

As they say -

	"Nothing in life is truly free"

This holds true for the companies that build these browsers and allow us to use them for free.

What are you giving them in return ? Its obvious that it is you that you are giving up - *everything about you* that can be inferred from what you do on these browsers.

#### Third-Party Cookies - the evil no-one knew existed

Till recently, almost every browser had enabled a way called <a href="https://sriramviswanathan.com/posts/2020-09-04/privacy-3-third-party-cookies" target="_blank">third-party cookies</a> where your activity from one website to another could be tracked uniquely without you knowing it.

Recent privacy advocacy from major organizations (including Apple) brought an end to this vicious practice in almost all browsers except - guess what - Google Chrome.  They <a href="https://www.cookiebot.com/en/google-third-party-cookies/" target="_blank">still haven't disabled it</a> and even disabling it as planned (by end of 2022) wouldn't end this immediately. At least they are planning to end it, which is in the right direction.

#### Google's next step

As everyone knows, everything is about money at the end of the day and for Google, that means Ads, and they cannot do something that drastically affects that business.

What this means is that any next step that Google takes would still need to produce *data* that is *valuable* to the advertisers. This means that they should still be able to target *you* with Ads.

#### FLoC - a flawed next step

To replace third-party-cookies, Google is proposing what they are calling Federated Learning Of Cohorts (FLoC) and is in the current <a href="https://adalytics.io/blog/google-chrome-floc" target="_blank">Chromium implementation</a>.

In FLoC, instead of identifying the individual you, you will be categorized into different groups called cohorts based on your browser history - *all your browser history*.  This is done using a in-browser calculation that preserves <a href="https://en.wikipedia.org/wiki/K-anonymity" target="_blank">k-anonymity</a> and with a possible addition of <a href="https://desfontain.es/privacy/differential-privacy-awesomeness.html" target="_blank">differential privacy</a> so that you cannot be identified even if I have access to the data collected at the cohort/group level.

#### Why should you know about all this ?

If you want to understand why this is a flawed next step, the post by folks at <a href="https://brave.com/" target="_blank">Brave</a> on <a href="https://brave.com/why-brave-disables-floc/" target="_blank">why they disabled FLoC</a> is an enlightening read.

Summarizing/para-phrasing points from there -

•	FLoC shares information about your browsing behavior with sites and advertisers that otherwise wouldn't have access to that information - anonymize'd or NOT, this is breaking your trust and sharing things you never intended to.

•	Use of FLoC over time builds a database of cohorts that you (as calculated by Google) fit into and this keeps getting refreshed as time goes by - allowing anybody with this history to identify your change in behaviour over time, maybe not you as an individual but atleast how you fit into different cohorts. As <a href="https://www.eff.org/deeplinks/2021/03/googles-floc-terrible-idea" target="_blank">EFF</a> wrote, "*Your FLoC ID will be like a succinct summary of your recent activity on the Web.*"

•	One of the most jarring points is the notion of sensitive cohorts/categories - security, privacy, education, research, sexual-orientation - anything that I as an individual do not want to reveal but because of FLoC knowing all my browsing history, it can un-intentionally put me in a sensitive cohort and thus advertise and worse track me.

•	Lastly, but not the least, this is default opt-out feature. Every website has to add an <a href="https://www.getrevue.co/profile/themarkup/issues/the-tl-dr-on-google-s-and-apple-s-privacy-changes-457346" target="_blank">HTTP response header</a> to explicitly opt-out. Otherwise, everyone's opt-in. This is one of the most important points which shows how little a business like Google would care for your consent.  They know full well that very few would go into their maze of options to turn this off, even if they end up giving an option to the end-users.

#### Amazon opts-out of FLoC

In a cunning move to protect its business, Amazon has started <a href="https://digiday.com/media/amazon-is-blocking-googles-floc-and-that-could-seriously-weaken-the-fledgling-tracking-system/" target="_blank">blocking</a> FLoC.  This is interesting because before FLoC, Google could not get the data about your shopping habbits but now with FLoC, it has access to all your browsing history and thus, your visits to Amazon.

Most websites would block FLoC for privacy concerns, but this move by Amazon is all about the money and I am sure no-one expected anything different.

#### Which browser should I use then ?

Simple - use <a href="https://brave.com/" target="_blank">Brave</a> as your default browser. Its based on Chromium (the open-source part of Chrome and the core part of the browser) and the user-experience is identical to Chrome. It <a href="https://brave.com/why-brave-disables-floc/" target="_blank">disables FLoC</a> by design and is determined to protect your privacy. Almost every browser except Chrome <a href="https://www.windowscentral.com/microsoft-vivaldi-mozilla-and-other-browser-makers-turn-down-googles-floc" target="_blank">plans to not implement FLoC</a> - Mozilla, Microsoft included.

#### Opting-out of FLoC as a website

If you are a website owner/maintainer and want to respect your audience's privacy, opt-out from FLoC by sending the HTTP response header -

	Permissions-Policy: interest-cohort=()

#### Conclusion

The hope is that the brilliant minds at Google can come with something that, while destroying the existing evil violations of privacy, doesn't bring new more sinister/hidden ways that harm privacy/trust of individuals in new ways.

#### References

- <a href="https://github.com/WICG/floc#overview" target="_blank">Google's FLoC proposal/description</a>

- <a href="https://brave.com/why-brave-disables-floc/" target="_blank">Brave's stance on FLoC</a>

- <a href="https://github.com/WICG/floc#opting-out-of-computation" target="_blank">Default opt-out behaviour</a>

- <a href="https://www.eff.org/deeplinks/2021/03/googles-floc-terrible-idea" target="_blank">Why EFF thinks FLoC is a terrible idea</a>

- <a href="https://www.theverge.com/2021/4/16/22387492/google-floc-ad-tech-privacy-browsers-brave-vivaldi-edge-mozilla-chrome-safari" target="_blank">Major browsers on FLoC</a>

