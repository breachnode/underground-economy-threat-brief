# 02 — The Lifecycle: Theft → Validation → Sale → Monetization

## Why a lifecycle matters to a defender

Fraud is usually discovered at the end of the chain — a chargeback, a drained account, a customer complaint. By then the attacker has already won. The lifecycle view matters because every stage before monetization is a place where the fraud can be stopped cheaply, and every stage leaves indicators that a defender can watch for. A control that works at the validation stage costs far less than a chargeback at the monetization stage.

Public reporting describes the same four-stage lifecycle across carding, credential abuse, and identity theft, with variations in the details. This file walks the four stages at the level public reporting uses, then lists the defender's interruption points.

## Stage 1 — Theft

Stolen data enters the ecosystem through a small number of well-documented channels. Per vendor threat reports and CISA/FBI advisories, the common ones are:

- **Data breaches.** Databases of customer records — names, addresses, card numbers, credentials — exfiltrated from a compromised organization. The breach itself may be months old by the time the data is traded; defenders learn about it from breach disclosures and threat-intel reporting, not from the thieves.
- **Phishing campaigns.** Bulk and targeted phishing pages that harvest credentials and payment data directly from victims. Phishing kits — pre-built phishing pages sold as a service — lower the skill needed to run a campaign, which is why phishing volume stays high even as detection improves. See `04-ttps-account-takeover.md`.
- **Malware that steals data.** Infostealer malware running on a victim's machine collects saved credentials, session cookies, and card data typed into the browser. Per vendor reporting, infostealers are one of the largest sources of fresh credentials in the underground economy, because a single infection can yield dozens of usable logins.
- **Insider theft and skimming.** Employees or contractors who exfiltrate customer data, and physical skimming devices on payment terminals and ATMs. These are a smaller share of volume than breaches and phishing, per public reporting, but they produce high-quality data because it is fresh and validated.
- **Public exposure.** Misconfigured cloud storage, leaked code repositories, and data left in public places. Not theft in the criminal sense, but functionally identical once the data is collected and traded.

**Defender's interruption points at this stage:** breach detection and disclosure (so your customers' data is not the next batch traded), brand-impersonation monitoring (so phishing pages using your name are taken down), and endpoint detection on your own systems. You cannot stop other organizations' breaches — but you can watch for your customers' data appearing in them, and you can shorten the window between a breach and your response.

## Stage 2 — Validation

Raw stolen data is unreliable. Card numbers may be cancelled, credentials may be stale, identities may be incomplete. So the ecosystem validates data before relying on it — and validation is the stage where a defender has the best visibility, because validation happens against the defender's own systems.

- **Card testing.** Small-value authorization attempts against a merchant's payment page, used to check whether a stolen card is still live. The amounts are small because the point is the authorization response, not the purchase. A single card may be tested against several merchants before it is used for a real purchase. What it looks like to a defender: a burst of low-value transactions from one device or IP range, a high decline rate, and mismatched billing data. See `03-ttps-carding.md`.
- **Credential validation.** Stolen username/password pairs are tested against login pages — credential stuffing — to find which pairs still work. The same pairs are often tested against many services, because people reuse passwords. What it looks like to a defender: a burst of login attempts with a high failure rate, distributed across many IP addresses, often at odd hours. See `04-ttps-account-takeover.md`.
- **Identity validation.** Stolen personal information is checked against public records, credit data, and account-recovery flows to see which identities are complete enough to use. What it looks like: password-reset requests and account-recovery attempts against identities that have not logged in for a long time, and new-account applications with thin or inconsistent identity data. See `05-ttps-identity-theft.md`.

**Defender's interruption points at this stage:** this is the highest-leverage stage in the whole lifecycle. Velocity limits, rate limiting, CAPTCHA or bot detection on login and payment pages, breached-password screening, and device fingerprinting all work here — and they work before any money moves. A validation attempt that is blocked costs the attacker a card or a credential pair and yields them nothing. See `07-controls.md`.

## Stage 3 — Sale

Validated data is traded onward. The brief does not name marketplaces or describe how to reach them. What matters to a defender is the structure, because the structure explains the timing of the attacks they see:

- **Validated data sells faster and for more.** That is why validation happens at all — and why the validation traffic a defender sees is a leading indicator. A spike in card-testing or credential-stuffing traffic often follows a major breach becoming public, as newly traded data is validated against many targets at once.
- **Data is sold in batches, not one record at a time.** A buyer purchases a batch of cards or credentials and works through it over days or weeks. That means an attack against your business is usually not one event but a campaign: repeated attempts over a period, with pauses and changes in technique as the buyer learns what your controls catch.
- **Specialization is normal.** Different actors handle theft, validation, sale, and monetization. The person testing cards against your payment page is rarely the person who stole the data. That is good news for a defender: you do not need to understand the whole ecosystem, only the stage that touches you.

**Defender's interruption points at this stage:** threat-intelligence feeds and breach notifications. If you know a breach exposed your customers' data, you can force password resets, step up authentication, and tighten payment controls before the validation wave arrives. The window between a breach disclosure and the validation spike is the most valuable warning a defender gets.

## Stage 4 — Monetization

This is where the fraud becomes a loss: unauthorized purchases, drained accounts, new accounts opened in a victim's name, tax refunds or benefits claimed fraudulently, and the laundering of the proceeds through mules and cash-out networks. The TTP files cover monetization in detail:

- Carding monetization: `03-ttps-carding.md`
- Account-takeover monetization: `04-ttps-account-takeover.md`
- Identity-theft monetization: `05-ttps-identity-theft.md`
- Cash-out and laundering: `06-ttps-mules-cashout.md`

**Defender's interruption points at this stage:** transaction monitoring, fraud rules, chargeback analysis, and account-behavior baselines. Controls here are more expensive than at the validation stage — money has already moved — but they are still far cheaper than doing nothing. And the data from this stage feeds back into the earlier stages: every confirmed fraud case is a labeled example that improves your validation-stage controls.

## The lifecycle as a defensive loop

The four stages form a loop, not a line. What you learn at monetization (which cards were fraudulent, which accounts were taken over, which identities were synthetic) should tighten your validation controls, which reduces the value of the next batch of stolen data, which makes the ecosystem's economics worse for the attacker. That is the entire defensive strategy in one sentence:

> **Make validation expensive and monetization slow, and the fraud moves somewhere easier.**

No single control does that. The control map in `07-controls.md` shows how the controls combine across stages.

## Lifecycle summary table

| Stage | Attacker goal | What the defender sees | Highest-leverage control |
|---|---|---|---|
| Theft | Obtain data | Breach disclosures, brand phishing | Breach monitoring, takedowns |
| Validation | Confirm data works | Card-testing bursts, login bursts | Velocity limits, bot detection, MFA |
| Sale | Trade validated data | Breach-notification timing | Threat-intel feeds, forced resets |
| Monetization | Convert data to money | Fraudulent transactions, chargebacks | Fraud rules, transaction monitoring |

Next: `03-ttps-carding.md` — the carding TTPs in detail.
