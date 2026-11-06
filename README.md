# Underground-Economy Threat Brief — Carding & Identity-Theft TTPs and the Controls That Stop Them

**Author:** Barry Harris — Greensboro, NC — bhar856@wgu.edu — [linkedin.com/in/barryha](https://www.linkedin.com/in/barryha)

**Background:** CompTIA CySA+ (Apr 2025), CompTIA PenTest+ (Jun 2025), CompTIA Security+ (Jul 2024), Google Cybersecurity Certificate (Mar 2025), ISC2 Certified in Cybersecurity (Mar 2025). Currently pursuing a BS in Cybersecurity and Information Assurance at Western Governors University (May 2023–Present).

## What this is

A defensive threat-landscape brief on how carding and identity-theft operations work at a high level: the lifecycle from stolen data to monetization, the tactics, techniques, and procedures (TTPs) seen in public reporting, and the defensive controls that stop each stage.

The one-line version: **dark-web threat intelligence research — studying how carding, fraud, and identity-theft operations work so organizations can detect, prevent, and respond to them.**

Every attacker behavior described here is paired with a detection or prevention control, mapped where possible to MITRE ATT&CK. If a behavior has no defensive pairing, it does not belong in this brief — and it is not here.

## Who it is for

- **Threat-intelligence and fraud analysts** who need a structured picture of the carding and identity-theft landscape, with indicators and controls they can use on Monday morning.
- **SOC analysts (Tier 1/2)** who want to recognize what the alerts mean, not just that they fired.
- **Small-business owners and defenders** who take online payments and need to understand what is aimed at them and what stops it.
- **Hiring managers** evaluating whether a researcher can turn threat reporting into actionable, structured analysis.

## How to read this brief

Each TTP entry follows the same shape — that structure is the whole value of the brief:

1. **What it is** — one paragraph, at the level public reporting describes it.
2. **What it looks like to a defender** — the observable indicators.
3. **What stops it** — the controls, with a pointer to the full control mapping in `brief/07-controls.md`.

Read in order:

1. **`brief/01-overview.md`** — what the underground economy is and why defenders should care.
2. **`brief/02-lifecycle.md`** — the high-level lifecycle: theft → validation → sale → monetization.
3. **`brief/03-ttps-carding.md`** — carding TTPs: card testing, stolen-card use, and the validation pipeline.
4. **`brief/04-ttps-account-takeover.md`** — credential stuffing, phishing kits, session hijacking, and account takeover patterns.
5. **`brief/05-ttps-identity-theft.md`** — identity theft: synthetic identity, full-identity takeover, and fraud against tax, benefits, and financial accounts.
6. **`brief/06-ttps-mules-cashout.md`** — money mules, reshipping, and cash-out networks.
7. **`brief/07-controls.md`** — the defensive control map: every TTP against its controls, with MITRE ATT&CK technique IDs.
8. **`indicators/sample-indicators.md`** — synthetic example indicators (labeled as such), showing how to write one.
9. **`templates/threat-brief-template.md`** — a blank template for writing your own brief in this same shape.
10. **`sources.md`** — the public sources this brief is built from.

## Scope and limits

- This brief describes attacker behavior **at the level public reporting uses**: threat-intelligence vendor reports, CISA and FBI advisories, Europol threat assessments, and academic measurement studies. See `sources.md`.
- It contains **no operational detail for wrongdoing**: no instructions for committing fraud, no instructions for reaching or using illicit markets, no marketplace names or links, no vendor handles, no prices, and no real stolen or victim data of any kind.
- All examples are either **synthetic (labeled as such)** or drawn from **public reporting**, with the source type cited. Where a claim has no public source, it is framed as a general defensive principle and labeled as such.
- MITRE ATT&CK is an enterprise-intrusion taxonomy; fraud techniques map to it imperfectly. Where a TTP has no clean ATT&CK equivalent, the brief says so and maps to the closest technique rather than forcing a fit. See `brief/07-controls.md` for the mapping notes.
- This brief is **not legal advice** and it is not a substitute for a fraud-risk assessment of a specific business. Thresholds and examples are illustrative starting points, not observed fraud rates.

## Disclaimer

This material is **defensive threat-intelligence research**. All examples are synthetic or drawn from public reporting, and nothing in this repository is intended to enable wrongdoing. The legal material in a companion project of mine (*threat-research-opsec-guide*) is **general information, not legal advice** — the author is not a lawyer. Laws vary by jurisdiction and change over time; consult a qualified attorney before undertaking any research activity you are unsure about. If anything in this brief conflicts with your employer's policies or applicable law, the law and your employer's policies win.

## Repository structure

```
underground-economy-threat-brief/
├── README.md                          # This file
├── brief/
│   ├── 01-overview.md                 # What the underground economy is
│   ├── 02-lifecycle.md                # Theft → validation → sale → monetization
│   ├── 03-ttps-carding.md             # Carding TTPs
│   ├── 04-ttps-account-takeover.md    # Credential stuffing, phishing, session hijack
│   ├── 05-ttps-identity-theft.md      # Synthetic identity, full takeover, benefits fraud
│   ├── 06-ttps-mules-cashout.md       # Money mules, reshipping, cash-out
│   └── 07-controls.md                 # Control map with MITRE ATT&CK technique IDs
├── indicators/
│   └── sample-indicators.md           # Synthetic example indicators (labeled)
├── templates/
│   └── threat-brief-template.md       # Blank brief template in this repo's shape
├── sources.md                         # Public sources only
└── LICENSE                            # MIT License
```

## The interview question this repo answers

Employers ask: *"Can you turn threat reporting into something a defender can act on?"* This brief is the answer: every behavior has indicators a SOC or fraud team can watch for, and every behavior has a control that stops it. That mapping — behavior to indicator to control — is the core skill of a threat-intelligence analyst.

The companion project *threat-research-opsec-guide* answers the other interview question: *"How do you do this research?"* Read them together.
