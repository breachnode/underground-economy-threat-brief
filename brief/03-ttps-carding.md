# 03 — TTPs: Carding

Carding is the use of stolen payment-card data to make unauthorized transactions. Per FBI IC3 reporting and vendor threat reports, it is one of the most persistent forms of online fraud, and it follows a consistent pattern: validate the card, then use it. Each entry below follows the brief's shape — what it is, what it looks like to a defender, what stops it. Full control mapping with MITRE ATT&CK IDs is in `07-controls.md`.

## TTP 3.1 — Card testing

**What it is.** Small-value authorization attempts against a merchant's payment page, used to check whether a stolen card number is still live and usable. The amounts are small — often the price of a digital good or a donation — because the point is the authorization response, not the purchase. A card that authorizes is treated as valid and passed on to the monetization stage; a card that declines is discarded. Per vendor threat reporting, card testing is usually automated and runs in bursts against many merchants, because a single card is tested against several sites before it is used.

Card testing is the validation stage of the lifecycle (see `02-lifecycle.md`). It is also the stage where a defender has the best chance of stopping the fraud cheaply: the attacker has spent money and time, and has received nothing yet.

**What it looks like to a defender.**

- A burst of low-value transactions arriving in a short window, often from a single device, IP address, or IP range — or distributed across many addresses if the attacker is rotating them.
- A high decline rate on those transactions, often well above the merchant's normal rate, because most tested cards are already dead.
- The same card number attempted against multiple amounts or multiple merchants in quick succession.
- Mismatched billing data: the billing address or postal code does not match the card, or the same billing details appear with many different card numbers.
- Transactions arriving at odd hours, or in a pattern that does not match the merchant's normal customer behavior — for example, dozens of identical low-value purchases of the same item.
- A sudden spike in authorization attempts on a payment page that normally sees steady, low-volume traffic.

**What stops it.**

- Velocity limits: cap the number of authorization attempts per card, per device, per IP address, and per account in a time window.
- Decline-rate monitoring: alert when the decline rate on low-value transactions spikes above baseline.
- Address Verification Service (AVS) and card-security-code checks, with strict handling of mismatches on low-value transactions.
- Bot detection and CAPTCHA on the payment page, tuned so legitimate customers are rarely challenged and automated bursts are blocked.
- 3-D Secure (3DS) step-up authentication for transactions that look like testing.
- Device fingerprinting: flag devices that have attempted many different cards in a short window.

*ATT&CK mapping: T1657 Financial Theft; T1071.001 Web Protocols (use of the merchant's payment interface). See `07-controls.md`.*

## TTP 3.2 — Stolen-card purchases

**What it is.** Once a card is validated, it is used to buy goods or services — typically high-value, easily resold items, digital goods, or gift cards. The purchase is made through the merchant's normal checkout, using the stolen card's details, often with the shipping address set to a reshipping mule or a drop address rather than the cardholder's address. Per vendor reporting, the time between validation and use is often short — hours to a few days — because a validated card's value decays as cardholders and issuers notice and cancel it.

**What it looks like to a defender.**

- A first-time customer placing a large order, especially of high-value or easily resold goods, with no prior purchase history.
- Billing and shipping addresses that do not match, particularly when the shipping address is in a different region or country from the billing address.
- A new account created minutes before a large purchase, or a guest checkout with no account at all.
- The shipping address matches addresses seen in previous fraud cases or in shared fraud-intelligence feeds.
- The device or IP address has a history of failed transactions or card-testing attempts.
- Multiple large orders in a short window, each with a different card but the same shipping address or device.
- The order is placed at an unusual hour for the merchant's customer base, or the customer uses an expedited shipping option on a first order.

**What stops it.**

- Hold high-value first orders for manual review, especially when billing and shipping addresses differ.
- AVS and card-security-code verification on every transaction, not only low-value ones.
- 3-D Secure step-up for transactions that combine risk signals (new account, address mismatch, high value).
- Device fingerprinting and IP reputation checks at checkout.
- Shared fraud-intelligence feeds: check shipping addresses and devices against known fraud patterns from other merchants.
- Delayed shipment for flagged orders — a short hold window lets the cardholder or issuer report the fraud before the goods ship.

*ATT&CK mapping: T1657 Financial Theft; T1078 Valid Accounts (use of a legitimate payment account). See `07-controls.md`.*

## TTP 3.3 — Refund and chargeback abuse

**What it is.** Fraud that exploits the refund and chargeback process itself. The common patterns in public reporting are:

- **Refund abuse:** a legitimate purchase is followed by a fraudulent refund claim — the customer claims the goods never arrived, or were defective, and requests a refund while keeping the goods. Repeated across many orders, this is a form of theft, not a customer-service problem.
- **Chargeback fraud ("friendly fraud"):** a cardholder disputes a legitimate charge with their issuer, claiming they did not authorize it or did not receive the goods, and the issuer reverses the payment. The merchant loses both the goods and the payment, plus a chargeback fee.
- **Chargeback as monetization:** a stolen card is used to make a purchase, and the fraud is discovered only when the real cardholder disputes the charge. From the merchant's side this looks like an ordinary chargeback — the fraud is in the original transaction, not the dispute.

Refund and chargeback abuse is easy to misread as a customer-service issue. The signal that separates fraud from ordinary disputes is pattern: velocity, repetition, and clustering.

**What it looks like to a defender.**

- A customer with an unusually high refund rate compared to the merchant's baseline, especially refunds claimed shortly after delivery.
- The same customer, device, or address appearing in multiple refund claims or chargebacks.
- Chargebacks clustered around a specific product, payment method, or time window — suggesting a batch of stolen cards was used.
- Refund claims where the customer cannot or will not return the goods, or where the goods were delivered to a different address than the billing address.
- A chargeback rate that rises after a period of elevated card-testing traffic — the testing wave and the fraud wave are the same campaign, weeks apart.

**What stops it.**

- Track refund and chargeback rates per customer, per device, and per address — not only in aggregate.
- Require return of goods before a refund is issued, and verify the return.
- Keep delivery evidence: tracking confirmation, delivery photos, and signature records for high-value orders.
- Dispute illegitimate chargebacks with the evidence you have — many issuers reverse the chargeback when the merchant can show delivery to the cardholder's address.
- Feed confirmed chargeback data back into your validation-stage controls: the cards, devices, and addresses from confirmed fraud cases should be blocked or stepped up at the payment page.

*ATT&CK mapping: T1657 Financial Theft. Chargeback abuse has no direct ATT&CK technique; it is a business-process fraud, and the mapping notes this in `07-controls.md`.*

## TTP 3.4 — Gift-card and digital-goods cash-out

**What it is.** A monetization pattern in which stolen cards are used to buy gift cards or digital goods (game currency, software licenses, streaming subscriptions) that can be resold or transferred quickly. Gift cards are attractive because they are near-cash, easy to transfer, and hard to trace once redeemed. Per vendor and law-enforcement reporting, gift-card purchases are a common first cash-out step after a card is validated, because they convert a stolen card into a portable store of value in minutes.

**What it looks like to a defender.**

- Multiple gift-card purchases in a short window, especially high-denomination cards, from a new account or a device with a short history.
- Gift cards purchased with a card whose billing data does not match the account holder.
- The same device or IP purchasing gift cards across multiple merchant sites.
- Gift-card purchases immediately followed by redemption or transfer to a different account.
- A spike in gift-card sales that does not match the merchant's normal seasonal pattern.

**What stops it.**

- Limit gift-card purchase amounts for new accounts, and step up authentication for large gift-card purchases.
- Delay gift-card activation or delivery for flagged purchases — a short hold window is often enough for the fraud to be reported.
- Monitor gift-card redemption velocity: cards redeemed immediately after purchase, or redeemed from a different device or location than the purchase, are a strong signal.
- Cap the number of gift cards per transaction and per account per day.

*ATT&CK mapping: T1657 Financial Theft. See `07-controls.md`.*

## How carding fits the lifecycle

| Lifecycle stage | Carding TTP | Defender's best move |
|---|---|---|
| Validation | Card testing (3.1) | Velocity limits, bot detection, decline monitoring |
| Monetization | Stolen-card purchases (3.2) | Address mismatch review, 3DS step-up, device checks |
| Monetization | Refund/chargeback abuse (3.3) | Per-customer tracking, evidence, dispute with proof |
| Monetization | Gift-card cash-out (3.4) | Purchase limits, activation delay, redemption monitoring |

Next: `04-ttps-account-takeover.md` — credential stuffing, phishing kits, and session hijacking.
