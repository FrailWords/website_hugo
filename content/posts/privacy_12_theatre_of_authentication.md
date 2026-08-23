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

But the consequences are wildly different.

| What you're doing | What the OTP shows you | What's actually at stake | If someone else gets in |
|---|---|---|---|
| Logging into 1mg | 6 digits | Your prescriptions, lab results, entire medicine history | Your medical history is out there, permanently |
| Logging into Bumble | 6 digits | Your chats, photos, personal conversations, matches | Personal conversations exposed, can't take them back |
| Logging into Rapido/Uber | 6 digits | Every ride you've taken, every place you've been | Your location history is a surveillance goldmine |
| Logging into LIC/IRCTC | 6 digits | Your insurance policies, nominations, travel history, ID details | Identity details exposed, policies at risk |
| Resetting your Gmail password | 6 digits | The master key. Email controls recovery for everything else | Cascade. Email leads to bank leads to everything |
| Banking transaction | 6 digits | Moving your money to someone | Money gone. Maybe recoverable, maybe not |
| Signing a rental agreement via e-sign | 6 digits | Legally binding your signature to a contract | You're legally committed. No undo |

The 'What the OTP shows you' column is the same in every row.

"Someone else gets in" isn't hypothetical. A SIM swap, where a fraudster convinces (or bribes) a telco to reissue your number on a new SIM, hands over every OTP silently, no phone theft required. Malware on your phone (a fake courier-tracking app, a sideloaded APK) can read incoming SMS and auto-forward OTPs without you noticing a thing. Neither needs your PIN, your face, or physical access to your device.

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
- At India's scale, this is significant revenue. Every move away from SMS OTP is money off the telco's books
- Your phone number being the universal identity keeps the telco permanently in the middle of every authentication flow in the country

**Banks**
- The [2009 RBI mandate][1] for additional-factor authentication was universally implemented as SMS OTP. The cheapest possible compliance
- It was the compliance floor, and it became the ceiling. OTP was legally sufficient, so nobody built anything better
- When fraud happened, the liability often sat with the customer, not the bank. Changing that required [new directions in 2025][2], which we'll get to

**Government portals**
- Aadhaar OTP gives them an identity layer for 1.4 billion people without building per-service authentication
- Every government service (income tax, DigiLocker, PF, gas subsidy) uses the same blind six-digit ceremony for everything

**The privacy angle:** your phone number links your 1mg login to your Zerodha login to your Bumble login to your Aadhaar verification. Every service that sends you an OTP knows your number, and it's the same number everywhere. This cross-service identity graph exists at the telco layer. They can see who you authenticate with, when, and how often.

Everyone's incentives align. Not with improving the system, but with keeping it exactly as it is. The ceremony exists so everyone can say consent was given.

This is the [Theatre of Privacy][3] applied to a different ritual.

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
"at least one of the factors of authentication is dynamically created
or proven, i.e., the proof of possession of the factor, being sent as
part of the transaction, is unique to that transaction."
```

Sounds progressive.

But OTP already satisfies this. It asks for the factor to be unique to the transaction, not bound to what the transaction actually is. A six-digit code that expires in five minutes and is freshly generated each time is already "unique to that transaction" by this definition. Nothing here requires the code to change if the amount or the payee changes. Uniqueness, not meaning.

Section 9(2) says issuers must compensate customers "in full without demur" for losses from non-compliant transactions. But since OTP already complies with Section 6(b), there's nothing to trigger the clause.

#### PSD2 Article 5: What Enforceable Language Looks Like

[PSD2 Article 5][4] (Europe, 2018) requires the authentication code to be 
```text
"specific to the amount of the payment transaction and 
the payee agreed to by the payer"
```

Change either the amount or payee and the code is invalidated. That's an auditable, enforceable way to tie the OTP to its *purpose*, immediately connecting it to fraud detection and investigation.

Article 5 of the PSD2 RTS has much clearer and more enforceable language than what RBI put out. PSD2 is from 2018. RBI's directions are from 2025. There is always an opportunity to learn from your predecessors, no?

RBI does not reference PSD2 anywhere in the circular.

#### UPI Already Moved Beyond OTP

UPI uses device binding plus a UPI PIN. No OTP in the transaction path. No six-digit ritual required.

UPI ties your account to your specific device (not just your phone number) and you approve each transaction with a PIN you chose. No SMS involved. No telco in the middle. The authentication happens between your phone and your bank directly.

The replacement exists. It just didn't propagate to cards and net banking, because replacing OTP is easy on a new rail and nearly impossible on an old one.

### What About Everything Else?

Payments at least have *a* regulator that noticed. Every other sector (health, dating, transport, government, legal) uses OTP with zero regulatory pressure, zero liability framework, and zero reason to change.

#### What Would a 'Transaction-Specific' OTP Look Like in Other Sectors?

PSD2 ties the authentication code to the specific transaction, amount and payee. The code is not just 'unique', it is *meaningful*. What if we applied that same idea outside payments? Not at signup or login, but for each specific action?

| What's happening right now | What the OTP would need to be tied to | What you'd see before typing | What you see today |
|---|---|---|---|
| 1mg accessing your lab results to show a doctor | Which records, which doctor | "Sharing **your lab results** with **Dr. Mehta on 1mg**" | 6 digits |
| Rapido sharing your ride history with an insurance app | What data, who gets it | "Sharing **your ride history** with **Acko Insurance**" | 6 digits |
| Porting your SIM to a new operator | Which number, to whom | "Porting **+91-XXXXX** from **Airtel to Jio**" | 6 digits |
| Government portal pulling your Aadhaar for verification | Which service, what data | "Sharing **your Aadhaar** with **Income Tax dept** for ITR filing" | 6 digits |
| Resetting your email password from a new device | Which account, from where | "**Password reset** for **your Gmail** from a new device in Delhi" | 6 digits |
| e-Signing a rental agreement | Which document, with whom | "Signing **rental agreement** with **Rahul Kumar**, 24 months" | 6 digits |

Yes, you 'consented' to everything at signup. But that was a blanket consent. A transaction-specific OTP would tie each *action* to a specific, auditable purpose, just as PSD2 does for payments. The equivalents exist in every domain. Nobody has required them.

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

[1]: <https://rbi.org.in/Scripts/NotificationUser.aspx?Id=4844&Mode=0> "RBI 2009 AFA Circular"
[2]: <https://rbi.org.in/Scripts/NotificationUser.aspx?Id=12898&Mode=0> "RBI Authentication Directions 2025"
[3]: </posts/privacy_8_is_india_ready_for_data_protection_law/> "Theatre of Privacy"
[4]: <https://www.legislation.gov.uk/eur/2018/389/article/5> "PSD2 RTS Article 5 - Dynamic Linking"
