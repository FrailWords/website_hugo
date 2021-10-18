---
title: "Privacy - 4 - End-To-End Encryption And Your Messaging App"
date: "2020-09-09"
tags: ['end-to-end encryption', 'privacy', 'security', 'encryption', 'signal', 'whatsapp', 'telegram']
---
#### Disclaimer
I am going to simplify a lot of concepts to a great deal so as to reach a broader audience and not make it very very tech oriented.

#### Encryption?
In simple terms, it’s about ‘obfuscating’ or 'encoding' or converting plain text information into a form that is not human readable and is based on mathematical properties/proofs.

#### Some Basics Principles
Basic aims which any good encryption protocol tries to achieve -
*   [pseudo-randomness](https://en.wikipedia.org/wiki/Pseudorandomness) - make the result appear random, even though the process which was used to encrypt is completely deterministic and not random.
*   [non-repudiation](https://en.wikipedia.org/wiki/Non-repudiation) - verification of data integrity as well who sent it and to be able to verify that the message was not tampered with.
*   [perfect-forward-secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) - even if the key of a single data exchange is compromised, it shouldn’t allow the attacker to read either past or future messages.

There are definitely some principles I’ve missed but these are some of the most basic ones.

All the end-to-end encrypted messaging apps today claim to do all these.  I say _claim_ because there are some messaging apps which are closed-source and so they are definitely not openly verifiable by any independent agency/person.


#### What does it mean to ‘break an encryption’ ?
To break an encryption means that, given an encrypted/encoded piece of information, you can access the original unencrypted data through some computation.  The better the encryption, the more the processing power (and consequently time) that is required to ‘break’ the encryption.

#### End-To-End Encryption ?
Message flow in a messaging app has the following participants -

    Sender <-----------> Server <-----------> Recepient

By end-to-end encryption, it means that the **data+identity** of each message is safe from prying eyes all the way from the sender to the **designated receiver**.  The server, who is in the middle, SHOULD also NOT be able to read ANY message and also NOT be able to read who is sending the message to whom.  We’ll come back to this point.

#### Symmetric And Asymmetric Encryption
We need to understand this before we go any further -

*   <u>Symmetric encryption</u><br />
    <br />
    When it’s the same ‘shared secret’ or ‘key’ that encrypts and decrypts the data.  A real world example would be that you and your friend meet at a coffee place and make two copies of the same key and then whenever you want to send something secret in a locked box, you can lock it with this key and your friend can unlock it.
    <br />
    <br />The biggest assumption/problem with this form of encryption is that you need to publicly meet to exchange the keys before this can work.
    <br />
    <br />
*   <u>Asymmetric encryption</u>
    <br />
    <br />
    When you use a mathematically proven algorithm/property which generates two keys - one a <strong>public</strong> key and the other a <strong>private</strong> key.  The public key is shared with the world and the private one is _never_ to be shared. The public and private key together form a “key pair” where something locked by one can be unlocked by the other.  This is awesome because we now do not need to meet in person :) and can use proven mathematical properties to do our secret communication.

Most protocols/frameworks for securing data transport use both these methods together as they have their own role to play in a typical protocol/algorithm which provides secure communication on the internet.  One common example of end-to-end encryption on the web is TLS or Transport Layer Security which is the basis for HTTPS that you see in your browser normally.  Signal protocol also uses a combination of both these methods.

#### Messaging == Identity + Content
The key point to understand is that messaging is not just about encryption but also identity of the individuals that are participating -
*   What is being communicated or shared
*   Who is sharing with whom

This differentiation is important because of how the different messaging apps behave.

#### Signal Protocol - WhatsApp vs Signal
This website - [https://www.securemessagingapps.com/about/](https://www.securemessagingapps.com/about/) does an amazing job of comparing all the messaging apps and their security/privacy credentials.

In brief, Signal is a protocol/framework developed by Open Whisper Systems (now [Signal](https://www.signal.org/)).  They have their own app, which is Signal and they also open-sourced this framework that is now used by WhatsApp, Facebook Messenger and others.

#### Criticism Of WhatsApp - Why ?
There are many reasons why WhatsApp is criticised (as compared to Signal), primary reason being that Signal is completely open-source and openly auditable, whereas WhatsApp is closed source.  A couple of other reasons is that WhatsApp doesn’t protect _metadata_ (i.e. who’s talking to whom, call duration, message type etc.) and only protects the message itself.  Here’s the [WhatsApp security whitepaper](https://scontent.whatsapp.net/v/t61.22868-34/68135620_760356657751682_6212997528851833559_n.pdf/WhatsApp-Security-White-paper.pdf?_nc_sid=2fbf2a&_nc_ohc=N5Xb4MV6oUAAX84EgZP&_nc_ht=scontent.whatsapp.net&oh=05a3c8e1754062235ee588e1fda9eef1&oe=5F5A9E35) for more info.

#### Privacy is subjective and contextual
What one wants to protect from prying eyes is very contextual and privacy is a trade-off at the end of the day.  And there are many people interested in this data - government, corporations, hackers - everyone has a different motive and incentive.

#### More Privacy == Less Social ?
This is the conundrum that is put forth towards the customer - if I use Signal, none of my ‘friends’ are on Signal.  If I use WhatsApp, everyone’s there.  Why is this the case? They have the same ease of installation and possibly same features as well.

#### Corporations, Governments == A Mutually Beneficial Relationship
The only reason I can see why certain apps are more popular than others is a relationship of mutual benefit - companies like Facebook benefit from lax data security policies because they can collect more data and have more influence over our everyday decisions/thought process.  They in-turn benefit the government through the revelations in case of [Cambridge Analytica](https://en.wikipedia.org/wiki/Facebook%E2%80%93Cambridge_Analytica_data_scandal), where user-data collected by Facebook could be used for electoral purposes without any consent from the users.

A direct consequence of this relationship is more investment and more visibility.  For e.g. government services using WhatsApp for official purposes in India.  In comparison, EU commission staff were directed this year in February to use Signal for official purposes.

#### Final Thoughts
Using a certain app is a personal decision and this post is just to say that all messaging apps are not created equal in terms of privacy/security and depending on what you are communicating, weigh the use of one app over another.

#### Donate To Wikipedia
I am sure each one of us has learnt a lot from Wikipedia articles.  It is an invaluable resource and a lot of what I’ve written in this post comes from there.  So, if you can, please donate to Wikimedia :) - [https://donate.wikimedia.org/wiki/Ways_to_Give](https://donate.wikimedia.org/wiki/Ways_to_Give)

#### Further Readings
*   Site comparing all security/privacy features - [https://www.securemessagingapps.com/about/](https://www.securemessagingapps.com/about/)
*   Diffie-Hellman Key Exchange - [https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) - used in almost all messaging apps and the web and in TLS.
