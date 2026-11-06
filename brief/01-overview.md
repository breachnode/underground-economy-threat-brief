# 01 — Overview: What the Underground Economy Is

## The short version

The "underground economy" is the name threat-intelligence reporting gives to the loosely connected set of criminal markets and services that trade in stolen data and fraud. It is not one organization, one website, or one group. It is an ecosystem of buyers, sellers, and service providers who mostly never meet each other, connected by reputation systems and escrow arrangements on illicit forums and marketplaces — the names and locations of which this brief deliberately does not publish.

A defender does not need to know the names of those places. A defender needs to know three things:

1. **What is traded.** Stolen payment-card data, stolen credentials, personal information, and access to compromised accounts and systems.
2. **How it is used.** Carding (unauthorized card transactions), account takeover, identity theft, and the laundering of the proceeds through mules and cash-out networks.
3. **Where it touches a legitimate business.** At the payment page, the login page, the account-recovery flow, and the refund and chargeback process — the same four places every online business already has.

This brief covers those three things, in that order, at the level public reporting describes them.

## Why defenders should care

Fraud is not a niche problem. Per the FBI Internet Crime Complaint Center's annual Internet Crime Reports, internet crime losses reported by the American public run into the billions of dollars every year, and identity theft and payment fraud are consistently among the most-reported categories. Per Europol's Internet Organised Crime Threat Assessment (IOCTA), the trade in stolen data is one of the most persistent forms of organized cybercrime affecting European citizens and businesses, and the same patterns appear in U.S. reporting.

The practical point for a defender is narrower than those numbers. Fraud against a business is rarely a single dramatic breach. It is usually a slow bleed: a few card-testing attempts a day, a handful of account takeovers a week, a chargeback rate that creeps upward. The businesses that catch it early are the ones that know what the early signs look like. That is what this brief is for.

## What this brief is not

- **It is not a catalog of illicit markets.** No marketplace names, no URLs, no vendor handles, no prices. Those details help no defender and would only serve as a directory.
- **It is not an investigation of a specific criminal group.** The TTPs here are patterns drawn from public reporting across many actors, not profiles of named groups.
- **It is not a how-to.** Every TTP is described the way a vendor threat report describes it: what the attacker is trying to achieve, what it looks like from the defender's side, and what stops it. Operational steps for carrying out fraud are absent by design.
- **It is not a claim about prevalence.** This brief does not say how common any given TTP is, or what fraud rate a business should expect. Where a claim would need a number, the brief either cites the public source that published the number or says plainly that the number is not available.

## The four places fraud touches your business

Almost every TTP in this brief arrives through one of four doors. If you defend only these four, you cover most of the ground:

1. **The payment page.** Card testing, stolen-card purchases, and refund fraud all arrive here. The indicators are velocity, decline rates, and mismatches between billing, shipping, and device data. See `03-ttps-carding.md`.
2. **The login page.** Credential stuffing, password spraying, and phishing-driven account takeover all arrive here. The indicators are login bursts, impossible travel, and new-device logins. See `04-ttps-account-takeover.md`.
3. **The account-recovery flow.** SIM swapping, password-reset abuse, and social engineering of support staff all target the recovery flow because it is designed to let a legitimate user back in — which is exactly what an attacker wants. See `04-ttps-account-takeover.md` and `05-ttps-identity-theft.md`.
4. **The refund and chargeback process.** Friendly fraud, refund abuse, and chargeback fraud exploit the process that exists to protect legitimate customers. The indicators are refund velocity, repeat refund claims, and chargeback clustering. See `03-ttps-carding.md` and `06-ttps-mules-cashout.md`.

A small business that monitors those four doors — even with simple rules — catches most of what this brief describes. The controls in `07-controls.md` are organized around them.

## How the underground economy is organized

Public reporting (vendor threat reports, Europol's IOCTA, and academic measurement studies) consistently describes the same rough structure. Understanding it helps a defender because each layer has different indicators and different controls:

- **Suppliers** obtain stolen data: through breaches, phishing campaigns, malware that steals credentials and card data, and insider theft. A defender meets this layer as a breach notification, a phishing campaign impersonating their brand, or malware on a customer's machine.
- **Brokers and marketplaces** trade the data onward. Stolen card data and credentials are typically validated before sale — tested to confirm they still work — because validated data commands a higher price. A defender meets this layer indirectly, as a spike in card-testing or credential-stuffing traffic after a major breach becomes public.
- **Buyers and operators** use the data: they make fraudulent purchases, take over accounts, or open new accounts with stolen identities. A defender meets this layer directly, at the payment page, the login page, and the recovery flow.
- **Facilitators** provide the services the other layers need: phishing kits, malware, money mules, reshipping networks, and laundering. A defender meets this layer as phishing pages impersonating their brand, or as mule accounts moving money through their platform.
- **Cash-out networks** convert fraud proceeds into usable money: through reshipping, gift cards, cryptocurrency, and mule accounts. A defender meets this layer as unusual refund, shipping, or payout patterns.

No single business faces all five layers at once. A merchant faces buyers and operators; a bank faces mules and cash-out; an email provider faces phishing kits. The brief's TTP files are organized so a reader can go straight to the layer that faces them.

## A note on confidence

Threat research separates fact from assessment, and this brief follows the same rule (see the companion *threat-research-opsec-guide*, `methodology/05-writeup-standards.md`):

- **Fact** — what a public source says. Cited by source type (e.g., "per FBI IC3 annual report").
- **Assessment** — what the pattern implies for a defender. Labeled as assessment, with a confidence level where it matters.
- **General defensive principle** — a control or practice that is standard in the field and does not depend on any single report. Labeled as such.

If a sentence in this brief does not carry a source, a confidence label, or a "general principle" marker, treat it as the author's assessment and verify it before acting on it.

## What comes next

`02-lifecycle.md` walks the lifecycle end to end: how stolen data moves from theft to validation to sale to monetization, and where a defender can interrupt it at each stage.
