# 07 — Defensive Control Map

This file is the point of the brief. Every TTP in files `03` through `06` is mapped here to the controls that stop or detect it, with MITRE ATT&CK technique IDs where a mapping exists. The map is organized two ways: first a master table (TTP → controls → ATT&CK), then a control-family reference with implementation notes.

## A note on ATT&CK mapping — read this first

MITRE ATT&CK is an enterprise-intrusion taxonomy: it describes how an attacker breaks into and moves through a corporate network. Fraud techniques map to it imperfectly, for three reasons:

1. **Fraud is often not intrusion.** Card testing, synthetic identity fraud, and refund abuse do not involve breaking into a system — they abuse legitimate functionality (a payment page, an application form, a refund process). ATT&CK has limited coverage for abuse of legitimate business processes.
2. **The closest technique is often generic.** Many fraud TTPs map to T1657 Financial Theft, which is accurate but not specific — it tells a defender that money was stolen, not how.
3. **Some fraud has no ATT&CK equivalent at all.** Synthetic identity creation, money-mule recruitment, and reshipping have no ATT&CK technique. Forcing a mapping would be dishonest tradecraft.

So this map follows a rule: **map to the closest real technique, and say plainly where no mapping exists.** A hiring manager who reads this map should see discipline, not box-ticking. Where a control is a general defensive principle rather than an ATT&CK-mapped countermeasure, it is labeled as such.

## Master control table

| TTP | ATT&CK technique(s) | Controls that stop or detect it |
|---|---|---|
| 3.1 Card testing | T1657 Financial Theft; T1071.001 Web Protocols | Velocity limits; decline-rate monitoring; AVS + security-code checks; bot detection / CAPTCHA; 3-D Secure step-up; device fingerprinting |
| 3.2 Stolen-card purchases | T1657 Financial Theft; T1078 Valid Accounts | Address-mismatch review; AVS + security-code checks; 3-D Secure step-up; device fingerprinting; IP reputation; shared fraud-intelligence feeds; shipment holds |
| 3.3 Refund / chargeback abuse | T1657 Financial Theft (no direct technique for chargeback abuse) | Per-customer refund tracking; return-before-refund; delivery evidence; chargeback disputes with proof; feed confirmed fraud back into validation controls |
| 3.4 Gift-card / digital-goods cash-out | T1657 Financial Theft | Purchase limits for new accounts; step-up auth for large purchases; activation / delivery delay; redemption-velocity monitoring; per-account daily caps |
| 4.1 Credential stuffing | T1110.004 Credential Stuffing; T1078 Valid Accounts | MFA (strongest control); rate limiting + progressive delays; breached-password screening; bot detection; login anomaly alerts; passkeys where feasible |
| 4.2 Phishing kits / brand impersonation | T1566.002 Spearphishing Link; T1566.003 Spearphishing via Service; T1583.001 Domains | DMARC + SPF + DKIM with reject policy; brand / lookalike-domain monitoring; documented takedown process; customer education; MFA (incl. phishing-resistant MFA) |
| 4.3 Session hijacking | T1550.002 Web Session Cookie; T1539 Steal Web Session Cookie; T1078 Valid Accounts | Session binding to device / network; short sessions for sensitive actions; step-up auth for high-risk actions; user-visible session management; HttpOnly + Secure + SameSite cookie flags |
| 4.4 Password-reset / recovery abuse | T1078 Valid Accounts; T1566.004 Spearphishing Voice | Deprecate SMS-only recovery; strong verification before recovery-detail changes; rate-limit resets; support verification scripts; second-channel change notifications |
| 5.1 Synthetic identity fraud | No direct technique; closest: T1585 Establish Accounts; T1657 Financial Theft | Thin-file checks; velocity rules; consortium / shared fraud data; bust-out pattern monitoring; stronger verification for thin-file applicants |
| 5.2 Full-identity takeover | T1589 Gather Victim Identity Information; T1586 Compromise Accounts; T1657 Financial Theft | Verification beyond static data; customer alerts on new accounts / detail changes; credit freezes and fraud alerts; cross-institution intelligence |
| 5.3 Tax-refund / benefits fraud | T1657 Financial Theft (no direct technique) | Payment-destination verification; hold first-time payments; cross-program fraud-pattern checks; build verification into program launch |
| 5.4 New-account fraud | T1585 Establish Accounts; T1657 Financial Theft | Stepped-up verification for new accounts; application velocity rules; 30-day enhanced monitoring; contact-detail age checks; consortium data |
| 6.1 Money mules | T1657 Financial Theft (no direct technique for mule activity) | Transaction monitoring tuned to mule patterns; holds on large first inbound transfers; behavioral baselines; mule-detection models; law-enforcement reporting |
| 6.2 Reshipping networks | T1657 Financial Theft (no direct technique) | Shipping-address checks against shared fraud data; first-order holds; address verification; shipment delays for flagged orders |
| 6.3 Gift-card / crypto conversion | T1657 Financial Theft; T1071.001 Web Protocols | Step-up auth for large purchases; new-account purchase limits; purchase-then-transfer monitoring; activation delays |
| 6.4 Layering | T1657 Financial Theft (no direct technique) | Network / graph analysis; cross-institution intelligence sharing; multi-hop movement monitoring; full-chain reporting |

## Control families — implementation notes

The controls in the table fall into nine families. Each family is described here with what it is, where it applies, and what to watch out for.

### C1 — Multi-factor authentication (MFA)

**What it is:** requiring a second factor — an authenticator app, a hardware key, or a passkey — in addition to a password. Per CISA guidance, MFA is the single most effective control against credential-based account takeover.

**Where it applies:** every login, and every sensitive action (password change, recovery-detail change, payment-method change, payout). The strongest form is phishing-resistant MFA (passkeys, hardware keys), which cannot be captured by a real-time phishing page.

**Watch out for:** SMS-based MFA is the weakest form — it is vulnerable to SIM swapping (see TTP 4.4). Offer it only as a fallback, and never as the sole recovery method for high-value accounts. Rolling out MFA without a recovery plan creates lockouts; plan the recovery flow before you enforce MFA.

*ATT&CK relevance: mitigates T1110.004, T1078, T1566.002. D3FEND: D3-MFA (Multi-factor Authentication).*

### C2 — Velocity limits and rate limiting

**What it is:** caps on how many times an action can be attempted in a time window — logins, password resets, payment authorizations, new-account applications, refund claims — per account, per device, per IP address, and per card.

**Where it applies:** the validation stage of the lifecycle, where it is the highest-leverage control in this brief. Card testing, credential stuffing, and new-account farming are all volume-dependent: they only work if the attacker can try many times quickly.

**Watch out for:** limits that are too tight block legitimate users (for example, a customer retrying a mistyped card). Use progressive responses — a delay, then a CAPTCHA, then a temporary block — rather than a hard cutoff on the first failure. And set limits per device and per card, not only per IP address: attackers rotate IPs.

*ATT&CK relevance: mitigates T1110.004, T1657 (validation stage). General defensive principle.*

### C3 — Bot detection and CAPTCHA

**What it is:** distinguishing automated traffic from human traffic on login and payment pages, using behavioral analysis, device fingerprinting, and challenge tests.

**Where it applies:** login pages (credential stuffing), payment pages (card testing), and new-account forms (farming).

**Watch out for:** CAPTCHAs that block legitimate users are a cost, not just an inconvenience — every blocked customer is lost revenue. Tune challenges to fire on anomalous behavior, not on all traffic, and prefer invisible or behavioral checks over puzzles where possible.

*ATT&CK relevance: mitigates T1110.004, T1071.001. General defensive principle.*

### C4 — Device fingerprinting and IP reputation

**What it is:** identifying a device by its combination of attributes (browser, OS, screen, installed fonts, behavior) and checking IP addresses against reputation data (proxy, VPN, hosting, and known-abuse ranges).

**Where it applies:** checkout, login, and new-account flows. A device that has attempted many different cards, or an IP address in a range associated with abuse, is a risk signal even if no single transaction looks fraudulent.

**Watch out for:** fingerprints change (browser updates, new devices) and IP reputation data goes stale. Treat fingerprint and reputation as risk signals in a scoring model, not as a binary block — a legitimate customer on a VPN should be stepped up, not rejected outright.

*ATT&CK relevance: mitigates T1078, T1657. General defensive principle.*

### C5 — 3-D Secure and step-up authentication

**What it is:** challenging a transaction or action with additional authentication (a bank app approval, a one-time code, a biometric) when risk signals are present — instead of challenging every transaction.

**Where it applies:** payment pages (card testing, stolen-card purchases) and sensitive account actions (recovery-detail changes, payouts, large gift-card purchases).

**Watch out for:** step-up adds friction, and friction loses customers. Apply it by risk score, not by default, and exempt low-risk patterns (a returning customer on a known device buying a routine item).

*ATT&CK relevance: mitigates T1657. General defensive principle.*

### C6 — Breached-password screening and identity verification

**What it is:** checking passwords against lists of known breached passwords, and checking identity data against verification services — including thin-file checks, deceased/minor flags, and consortium data.

**Where it applies:** password creation and change, new-account onboarding, and high-value applications.

**Watch out for:** breached-password lists must come from legitimate sources (for example, Have I Been Pwned's k-anonymity API, which never reveals the password). Never store or handle raw breach data yourself. Identity-verification services cost money per check — apply the expensive checks where the risk justifies them (new accounts, high-value applications), not on every login.

*ATT&CK relevance: mitigates T1110.004, T1585. General defensive principle.*

### C7 — Transaction monitoring and fraud rules

**What it is:** rules and models that watch transactions as they happen — velocity, amounts, destinations, and behavior against per-account baselines — and flag or hold the ones that do not fit.

**Where it applies:** payment processing, bank accounts (mule detection), gift-card and crypto purchases, and refund processing.

**Watch out for:** rules generate false positives, and every false positive costs analyst time and customer goodwill. Tune rules with labeled data — confirmed fraud cases and confirmed legitimate cases — and review rule performance regularly. A rule that has not fired in six months is either perfect or broken; find out which.

*ATT&CK relevance: mitigates T1657. General defensive principle.*

### C8 — Brand monitoring and takedown

**What it is:** watching for lookalike domains, phishing pages, and social-media impersonation of your brand, and having a documented process to get them removed quickly.

**Where it applies:** any organization whose customers log in or pay online — which is to say, nearly every organization.

**Watch out for:** takedown speed matters more than takedown completeness. A phishing page's useful life is often measured in hours, so the process must work without a meeting. Keep the registrar and hosting-provider contacts in a runbook, and practice the process before you need it.

*ATT&CK relevance: mitigates T1566.002, T1566.003, T1583.001. General defensive principle.*

### C9 — Intelligence sharing and reporting

**What it is:** consuming shared fraud intelligence (breach notifications, consortium data, industry fraud feeds) and contributing confirmed fraud data back — to other institutions, to shared feeds, and to law enforcement (FBI IC3 in the U.S.).

**Where it applies:** everywhere. Fraud is a network problem: the same cards, devices, addresses, and identities appear at many organizations. No single organization sees the whole pattern.

**Watch out for:** share indicators, not customer data. Share the card's fingerprint, the device's fingerprint, the shipping address, and the pattern — never the customer's personal information. And report confirmed fraud to law enforcement even when the loss is small: small losses across many victims are how large fraud rings are identified.

*ATT&CK relevance: supports detection of all techniques in this brief. General defensive principle.*

## Priority order — if you can only do five things

For a small business or a junior analyst asked "where do we start," the answer is:

1. **MFA everywhere (C1)** — the highest-value single control against account takeover.
2. **Velocity limits on login and payment pages (C2)** — the highest-leverage control against validation-stage attacks.
3. **DMARC with a reject policy (C8)** — cheap, and it cuts phishing volume impersonating your brand.
4. **Transaction monitoring with per-account baselines (C7)** — catches mules, cash-out, and bust-out patterns.
5. **A takedown runbook (C8)** — because phishing pages will appear, and speed is the whole game.

Everything else in this file builds on those five.

## How to use this map in an interview

A hiring manager may ask: "How would you defend against credential stuffing?" The answer this map teaches is not a list of product names. It is a chain: **technique (T1110.004) → indicator (login burst, high failure rate, distributed IPs) → control (MFA, rate limiting, bot detection, breached-password screening) → residual risk (phishing-resistant MFA still needed, because a phished password plus an SMS code can still be captured in real time).** That chain — technique, indicator, control, residual risk — is the shape of a threat-intelligence analyst's answer.
