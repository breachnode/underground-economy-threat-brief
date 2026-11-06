# Public Sources

Every claim in this brief is built from public reporting, or framed as a general defensive principle and labeled as such. This file lists the source types used, what each one supports, and what each one cannot prove. No source in this list is an illicit marketplace, a leaked dataset, or an anonymous source without provenance — the absence of those source types is intentional.

## How to cite from this brief

Claims in the TTP files cite the source *type* (for example, "per FBI IC3 annual report" or "per vendor threat reporting"). That is deliberate: this brief synthesizes patterns across many reports rather than resting on any single document, and fabricating a specific citation — a report title, a page number, a date that was not actually checked — would be worse than citing the source type honestly. If you reuse a claim from this brief in your own work, go to the source type listed here, find the current report, and cite that report directly.

## Government and law-enforcement sources

- **FBI Internet Crime Complaint Center (IC3), Annual Internet Crime Reports** — ic3.gov — The baseline source for fraud categories, reported losses, and trends in identity theft, payment fraud, and business email compromise. Supports the overview (01) and the identity-theft and cash-out TTPs (05, 06).
- **FBI public service announcements on financial fraud** — Published alerts on SIM swapping, money mules, reshipping schemes, and business email compromise. Supports TTPs 4.4, 6.1, and 6.2.
- **CISA (Cybersecurity and Infrastructure Security Agency), advisories and alerts** — cisa.gov — Guidance on credential stuffing, phishing, and multi-factor authentication, including CISA's position that MFA is the most effective single control against credential-based account takeover. Supports TTPs 4.1 and 4.2 and control family C1.
- **IRS and Treasury Inspector General reporting on tax-refund fraud** — Public reporting on the scale, seasonality, and methods of tax-refund fraud. Supports TTP 5.3.
- **Europol / EC3, Internet Organised Crime Threat Assessment (IOCTA)** — Annual assessment of organized cybercrime affecting Europe, including the trade in stolen data, carding, and money-mule networks. Supports the overview (01), the lifecycle (02), and the cash-out TTPs (06).
- **Federal Reserve, synthetic identity fraud reporting** — Public research on synthetic identity fraud, including the "farming" and "bust-out" pattern. Supports TTP 5.1.

## Frameworks and standards

- **MITRE ATT&CK** — attack.mitre.org — The standard taxonomy of adversary tactics and techniques. Every technique ID in `brief/07-controls.md` is drawn from ATT&CK. Where ATT&CK has no equivalent for a fraud technique, the control map says so rather than forcing a fit.
- **MITRE D3FEND** — d3fend.mitre.org — The defensive counterpart to ATT&CK. Referenced in the control map where a D3FEND countermeasure matches a control family (notably D3-MFA for multi-factor authentication).
- **OWASP, Credential Stuffing Prevention Cheat Sheet and Top 10** — owasp.org — Defensive guidance on credential stuffing, rate limiting, and bot detection. Supports TTP 4.1 and control families C2 and C3.
- **NIST Cybersecurity Framework (CSF)** — nist.gov/cyberframework — The standard framework for organizing defensive controls. Supports the control-family structure in `brief/07-controls.md`.

## Vendor threat-intelligence research (public blogs and reports)

Major security vendors publish free threat research drawn from their telemetry — annual threat reports, campaign write-ups, and fraud-trend analyses. This brief draws on that body of reporting for the *patterns* described in the TTP files: card-testing bursts, credential-stuffing campaigns, infostealer-driven session theft, phishing-kit commoditization, and gift-card cash-out. Read vendor reporting critically — note each vendor's data source and commercial incentives — and corroborate important claims across at least two independent vendors.

- **Vendor annual threat reports** — trends and base rates for phishing, credential abuse, and payment fraud.
- **Vendor campaign write-ups** — detailed analysis of specific fraud campaigns, usually with indicators and mitigations.
- **Payment-processor and card-network fraud reporting** — public guidance on card-testing detection, chargeback patterns, and merchant fraud controls. Supports TTPs 3.1–3.4.

## Academic and research sources

- **Peer-reviewed measurement studies on the underground economy** — search via Google Scholar for studies of stolen-data markets, phishing-kit ecosystems, and fraud measurement. These studies are the strongest source for *how the ecosystem is structured* (the lifecycle in file 02), because they measure it rather than narrating it.
- **USENIX Security, IEEE S&P, ACM CCS proceedings** — the major peer-reviewed security conferences; papers are typically freely available from the authors or the conference site.

## Security journalism and community analysis

- **Established security news outlets** (for example, Krebs on Security, BleepingComputer, The Record) — timely coverage of fraud campaigns and breaches, often with primary-source reporting. Read the article, not the headline, and check whether claims are sourced.
- **SANS Internet Storm Center** — isc.sans.edu — Daily practitioner write-ups on observed threats.

## Reporting fraud and abuse

- **FBI IC3** — ic3.gov — Report internet crime, including fraud and identity theft.
- **FTC ReportFraud** — reportfraud.ftc.gov — Report fraud, scams, and identity theft.
- **Your bank or payment processor's fraud team** — for fraud affecting your own accounts or business.
- **The affected organization's abuse/security contact** — for phishing or fraud impersonating a specific company.

## What this brief does not cite — and why

- **No illicit marketplaces, forums, or channels.** Naming them would serve as a directory and would add nothing to any defensive finding.
- **No leaked datasets or breach data.** Stolen data is not a source, even when it is publicly available.
- **No anonymous sources without provenance.** If a claim cannot be traced to a citable public source, it is framed as a general defensive principle and labeled as such — or cut.

If a research question cannot be answered from the sources above, the question is too broad. Narrow it.
