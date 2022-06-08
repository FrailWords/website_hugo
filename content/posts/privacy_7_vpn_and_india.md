---
title: "Privacy - 7 - VPN Providers and India - what's going on ? "
date: "2022-06-08"
tags: ['privacy', 'vpn', 'india']
---
<br />

### Context

The Indian government's cybersecurity wing is called Computer Emergency Response Team (<a href="https://cert-in.org.in/" target="_blank">CERT-In</a>), under the Ministry of Electronics and Information Technology (MeitY).

In April'22, CERT-In <a href="https://www.pib.gov.in/PressReleasePage.aspx?PRID=1820904" target="_blank">published</a> _directions_ to address gaps in _incident analysis_.  Most controversially, this includes the following - 

    Maintenance of logs of ICT systems; subscriber/customer registrations 
    details by Data centers, Virtual Private Server (VPS) providers, 
    VPN Service providers

and

    These directions shall enhance overall cyber security posture and 
    ensure safe & trusted Internet in the country.

Due to the very _concise_ nature of this document, CERT-In <a href="https://www.cert-in.org.in/PDF/FAQs_on_CyberSecurityDirections_May2022.pdf" target="_blank">published another document</a> in May'22 to _clarify_ why they are asking VPN providers to share their customer details and also log all their activity.

Quote from that clarification -

    The right to informational privacy of individuals is not affected by these Cyber
    Security Directions of 28.04.2022. These directions do not envisage seeking of
    information by CERT-In from the service providers on continuation basis as a standing
    arrangement. CERT-In may seek information from service providers in case of cyber
    security incidents and cyber incidents, on case to case basis, for discharge of its statutory
    obligations to enhance cyber security in the country. The service providers are bound to
    protect the users’ information by following reasonable security practises and
    procedures.

Fair enough. This is the _trust me, I am the govt'_ promise. Sounds very familiar from many other parts of the world, doesn't it ?    

<br />

### Does this apply to corporate VPNs ? 

Quote again -

    No. For the purpose of this direction, VPN Service provider refer to an entity that
    provide “Internet proxy like services” through the use of VPN technologies, standard or
    proprietary, to general Internet subscribers/users.

This explanation opens up so many questions, I am not even sure what to say. Are corporate VPNs not "Internet proxy like services" ? ?   

<br />

### Let's see what a VPN does first

#### Before VPN - why do we need VPN ? - simple explanation

Usually, when you are browsing a website, the path of the request is -

    You -> Your ISP (internet service provider) -> Website

and all the way back. This whole communication happens over a protocol like HTTP/HTTPS.  

When the final website gets your request, it knows that you are in India (based on your original request IP address) and so, it will serve content based on its policies for India as a country.

Some websites are blocked in India and so when you try to access it, you'll not be able to reach the website.  Same goes for when you are in another country and trying to access an Indian website like Hotstar.

#### Say hello to VPN :)

I am sure almost everyone uses VPN nowadays to access websites that are blocked in India. For the other way around - when you are abroad and want to access India only sites.

What a VPN does is that they provide a server in some other part of the world to act as a _intermediary_ on our way to access the website.

So, our path becomes -

    You -> Your ISP -> VPN Server -> Website

and all the way back.  The biggest difference is that, now, there's an _encrypted_ connection between you and the VPN server and the VPN server _acts on behalf of you_ to access the website.

The VPN server can be in any country that _allows_ the access to the site you are trying to access.

#### What's happened here ? 

Now, the ISP can no longer _see_ where you are going - it just sees that you are talking to any of the VPN servers and so the ISP cannot track your requests. It can see your IP but can't really do anything with it, as the final website
details are _encrypted_. Only the VPN server can _decrypt_ and see where you are trying to go and what you are requesting.

<br />

### Why are the VPNs complaining about the _directions_ from CERT-In ?

What CERT-In is asking VPNs to do is 2 things mainly -

1. Log all the activity of its customers and provide it when requested
2. Share all the subscriber data 

Implementing both these points doesn't really _break_ VPN functionality as such but then again, it damages one thing - **privacy**.

VPN is not just about being able to access blocked content or content from other countries, its also about doing so in a way that our privacy is not out there for the taking.

<br />

### Technical segway - "what about HTTPS?" you might ask

_HTTPS_ is an improvement over _HTTP_ that _encrypts_ all communication between the user and the website.

What it cannot do - _it can’t be used to conceal your identity with an anonymous IP address_.

Here's where VPNs come in. VPN is an additional layer of security and privacy over HTTPS as a secure way of communication.

### Can you trust VPNs ? 

Quoting directly from <a href="https://www.schneier.com/blog/archives/2021/06/vpns-and-trust.html" target="_blank">Schneier on Security - VPNs and Trust</a> reference -

    We don’t talk about it a lot, but VPNs are entirely based on trust. 
    
    As a consumer, you have no idea which company will best protect your privacy.

    You don’t know the data protection laws of the Seychelles or Panama.

    You don’t know which countries can put extra-legal pressure on companies 
    operating within their jurisdiction. 

    You don’t know who actually owns and runs the VPNs.

    You don’t even know which foreign companies the intelligence agencies have 
    targeted for mass surveillance.

    All you can do is make your best guess, and hope you guessed well.

### How are VPN providers reacting to these _directions_ ? 

ExpressVPN and SurfShark are two providers that have acted by closing down their servers in India.   

NordVPN has also warned that it will be removing physical servers from India if the directives are not reversed.

### How will this ultimately affect VPN providers in India ?

India is a huge market for these providers. Just removing their servers from India only does one thing - every request has to go _outside_ of India another country.

This adds latency and probably cost as well to them but that's about it from what I can understand.

Quote from the NordVPN PR person -

    “We will remove our servers as there will be no other way to stay in India,
    while preserving the privacy of our customers and integrity of our service.”

The most this will affect is an _Indian_ VPN provider who only have servers in India.  All the global VPN providers would still be able to route requests to outside servers.

#### Conclusion

In a democratic nation like ours, these govt institutions should be held to parliamentary debate and discussion and be treated like how traditional bills are passed through majority.

We need to hold accountable the mandate given to an organization like CERT-In, have it presented for public feedback and also put up for debate and not just passed on as a 'directive'.

These decisions affect the basic rights of every citizen and should be treated so.

#### References

- <a href="https://kinsta.com/blog/how-does-a-vpn-work/" target="_blank">How does a VPN work ?</a>

- <a href="https://www.purevpn.com/blog/https-vs-vpn/" target="_blank">VPN vs HTTPS</a>

- <a href="https://www.theregister.com/2022/06/08/india_it_regulation_criticism/" target="_blank">India IT regulation criticism</a>
 
- <a href="https://www.moneycontrol.com/news/business/vpn-service-providers-slam-cert-in-directions-on-maintaining-logs-8480081.html" target="_blank">VPN providers against the CERT-In directions</a>
