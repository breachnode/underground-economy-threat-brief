# 04 — TTPs: Account Takeover

Account takeover (ATO) is the use of stolen credentials or session data to gain control of a victim's account — and then to use that account for fraud, or to steal more data from it. Per CISA advisories and vendor threat reports, ATO is one of the most common fraud patterns against consumer and business accounts, because it exploits the weakest link in most authentication: the password. Each entry follows the brief's shape — what it is, what it looks like to a defender, what stops it. Full control mapping with MITRE ATT&CK IDs is in `07-controls.md`.

## TTP 4.1 — Credential stuffing

**What it is.** Automated login attempts using username and password pairs obtained from data breaches, tested against many services at once. The technique works because people reuse passwords: a pair stolen from one breached service often works on another. Per CISA advisories and OWASP guidance, credential stuffing is typically automated, distributed across many IP addresses to evade simple rate limits, and run in bursts — thousands of attempts in minutes.

Credential stuffing is the account-takeover equivalent of card testing: it is the validation stage, and it is where a defender has the best chance of stopping the attack cheaply. A stuffed credential that is blocked costs the attacker nothing to lose but yields them nothing either.

**What it looks like to a defender.**

- A burst of login attempts with a high failure rate, often 90% or more, arriving in a short window.
- The attempts are distributed across many IP addresses, often from proxy or VPN ranges, so no single address accounts for most of the traffic.
- Login attempts against accounts that have not logged in for a long time, or against usernames that do not exist in your system — the attacker is working through a list, not targeting specific users.
- Attempts arriving at odd hours for your user base, or in a pattern that does not match normal login traffic (for example, a steady rate of attempts around the clock rather than the normal daytime peak).
- A sudden increase in password-reset requests following a login burst — the attacker is testing which accounts exist and which recovery flows are available.
- Successful logins immediately followed by changes to the account: a new email address, a new phone number, a password change, or a new shipping address — the attacker securing the account before the victim notices.

**What stops it.**

- Multi-factor authentication (MFA) on every account, especially on accounts with payment methods or stored personal data. MFA is the single most effective control against credential stuffing, per CISA guidance.
- Rate limiting and account lockout on failed logins — with care to avoid locking out legitimate users (use progressive delays and CAPTCHA before hard lockout).
- Breached-password screening: check new and changed passwords against lists of known breached passwords, and force a reset when a user's current password appears in a breach.
- Bot detection on the login page: behavioral analysis, device fingerprinting, and CAPTCHA tuned to challenge automated bursts without blocking legitimate users.
- Login anomaly alerts: notify the user (by email or push) when a login comes from a new device or an unusual location, and give them a one-click way to report it.
- Passwordless or passkey-based login where feasible — it removes the reused-password problem entirely.

*ATT&CK mapping: T1110.004 Credential Stuffing; T1078 Valid Accounts. See `07-controls.md`.*

## TTP 4.2 — Phishing kits and brand impersonation

**What it is.** Pre-built phishing pages — often sold as a service — that impersonate a legitimate brand's login page, payment page, or support page. The victim is lured to the page by email, text message, or a malicious advertisement, and enters their credentials or payment data, which is harvested by the attacker. Per vendor threat reports and FBI advisories, phishing kits have made phishing a commodity: an attacker with little technical skill can run a convincing campaign using a kit built by someone else.

For the brand being impersonated, the damage is twofold: customers lose accounts and money, and the brand loses trust. For a defender at the impersonated organization, the work is detection and takedown, not prevention — you cannot stop someone from building a lookalike page, but you can find it fast and get it removed.

**What it looks like to a defender.**

- Customers reporting login pages that look like yours but are hosted on a different domain — often a lookalike domain (a misspelling, an extra word, a different top-level domain) or a newly registered domain.
- A spike in password-reset requests or support tickets from customers who "logged in" but cannot access their accounts — they gave their credentials to the phishing page.
- Phishing emails or text messages impersonating your brand, reported by customers or detected by your email-security tools.
- A new domain registered that is similar to yours, or a subdomain of a free hosting service that mimics your login page.
- Social-media posts or advertisements impersonating your brand and linking to the phishing page.

**What stops it.**

- DMARC, SPF, and DKIM on your email domains, enforced with a reject policy — so phishing emails that claim to come from you are rejected by receiving mail servers.
- Brand monitoring: watch for newly registered domains similar to yours, and for your brand name appearing on free hosting services and URL shorteners.
- Takedown process: have a documented process and a point of contact for reporting phishing pages to hosting providers, domain registrars, and browsers' safe-browsing programs. Speed matters — a phishing page's useful life is often measured in hours.
- Customer education: teach customers to check the domain before logging in, and to report suspicious messages. Keep it concrete — show them what your real login page looks like and where the real domain is.
- MFA on customer accounts: even if a customer hands over their password to a phishing page, MFA blocks the attacker from using it (unless the phishing page also captures the second factor in real time, which is why phishing-resistant MFA such as passkeys is the stronger option).

*ATT&CK mapping: T1566.002 Spearphishing Link; T1566.003 Spearphishing via Service; T1583.001 Domains (acquire infrastructure — the lookalike domain). See `07-controls.md`.*

## TTP 4.3 — Session hijacking

**What it is.** Theft of a live session — usually the session cookie or token that keeps a user logged in — so the attacker can use the victim's account without knowing the password. Per vendor threat reports, session cookies are commonly harvested by infostealer malware on the victim's machine, or captured through a malicious browser extension or a compromised network. Once the attacker has the session token, they can act as the victim until the session expires or is revoked.

Session hijacking is harder to detect than credential stuffing, because the login looks legitimate: the attacker is using a valid session, from the victim's own authentication. The indicators are behavioral, not credential-based.

**What it looks like to a defender.**

- A logged-in session that suddenly changes behavior: a new device fingerprint, a new IP address or geographic location, or a new browser, without a fresh login.
- A session that performs actions the account owner normally does not: changing the account email, adding a new payment method, placing a large order, or initiating a payout.
- Multiple sessions active for the same account from different locations at the same time.
- A session that accesses the account from a device type the owner has never used.
- A spike in session-related support tickets: customers reporting that they were logged out, or that their account settings changed without their knowledge.

**What stops it.**

- Bind sessions to the device and network that created them: invalidate a session when the device fingerprint or IP address changes sharply, and require re-authentication.
- Short session lifetimes for sensitive actions: require fresh authentication (password or MFA) before changing an email address, adding a payment method, or initiating a payout, even if the user is already logged in.
- Step-up authentication for high-risk actions: a session that has been quiet for a long time, or that suddenly attempts a sensitive action, should be challenged.
- Allow users to see and revoke active sessions from their account settings, and notify them when a new session is created.
- HttpOnly and Secure flags on session cookies, and SameSite attributes, so cookies are harder to steal through cross-site scripting and are not sent over unencrypted connections.

*ATT&CK mapping: T1550.002 Web Session Cookie; T1539 Steal Web Session Cookie; T1078 Valid Accounts. See `07-controls.md`.*

## TTP 4.4 — Password-reset and recovery abuse

**What it is.** Abuse of the account-recovery flow — the "forgot password" process — to take over an account. The common patterns in public reporting are:

- **Reset-link interception:** the attacker triggers a password reset and intercepts the reset link or code, usually by having already compromised the victim's email account or by exploiting a weakness in how the reset is delivered.
- **SIM swapping:** the attacker convinces the victim's mobile carrier to move the victim's phone number to a SIM card the attacker controls, then uses SMS-based password resets and MFA codes to take over accounts. Per FBI advisories, SIM swapping is a well-documented precursor to account takeover, especially for financial and cryptocurrency accounts.
- **Knowledge-based reset abuse:** the attacker answers security questions using personal information obtained from breaches, social media, or public records.
- **Support social engineering:** the attacker contacts the victim organization's support team, impersonates the account owner, and persuades the agent to reset the account or change the recovery email.

The recovery flow is designed to let a legitimate user back in when they have lost access — which is exactly what makes it attractive to an attacker. Every recovery option is a second front door, and it needs the same level of defense as the login page.

**What it looks like to a defender.**

- A burst of password-reset requests against accounts that have not logged in recently, or against many accounts in a short window.
- Reset requests followed immediately by a change to the account's recovery email or phone number.
- A support ticket in which the requester cannot answer verification questions but is persistent and urgent, or in which the details keep changing.
- An account whose recovery phone number was recently changed, followed by a password reset and a large transaction.
- Customers reporting that their phone stopped working (a possible SIM swap) followed by account-takeover reports.

**What stops it.**

- Never use SMS alone as a recovery or MFA method for high-value accounts — offer app-based authenticators or passkeys, and treat SMS as the weakest option.
- Require strong verification before any recovery-email or phone-number change: a fresh login plus MFA, and a notification to the old contact details with a delay before the change takes effect.
- Rate-limit password-reset requests per account and per IP address, and alert the account owner when a reset is requested.
- Train support staff on social-engineering tactics and give them a clear verification script — and a clear rule that they never bypass verification under pressure.
- Notify the account owner through a second channel (email and push, for example) whenever recovery details change, with a link to report it if they did not make the change.

*ATT&CK mapping: T1078 Valid Accounts; T1566.004 Spearphishing Voice (vishing, where the attacker calls the victim or the carrier); T1556.003 Modify Authentication Process (where applicable). See `07-controls.md`.*

## How account takeover fits the lifecycle

| Lifecycle stage | ATO TTP | Defender's best move |
|---|---|---|
| Theft | Phishing kits (4.2) | DMARC, brand monitoring, takedowns |
| Validation | Credential stuffing (4.1) | MFA, rate limiting, bot detection |
| Validation | Password-reset abuse (4.4) | Recovery verification, SMS deprecation |
| Monetization | Session hijacking (4.3) | Session binding, step-up auth, revocation |

Next: `05-ttps-identity-theft.md` — synthetic identity, full-identity takeover, and fraud against tax, benefits, and financial accounts.
