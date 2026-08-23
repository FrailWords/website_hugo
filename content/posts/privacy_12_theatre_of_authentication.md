---
title: "Privacy - 12 - Theatre of Authentication"
date: "2026-08-23"
tags: ['privacy', 'authentication', 'OTP', 'india', 'data-protection']
image: "/images/theatre-of-authentication-og.png"
---
<br />

### The Ritual

India runs on a six-digit ritual. It proves a specific SIM was reachable, nothing more. We know why we are typing, but not what we just agreed to.

OTPs are sprinkled all through our day. The same mechanism, a 6-digit number on your phone, whether you are booking a railway ticket, transferring lakhs of rupees to someone or just confirming if you've received a courier.

What are these numbers used for? Are they trying to make us 'acknowledge' something? 'Approve' something? 'Login into' something? If so, what is that 'something'?

### Same Ritual, Different Consequences

Same six digits for logging into 1mg and for transferring money from your bank. Same ceremony for checking your Rapido ride history and for signing a rental agreement. Same OTP to get into Bumble (where you have your chats, your photos, pictures of your cats and dogs) and to reset your email password.

The stakes are wildly different. The OTP never changes.

| What you're doing | What the OTP shows you | If someone else gets in |
|---|---|---|
| Logging into 1mg | 6 digits | Your medical history is out there, permanently |
| Banking transaction | 6 digits | Your account and spending history exposed, money moved without your knowledge |
| Signing a rental agreement via e-sign | 6 digits | Your identity and signature tied to a contract you didn't choose |

Same six digits, whether it's a courier update or your life savings.

A TOTP app at least names the service the code is for. Face ID shows you which app is asking, unlocking your phone or approving a payment. SMS OTP shows you neither. Just the number.

"Someone else gets in" isn't hypothetical. A SIM swap, where a fraudster convinces (or bribes) a telco to reissue your number on a new SIM, hands over every OTP silently, no phone theft required. Malware on your phone (a fake courier-tracking app, a sideloaded APK) can read incoming SMS and auto-forward OTPs without you noticing a thing. Neither needs your PIN, your face, or physical access to your device. The same blindness that fails you also helps them. Whoever holds the code doesn't need to know what it's for either.

The OTP doesn't differentiate between a login, an approval and an acknowledgement. Whether you're getting *into* an app, *approving* a transaction, or *acknowledging* a delivery, the ceremony is identical. Six digits, type, move on.

### The 'Identity' Layer That Is Not Your Identity

India built its identity layer on the mobile number. Your phone number became your username and the OTP became your password.

In many of these cases, this is not 2FA (two-factor authentication). This *is* the login. There is no first factor. The OTP is doing all the work, and it's doing it blind.

If someone else has access to your phone, they are 'you' as far as any of these apps are concerned.

### Presence != Intent

OTP proves presence, not intent. Six digits carry zero meaning. It never gives the person looking at it anything to 'think' about.

You could say there's a context in which the person is typing the OTP and that's clear. But this 'context' is a mental model, not an enforced contract. There is nothing stopping the app or website from using your authentication to do five other things you never wanted. Where is the 'intent' here?

### Who Benefits?

If OTPs suddenly carried real information about what you're authorizing, who would lose?

**Apps and platforms**
- Your phone number is their username. No need to build proper account management or recovery flows
- No need to specify what data the user is granting access to. The OTP doesn't carry scope, so the app doesn't have to declare it
- One integration (SMS gateway) covers login, re-authentication and account recovery. Cheap, universal, done

**Telcos**
- Every OTP is an A2P SMS (Application-to-Person), a bulk message that the bank or app *pays the telco for*, per message
- Multiply that by how many OTPs India sends daily, and it adds up. Every move away from SMS OTP is money off the telco's books
- Your phone number being the universal identity keeps the telco permanently in the middle of every authentication flow in the country

**Banks**
- The [2009 RBI mandate][1] for additional-factor authentication was almost always implemented as SMS OTP. The cheapest possible compliance
- It was the compliance floor, and for most banks it became the ceiling. OTP was legally sufficient, so there was little incentive to build anything better
- When fraud happened, the liability often sat with the customer, not the bank. Changing that required [new directions in 2025][2], which we'll get to

**Government portals**
- Aadhaar OTP gives them an identity layer for 1.4 billion people without building per-service authentication
- Every government service (income tax, DigiLocker, PF, gas subsidy) uses the same blind six-digit ceremony for everything

Everyone's incentives align. Not with improving the system, but with keeping it exactly as it is. The ceremony exists so everyone can say consent was given.

This is the [Theatre of Privacy][3] applied to a different ritual.

### One Number, Every Service

Every OTP, no matter which app sent it, passes through the same telco pipe, tied to the same number, stamped with the same timestamp. String enough of these together and the telco holds something no single app has: a record of which services you use and when, across all of them, without opening a single message.

Encryption on its own wouldn't fix this. It can hide what a message says. It can't hide which app sent it or when, and that's the part that builds this record. And for the record, that kind of encryption isn't even coming for OTPs. RCS, the standard meant to eventually replace SMS, added real end-to-end encryption in 2026, but only for person-to-person chats. The [spec itself][5] says plainly:
```text
"SMS messages are not subject to End-to-End Encryption (E2EE)."
```
The section covering business Chatbots, the category OTPs fall under, carries no encryption requirement of its own either. The actual fix isn't encryption. It's removing the telco from the path, which is exactly what UPI already does.

### The Habituation Trap

We go through our days receiving and typing so many OTPs that, at some point, it becomes a mind-numbing activity. We don't pause to think about what we are reading, who we are reading it to or, for that matter, typing it immediately when we see that 'fill in the blanks' UI. It's almost like we're over-eager to get it done, given most of these OTPs expire in seconds.

Banks send them for every login, transaction, and profile change. Government portals demand one on every single visit, no exceptions, for income tax, LIC, DigiLocker, IRCTC. Travel apps like Rapido and Uber send them regularly, often in-app rather than SMS. Health and pharmacy apps like 1mg send one on every login. Dating apps like Bumble send one on every login or reinstall.

The OTPs that matter most (banks, government, health) are also the ones we type most often. Every routine banking OTP is a rehearsal for the fraud OTP.

### Our Phones Don't Help

Even if someone added more 'context' to the OTP message (payee, amount, scope, purpose), we would never see it.

Both Android and iOS phones automatically parse the OTP from the SMS and show it right there in the app you're using. The actual message is never opened. We just tap the auto-suggested digits and move on.

Most sectors have no regulator looking at any of this. Payments do. RBI had a recent regulation/directive around online payment authentication.

### Financial Sector as a Special Case

Not all OTPs are created equal. One sector where almost all OTP use-cases can be considered *critical* is finance and our interactions with financial institutions.

#### RBI's Missed Opportunity: Sounding Progressive Without Making Progress

The RBI issued new authentication [directions in September 2025][2]. Seven pages, that's it, that too bulleted and double spaced. When I initially read it, I thought the actual document might be a link somewhere but this *was* the actual 'directions' document.

The whole thing has no concrete detail on what financial institutions and banks *must* do as an improvement over OTP. Worst part, it leaves the door wide open for them to continue using OTP.

Section 6(b) requires:
```text
"at least one of the factors of authentication is dynamically created or proven, i.e., the proof of possession of the factor, being sent as part of the transaction, is unique to that transaction."
```

Sounds progressive.

But OTP already satisfies this. A six-digit code that expires in five minutes and is freshly generated each time is already "unique to that transaction" by this definition. What it doesn't do is verify *what* that transaction is. The code proves a transaction happened and someone was present for it. It says nothing about the amount or the payee. If those details get altered somewhere between what you saw and what actually reaches the bank, say by malware on your device, the same OTP still goes through. Uniqueness, not meaning.

Section 9 says issuers must compensate customers "in full without demur" for losses from non-compliant transactions. But since OTP already complies with Section 6(b), there's nothing to trigger the clause.

#### PSD2 Article 5: What Enforceable Language Looks Like

[PSD2 Article 5][4] (Europe, 2018) requires the authentication code to be 
```text
"specific to the amount of the payment transaction and the payee agreed to by the payer"
```

The code itself is generated from the amount and payee, not something you see, something the bank checks. If either gets altered after you approve and before it reaches the bank, the code no longer matches and the bank can catch it. That's an auditable, enforceable way to tie the OTP to its *purpose*: not proof someone was merely present, but proof the transaction that got approved is the one that got executed.

Article 5 of the PSD2 RTS has much clearer and more enforceable language than what RBI put out. PSD2 is from 2018, RBI's directions are from 2025. The circular doesn't reference PSD2 anywhere. Whether that's because nobody looked, or because they looked and chose not to adopt it, the result is the same: weaker language than what already existed.

#### UPI Already Moved Beyond OTP

UPI ties your account to your specific device, not just your phone number, and you approve each transaction with a PIN you chose. No OTP in the transaction path, no SMS, no six-digit ritual. The authentication happens between your phone and your bank directly.

The replacement exists. It just didn't propagate to cards and net banking, because replacing OTP is easy on a new rail and nearly impossible on an old one.

### What About Everything Else?

Payments at least have *a* regulator that noticed, even if what they did about it fell short. Health, dating, transport, government, legal: none of them have anything like RBI's or PSD2's authentication rules. Nobody has required OTPs to mean anything in those sectors either.

#### What Better Would Actually Look Like

The fix isn't a smarter OTP message. SMS OTP is a blind text sent by a telco. There isn't a version of that pipe that can reliably carry "sharing your lab results with Dr. Mehta" for every provider without rebuilding how A2P SMS templates work.

UPI already shows the alternative. Before you enter your UPI PIN, the app shows you the payee and the amount, on the same screen where you approve. The approval is tied to that specific detail, not sent blind over a separate channel.

The same idea could apply outside payments. 1mg could show "Share your lab results with Dr. Mehta?" on its own screen before asking you to approve, the same way UPI shows the payee before the PIN. Nothing about SMS or telcos needs to change for that. Nobody is required to build it.

### Final Thoughts

Our beloved six-digit OTP ritual will outlive the RBI directive. Not because alternatives don't exist (UPI proved they do), but because everyone in the chain, apps, telcos, banks, government, has built their system on the assumption that the user doesn't need to understand what they're approving.

---

*This post is part of my ongoing Privacy series. Previously: [Theatre of Privacy][3] explored how notice-and-consent became a performance nobody watches.*

---

**References**

1. [RBI 2009 AFA Circular][1]
2. [RBI Authentication Directions 2025][2]
3. [Theatre of Privacy][3]
4. [PSD2 RTS Article 5 - Dynamic Linking][4]
5. [GSMA RCC.71 - RCS Universal Profile Service Definition Document, v4.0][5]

[1]: <https://rbi.org.in/Scripts/NotificationUser.aspx?Id=4844&Mode=0> "RBI 2009 AFA Circular"
[2]: <https://rbi.org.in/Scripts/NotificationUser.aspx?Id=12898&Mode=0> "RBI Authentication Directions 2025"
[3]: </posts/privacy_8_is_india_ready_for_data_protection_law/> "Theatre of Privacy"
[4]: <https://www.legislation.gov.uk/eur/2018/389/article/5> "PSD2 RTS Article 5 - Dynamic Linking"
[5]: <https://media.gsma.com/assets/2026/rcs/RCC.71+v4.0.pdf> "GSMA RCC.71 - RCS Universal Profile Service Definition Document, v4.0"
