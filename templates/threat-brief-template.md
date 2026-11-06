# Threat-Brief Template

Use this template to write your own brief in the same shape as this repo. Each TTP entry follows the same structure: what it is, what it looks like to a defender, what stops it. That structure is the whole value of the format — do not skip a section.

Copy this file, rename it for your topic, and fill in each section. Delete the guidance in italics as you go.

---

# [Topic] Threat Brief — [TTP family] and the Controls That Stop Them

**Author:** [Name] — [Location] — [Email] — [LinkedIn]

**Background:** [Certifications with dates. Education status stated honestly — "currently pursuing" if in progress, never presented as completed.]

## What this is

*[One paragraph: what the brief covers, at the level public reporting describes it. One sentence naming the defensive framing: studying how the threat works so organizations can detect, prevent, and respond.]*

## Who it is for

- *[Audience 1 — e.g., threat-intelligence analysts]*
- *[Audience 2 — e.g., SOC analysts]*
- *[Audience 3 — e.g., small-business defenders]*

## Scope and limits

- *[What sources the brief is built from — public reporting only.]*
- *[What is excluded: no operational detail for wrongdoing, no marketplace names or links, no real stolen or victim data.]*
- *[How examples are handled: synthetic (labeled) or drawn from public reporting with the source type cited.]*
- *[Any mapping caveats — e.g., where MITRE ATT&CK does not cover the technique cleanly.]*

## Disclaimer

*[Defensive threat-intelligence research. All examples synthetic or drawn from public reporting. Nothing in the brief is intended to enable wrongdoing. Not legal advice.]*

---

## TTP [number] — [TTP name]

**What it is.**

*[One paragraph, at the level public reporting describes it. What the attacker is trying to achieve. No operational steps, no how-to.]*

**What it looks like to a defender.**

- *[Observable indicator 1 — specific, tied to a log or system.]*
- *[Observable indicator 2.]*
- *[Observable indicator 3.]*
- *[Add or remove bullets as needed. Every indicator should name where it is observed.]*

**What stops it.**

- *[Control 1 — specific, with where it applies.]*
- *[Control 2.]*
- *[Control 3.]*
- *[Add or remove bullets as needed.]*

*ATT&CK mapping: [Technique ID and name, or "no direct technique — closest mapping is X, and the gap is noted." See the control map.]*

---

## Control map

| TTP | ATT&CK technique(s) | Controls |
|---|---|---|
| [TTP number and name] | [ID(s) or "no direct technique"] | [Control list] |
| *[Add one row per TTP.]* | | |

## Priority order — if you can only do five things

1. *[Highest-value control.]*
2. *[Second.]*
3. *[Third.]*
4. *[Fourth.]*
5. *[Fifth.]*

## Sources

- *[Source type and what it supports — e.g., "CISA advisory on [topic] — supports the indicator list for TTP 1."]*
- *[Do not fabricate specific citations. If you cannot name the source, cut the claim or reframe it as a general defensive principle and label it as such.]*

---

## Pre-publication checklist

Before publishing a brief written from this template, check every item. If any item fails, fix it before publishing — do not publish and fix later.

- [ ] Every TTP entry has all three sections (what it is / what it looks like / what stops it).
- [ ] No operational detail for wrongdoing anywhere in the brief.
- [ ] No marketplace names, URLs, vendor handles, or prices.
- [ ] No real stolen or victim data of any kind, including partial or redacted data.
- [ ] Every example is labeled synthetic or cited to a public source by type.
- [ ] Every threshold is labeled as an illustrative starting point, not an observed rate.
- [ ] Every ATT&CK mapping is to a real technique ID, or explicitly marked as "no direct technique."
- [ ] No claim about prevalence or fraud rates without a cited public source.
- [ ] The disclaimer is present and accurate.
- [ ] Read the whole brief once as a skeptic — a hiring manager, a background investigator, a compliance reviewer. If any line would embarrass you in front of them, cut it.
