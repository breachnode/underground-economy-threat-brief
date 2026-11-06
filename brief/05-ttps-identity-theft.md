# 05 — TTPs: Identity Theft

Identity theft is the use of another person's personal information to open accounts, claim benefits, or commit fraud in their name. Per FBI IC3 reporting, it is one of the most-reported categories of internet crime, and it differs from account takeover in an important way: account takeover steals access to an existing account, while identity theft creates new activity — new accounts, new credit lines, new claims — that the victim may not discover for months. Each entry follows the brief's shape — what it is, what it looks like to a defender, what stops it. Full control mapping with MITRE ATT&CK IDs is in `07-controls.md`.

## TTP 5.1 — Synthetic identity fraud

**What it is.** The creation of a new identity by combining real and fabricated information — for example, a real Social Security number (often a child's, or one that has never been used for credit) combined with a fabricated name, date of birth, and address. The synthetic identity is then used to open accounts and build a credit history over time: small credit lines are opened, paid on time, and gradually increased, until the identity is trusted enough to take out a large loan or credit line — which is never repaid. Per Federal Reserve and vendor fraud reporting, synthetic identity fraud is one of the fastest-growing forms of identity fraud, and one of the hardest to detect, because there is no single victim who reports it: the identity itself is partly fictional, so no one person receives the bill.

The key fact for a defender is the timeline. Synthetic identity fraud is a long game — the identity is typically "farmed" for months before the fraud occurs. That long timeline is also the defender's opportunity: the farming stage leaves a trail of thin, inconsistent, and high-velocity applications.

**What it looks like to a defender.**

- A new-account application with a thin credit file: little or no credit history, a recently issued Social Security number, or a number that has never appeared in credit data before.
- Identity data that does not fit together: the name, date of birth, and address combination has no history, or the same Social Security number appears with different names across applications.
- Multiple applications from the same device, IP address, or physical address, each with a different name — the "farm" being built.
- An account that is used carefully for months — small purchases, on-time payments — and then suddenly maxes out its credit or takes a large cash advance.
- A Social Security number that belongs to a minor, a deceased person, or someone who has never applied for credit, appearing on a new application.
- Applications that fail knowledge-based verification or identity-document checks in inconsistent ways — sometimes passing, sometimes failing, as if the applicant is testing what the system accepts.

**What stops it.**

- Check new applications against identity-verification services that flag thin files, recently issued numbers, and numbers associated with minors or deceased persons.
- Look for velocity: multiple applications from one device, address, or phone number in a short window, even if each application is individually plausible.
- Use consortium data — shared fraud intelligence from other institutions — to see whether the same identity elements have appeared elsewhere under different names.
- Monitor accounts for the "bust-out" pattern: a long period of careful use followed by sudden maxing-out. A credit line that is used responsibly for months and then drained in days is a strong signal.
- Require stronger verification for thin-file applicants: document verification, or a step that cannot be passed with public data alone.

*ATT&CK mapping: no direct ATT&CK technique covers synthetic identity creation. The closest mappings are T1585.001 Social Media Accounts and T1586 Compromise Accounts for the identity elements, and T1657 Financial Theft for the monetization. The mapping notes this gap in `07-controls.md`.*

## TTP 5.2 — Full-identity takeover

**What it is.** The use of a victim's complete personal information — name, date of birth, address, Social Security number, and often answers to security questions — to impersonate the victim across many services at once: opening new credit accounts, taking over existing accounts, filing tax returns, and claiming benefits. Unlike synthetic identity fraud, full-identity takeover has a real victim, and the victim usually discovers it when a bill, a collections notice, or a denied application arrives. Per FBI IC3 reporting, the data for full-identity takeover typically comes from breaches, phishing, or malware, and the fraud is often discovered months after it began.

**What it looks like to a defender.**

- A new credit application in the name of an existing customer, with a different address, phone number, or email than the one on file.
- Multiple new accounts opened in a short window under the same identity, across different institutions.
- An existing customer's contact details changed, followed by new-account applications using the old identity with the new contact details.
- A customer reporting that they received a bill, a collections notice, or a credit inquiry they did not authorize.
- An application that passes identity verification but fails behavioral checks — for example, the device, IP address, and application timing do not match the real person's history.

**What stops it.**

- Identity verification that checks more than static data: device, behavior, and history, not just name and Social Security number.
- Alerts to the customer whenever a new account is opened in their name, or whenever their contact details change — sent through a channel the attacker has not compromised.
- Credit freezes and fraud alerts: encourage customers to freeze their credit files, and honor fraud alerts on applications.
- Cross-institution fraud intelligence: share and consume fraud signals so that an identity flagged at one institution is checked at the next.
- Monitor for the combination of a contact-detail change and a new-account application in the same short window — that pairing is one of the strongest signals of full-identity takeover.

*ATT&CK mapping: T1589 Gather Victim Identity Information (T1589.001 Credentials, T1589.003 Employee Names — and by extension personal data); T1586 Compromise Accounts; T1657 Financial Theft. See `07-controls.md`.*

## TTP 5.3 — Tax-refund and benefits fraud

**What it is.** The use of a stolen identity to file a fraudulent tax return and claim the refund, or to claim government benefits — unemployment, disaster relief, or other programs — in the victim's name. Per IRS and FBI reporting, tax-refund fraud is seasonal (it peaks during tax season) and benefits fraud spikes during disasters and economic crises, when new programs are launched quickly and verification is loosened to get money out fast. The fraud is discovered when the real person files their own return or applies for their own benefits and is told they already have.

**What it looks like to a defender.**

- A tax return or benefits claim filed very early in the season, before the real person has filed.
- A refund or benefit directed to a bank account, prepaid card, or address that does not match the identity on file.
- Multiple claims filed from the same device, IP address, or bank account, under different names.
- A claim with identity data that is correct but with contact and payment details that are new and inconsistent with the person's history.
- A sudden spike in claims for a new program, with a high rate of claims that fail verification or are later reversed.

**What stops it.**

- Verify the payment destination, not just the identity: a refund going to a new bank account or prepaid card is a stronger signal than a mismatch in the name field.
- Hold first-time refunds or benefit payments for verification when the payment details are new.
- Cross-check claims against known fraud patterns from previous seasons and other programs — the same identities and payment destinations recur.
- Encourage victims to report quickly: the faster a fraudulent claim is reported, the faster the payment can be stopped or recovered.
- For program administrators: build verification into the program launch, not after the fraud appears. The programs that lose the most money are the ones that prioritized speed over verification at launch.

*ATT&CK mapping: T1657 Financial Theft. Tax and benefits fraud has no direct ATT&CK technique; it is a business-process fraud, and the mapping notes this in `07-controls.md`.*

## TTP 5.4 — New-account fraud (general pattern)

**What it is.** The umbrella pattern behind most identity-theft monetization: opening a new account — a bank account, a credit card, a phone contract, an e-commerce account — using stolen or synthetic identity data. New-account fraud is the step where the stolen identity becomes usable money or services, and it is the step where a financial institution or merchant has its last chance to catch the fraud before the account is used. Per vendor fraud reporting, new-account fraud is the fastest-growing category of identity fraud, because it does not require taking over an existing account — only passing the onboarding check.

**What it looks like to a defender.**

- A spike in new-account applications, especially applications that fail verification at a higher rate than normal.
- Applications with thin or inconsistent identity data, arriving from the same device or IP range.
- New accounts that are used immediately and aggressively — a large purchase or cash advance within hours of account opening — rather than gradually.
- New accounts whose first transaction is a high-value purchase, a cash advance, or a transfer to another account.
- Contact details (email, phone) that were created very recently, or that appear on many different applications.

**What stops it.**

- Step up verification for new accounts: document checks, device checks, and behavioral checks, not just static identity data.
- Velocity rules on applications: limit how many applications can come from one device, IP address, or phone number in a window.
- Monitor new accounts for the first 30 days with tighter fraud rules than established accounts — the first month is when most new-account fraud happens.
- Check the age of the contact details: an email address created yesterday, used on a credit application today, is a signal.
- Use consortium and shared fraud data to check whether the identity elements in the application have been seen in fraud elsewhere.

*ATT&CK mapping: T1585 Establish Accounts; T1657 Financial Theft. See `07-controls.md`.*

## How identity theft fits the lifecycle

| Lifecycle stage | Identity-theft TTP | Defender's best move |
|---|---|---|
| Theft | Full-identity data from breaches/phishing (5.2) | Breach monitoring, customer alerts |
| Validation | New-account applications (5.4) | Verification, velocity rules, consortium data |
| Monetization | Synthetic identity bust-out (5.1) | Thin-file checks, bust-out pattern monitoring |
| Monetization | Tax-refund and benefits fraud (5.3) | Payment-destination verification, hold first payments |

Next: `06-ttps-mules-cashout.md` — money mules, reshipping, and cash-out networks.
