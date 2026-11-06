# Sample Indicators — Synthetic Examples

## Read this first

**Every indicator in this file is SYNTHETIC.** The values below are invented illustrations of what a fraud indicator looks like — they are not observed data, not real accounts, not real cards, not real IP addresses, and not real victims. They exist to show the *shape* of a good indicator: specific, observable, and tied to a TTP and a control.

A good indicator has five parts:

1. **What to observe** — the specific event or pattern.
2. **Where to observe it** — the system or log that shows it.
3. **The threshold** — what counts as anomalous (always labeled as an illustrative starting point, to be tuned to your volume).
4. **The TTP it maps to** — so the analyst knows what they are looking at.
5. **The action** — what to do when it fires.

Every example below follows that shape. Thresholds are illustrative starting points — tune them to your own traffic before using them in production. Never present a threshold from this file as an observed fraud rate.

## Carding indicators

### IND-CARD-01 — Card-testing burst (synthetic example)

- **What to observe:** more than 20 low-value authorization attempts (illustrative threshold: under $5 each) from a single device fingerprint within 10 minutes.
- **Where:** payment gateway authorization logs.
- **Threshold:** 20 attempts / 10 minutes / one device. *Illustrative — tune to your volume.*
- **TTP:** 3.1 Card testing (T1657).
- **Action:** block the device for 1 hour; alert the fraud queue; add the device fingerprint to the watchlist.

### IND-CARD-02 — Decline-rate spike (synthetic example)

- **What to observe:** the decline rate on low-value transactions rises above 40% in a rolling 1-hour window, against a baseline of under 10%.
- **Where:** payment gateway authorization logs.
- **Threshold:** 40% decline rate / 1 hour. *Illustrative — tune to your baseline.*
- **TTP:** 3.1 Card testing (T1657).
- **Action:** alert the fraud queue; enable CAPTCHA on the payment page for the affected traffic; review the source IP ranges.

### IND-CARD-03 — Billing/shipping mismatch on a first order (synthetic example)

- **What to observe:** a first-time customer places an order over $200 (illustrative threshold) with a shipping address in a different state or country from the billing address.
- **Where:** order management system.
- **Threshold:** first order + value over $200 + address mismatch. *Illustrative — tune to your average order value.*
- **TTP:** 3.2 Stolen-card purchases (T1657).
- **Action:** hold the order for manual review; verify the cardholder by phone or email before shipping.

### IND-CARD-04 — Gift-card purchase spike (synthetic example)

- **What to observe:** more than 3 gift-card purchases from one account in 24 hours, or a single gift-card purchase over $500 from an account less than 7 days old.
- **Where:** order management system and gift-card platform.
- **Threshold:** 3 purchases / 24 hours, or $500+ on an account under 7 days old. *Illustrative.*
- **TTP:** 3.4 Gift-card cash-out (T1657).
- **Action:** hold the gift cards for review; step up authentication before release.

## Account-takeover indicators

### IND-ATO-01 — Credential-stuffing burst (synthetic example)

- **What to observe:** more than 100 failed login attempts in 5 minutes, distributed across more than 20 IP addresses, with a failure rate above 90%.
- **Where:** authentication logs.
- **Threshold:** 100 failures / 5 minutes / 20+ IPs / 90% failure rate. *Illustrative.*
- **TTP:** 4.1 Credential stuffing (T1110.004).
- **Action:** enable CAPTCHA on the login page; rate-limit the source ranges; force a password reset for any account that was successfully logged in during the burst.

### IND-ATO-02 — Login from a new device followed by a contact-detail change (synthetic example)

- **What to observe:** a successful login from a device never seen on the account, followed within 1 hour by a change to the account email address or phone number.
- **Where:** authentication logs and account-change logs.
- **Threshold:** new device + contact change within 1 hour. *Illustrative.*
- **TTP:** 4.1 Credential stuffing / 4.3 Session hijacking (T1078, T1550.002).
- **Action:** revert the change; lock the account; notify the customer through the original contact details; require MFA re-enrollment.

### IND-ATO-03 — Password-reset burst (synthetic example)

- **What to observe:** more than 10 password-reset requests against one account in 1 hour, or more than 50 reset requests across many accounts from one IP address in 1 hour.
- **Where:** password-reset logs.
- **Threshold:** 10 resets / 1 hour / one account, or 50 resets / 1 hour / one IP. *Illustrative.*
- **TTP:** 4.4 Password-reset abuse (T1078).
- **Action:** rate-limit resets; alert the account owner; review the source IP range.

### IND-ATO-04 — Session behavior change (synthetic example)

- **What to observe:** an active session's device fingerprint or IP geolocation changes sharply mid-session, followed by a sensitive action (password change, payout, large purchase).
- **Where:** session logs and audit logs.
- **Threshold:** fingerprint or location change + sensitive action in the same session. *Illustrative.*
- **TTP:** 4.3 Session hijacking (T1550.002).
- **Action:** invalidate the session; require re-authentication with MFA; notify the customer.

## Identity-theft indicators

### IND-ID-01 — Thin-file new account (synthetic example)

- **What to observe:** a new-account application with no credit history, a recently issued Social Security number, and contact details (email, phone) created within the last 30 days.
- **Where:** new-account application system and identity-verification service.
- **Threshold:** thin file + recently issued number + contact details under 30 days old. *Illustrative.*
- **TTP:** 5.1 Synthetic identity fraud (T1585).
- **Action:** step up verification (document check); hold the account for review before extending credit.

### IND-ID-02 — Application velocity from one device (synthetic example)

- **What to observe:** more than 5 new-account applications from one device fingerprint in 24 hours, each under a different name.
- **Where:** new-account application system.
- **Threshold:** 5 applications / 24 hours / one device. *Illustrative.*
- **TTP:** 5.1 Synthetic identity fraud; 5.4 New-account fraud (T1585).
- **Action:** block the device; review all five applications manually; share the device fingerprint with your fraud consortium.

### IND-ID-03 — Bust-out pattern (synthetic example)

- **What to observe:** an account that was used carefully for 6+ months (small purchases, on-time payments) suddenly maxes out its credit line or takes a large cash advance within 7 days.
- **Where:** account transaction history.
- **Threshold:** 6 months of careful use + credit utilization jumping above 90% within 7 days. *Illustrative.*
- **TTP:** 5.1 Synthetic identity fraud (T1657).
- **Action:** freeze further credit increases; review the account; check the identity against consortium fraud data.

### IND-ID-04 — Contact-detail change plus new application (synthetic example)

- **What to observe:** an existing customer's contact details change, and within 7 days a new credit application is filed in the same name using the new contact details.
- **Where:** account-change logs and new-account applications.
- **Threshold:** detail change + new application within 7 days. *Illustrative.*
- **TTP:** 5.2 Full-identity takeover (T1586).
- **Action:** contact the customer through the original contact details; hold the application pending verification.

## Money-mule and cash-out indicators

### IND-MULE-01 — Rapid in-and-out movement (synthetic example)

- **What to observe:** an account receives a deposit over $1,000 and forwards more than 80% of it onward within 24 hours, to a different bank or a crypto exchange.
- **Where:** transaction monitoring system.
- **Threshold:** 80% forwarded within 24 hours. *Illustrative — tune to your customer base.*
- **TTP:** 6.1 Money mules (T1657).
- **Action:** hold the outbound transfer for review; contact the account holder; file a report if confirmed.

### IND-MULE-02 — Round-number layering (synthetic example)

- **What to observe:** money moves through three or more accounts in one day, in round amounts, with each account keeping a small percentage and forwarding the rest.
- **Where:** transaction monitoring system, viewed as a network.
- **Threshold:** 3+ hops in one day, round amounts. *Illustrative.*
- **TTP:** 6.4 Layering (T1657).
- **Action:** review the full chain; report the chain to law enforcement (FBI IC3) and to the other institutions involved.

### IND-MULE-03 — Drop-point shipping address (synthetic example)

- **What to observe:** one shipping address receives orders for more than 5 different customers in 30 days, each order paid with a different card.
- **Where:** order management system.
- **Threshold:** 5 customers / 30 days / one address. *Illustrative.*
- **TTP:** 6.2 Reshipping networks (T1657).
- **Action:** hold orders to that address for review; check the address against shared fraud data; report if confirmed.

## How to write your own indicator

Use the five-part shape above. Then test it before you deploy it:

1. **Run it against last month's data.** How many times would it have fired? If the answer is thousands, the threshold is too loose. If it is zero, the threshold is too tight or the data is not in the log you think it is.
2. **Check the false-positive rate.** Of the times it fires, how many are legitimate? An indicator that is right 10% of the time will be ignored by analysts within a week.
3. **Give it a sunset review.** Re-check every indicator after 90 days. Attackers adapt; an indicator that worked in January may be useless by April.
4. **Label synthetic examples as synthetic.** If you write an example indicator with invented values, label it the way this file does. An invented example presented as observed data is a lie that will be found out.
