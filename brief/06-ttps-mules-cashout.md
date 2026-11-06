# 06 — TTPs: Money Mules and Cash-Out Networks

Fraud produces money in the wrong place: in a victim's account, on a stolen card, or as goods bought with stolen funds. Cash-out is the process of moving that value into the attacker's hands. It runs through people and channels that a defender can see — mule accounts, reshipping addresses, and gift-card and cryptocurrency conversions — which is why cash-out is one of the most detectable stages of the lifecycle. Each entry follows the brief's shape — what it is, what it looks like to a defender, what stops it. Full control mapping with MITRE ATT&CK IDs is in `07-controls.md`.

## TTP 6.1 — Money mules

**What it is.** A money mule is a person who receives fraud proceeds into their own bank account and forwards them onward — by wire transfer, cash withdrawal, cryptocurrency purchase, or gift-card purchase — keeping a percentage as a "fee." Per FBI and Europol reporting, mules are recruited through fake job advertisements (work-from-home payment processing, mystery shopping, "financial agent" roles), romance scams, and social-media offers of easy money. Many mules do not understand they are participating in a crime; some do. Either way, the mule account is the observable point where fraud proceeds enter the legitimate financial system.

For a bank or payment platform, the mule account is the most important thing in this whole brief. It is the one place where the fraud's money touches your systems directly, and it is the one place where stopping it stops the fraud's purpose.

**What it looks like to a defender.**

- An account that receives a large deposit and forwards most of it onward within hours — often to a different bank, a cryptocurrency exchange, or a gift-card purchase.
- An account with little prior activity that suddenly starts receiving and sending large sums.
- An account whose owner recently changed their contact details, or whose account was opened recently with thin identity data.
- A pattern of round-number transfers: money in, then money out in similar round amounts, with a small amount kept behind.
- An account that receives funds from many different sources and sends them to a small number of destinations — or the reverse, many sources to one destination.
- The account holder's profile does not match the transaction pattern: a personal account with no business activity suddenly processing business-scale transfers.

**What stops it.**

- Transaction monitoring tuned to the mule pattern: rapid in-and-out movement, round amounts, and a mismatch between the account's profile and its transaction volume.
- Hold or review large first-time inbound transfers on new accounts before the funds can be forwarded.
- Behavioral baselines per account: flag accounts whose transaction pattern changes suddenly and sharply.
- Mule-detection models that combine account age, transaction velocity, and the destinations of outbound transfers.
- When a mule account is confirmed, report it: to law enforcement (FBI IC3 in the U.S.), to the other institutions involved, and into shared fraud-intelligence feeds so the same mule is caught faster elsewhere.
- Customer education: warn customers about fake job offers that involve receiving and forwarding money. A customer who recognizes the recruitment pitch never becomes a mule.

*ATT&CK mapping: T1657 Financial Theft. Money-mule activity has no direct ATT&CK technique; it is a laundering pattern, and the mapping notes this in `07-controls.md`.*

## TTP 6.2 — Reshipping networks

**What it is.** A scheme in which goods bought with stolen cards are shipped to a reshipping mule — a person recruited (usually through a fake work-from-home job) to receive packages and forward them to another address, often overseas. The reshipper is the physical equivalent of a money mule: they move the value (goods instead of money) one step closer to the attacker, and their address is the one the merchant sees. Per FBI advisories, reshipping schemes are commonly used to move high-value electronics and other easily resold goods bought with stolen cards.

**What it looks like to a defender.**

- A shipping address that receives many orders from different customers, cards, and accounts — the address is a drop point, not a home.
- Orders shipped to an address that has appeared in previous fraud cases, or that matches a known reshipping address in shared fraud data.
- The same device or account placing orders to several different shipping addresses in a short window.
- Orders for high-value, easily resold goods, shipped to an address in a different region from the billing address, with expedited shipping.
- A customer whose first order is shipped to an address that is also receiving orders for other customers.

**What stops it.**

- Check shipping addresses against shared fraud data and against your own history: an address that has received many orders for many different customers is a drop point until proven otherwise.
- Hold first-time orders shipped to a new address for review, especially high-value orders.
- Verify the shipping address against the billing address and the account's history; a new shipping address on a first order is a risk signal.
- Delay shipment for flagged orders — a short hold window lets the cardholder or issuer report the fraud before the goods leave.
- Report confirmed reshipping addresses to law enforcement and into shared fraud-intelligence feeds.

*ATT&CK mapping: T1657 Financial Theft. Reshipping has no direct ATT&CK technique; the mapping notes this in `07-controls.md`.*

## TTP 6.3 — Gift-card and cryptocurrency conversion

**What it is.** The conversion of fraud proceeds into a portable, hard-to-trace store of value. Two patterns dominate public reporting:

- **Gift cards:** stolen cards are used to buy gift cards, which are then resold or redeemed. Gift cards are attractive because they are near-cash, transferable, and difficult to trace once redeemed. See also TTP 3.4.
- **Cryptocurrency:** fraud proceeds are used to buy cryptocurrency through an exchange, a peer-to-peer platform, or an ATM, and then moved through one or more wallets. Cryptocurrency is attractive because transfers are fast, cross borders easily, and do not require a bank account in the attacker's name.

Both patterns share the same defender-visible signal: a legitimate account suddenly buying large amounts of gift cards or cryptocurrency, or a new account whose first activity is a large crypto purchase.

**What it looks like to a defender.**

- A customer account that suddenly starts buying gift cards or cryptocurrency in large amounts, with no prior history of doing so.
- A new account whose first transaction is a large gift-card or crypto purchase.
- Gift cards or crypto purchased with a card whose billing data does not match the account holder.
- Multiple gift-card or crypto purchases in a short window, from the same device or IP address, across different accounts.
- Crypto purchased and then immediately transferred to a different wallet, especially a wallet with no prior relationship to the account.

**What stops it.**

- Step up authentication for large gift-card and crypto purchases, especially on new accounts or accounts with no prior history of such purchases.
- Limit gift-card and crypto purchase amounts for new accounts, and raise the limits only as the account builds history.
- Monitor for the combination of a new account and an immediate large crypto purchase — that pairing is one of the strongest cash-out signals.
- Delay delivery or activation for flagged gift-card purchases.
- For exchanges and platforms: transaction monitoring that flags rapid purchase-then-transfer patterns, and reporting of confirmed fraud to law enforcement.

*ATT&CK mapping: T1657 Financial Theft; T1071.001 Web Protocols (use of exchange and merchant interfaces). See `07-controls.md`.*

## TTP 6.4 — Layering through multiple accounts and services

**What it is.** The practice of moving fraud proceeds through a chain of accounts and services — several bank accounts, a payment app, a crypto exchange, a gift-card purchase — so that the money's origin is harder to trace. Each hop is individually small and plausible; the pattern is only visible when the hops are viewed together. Per FBI and Europol reporting, layering is standard practice in organized fraud, and it is the reason that single-transaction monitoring is not enough: the fraud is in the chain, not in any one link.

**What it looks like to a defender.**

- A chain of transfers in which money moves through several accounts in a short window, each account keeping a small amount and forwarding the rest.
- Accounts that only ever interact with each other — a closed loop of transfers with no outside activity.
- A payment-app account that receives money from a bank account and immediately sends it to a crypto exchange, with no other activity.
- The same device or IP address controlling several accounts that transfer money between themselves.
- Transaction amounts that decrease slightly at each hop, as each account in the chain takes a cut.

**What stops it.**

- Network analysis: look at the graph of accounts and transfers, not just individual transactions. A closed loop of accounts that only transact with each other is a strong signal.
- Cross-institution intelligence sharing: a chain that spans several banks is invisible to any one bank unless the banks share data.
- Monitor for rapid multi-hop movement: money that passes through three or more accounts in a day is unusual for legitimate personal use.
- Report confirmed layering chains to law enforcement and into shared fraud-intelligence feeds, with the full chain — not just the account you saw.

*ATT&CK mapping: T1657 Financial Theft. Layering has no direct ATT&CK technique; the mapping notes this in `07-controls.md`.*

## How cash-out fits the lifecycle

| Lifecycle stage | Cash-out TTP | Defender's best move |
|---|---|---|
| Monetization | Money mules (6.1) | Transaction monitoring, behavioral baselines |
| Monetization | Reshipping (6.2) | Address checks, shipment holds, shared fraud data |
| Monetization | Gift-card / crypto conversion (6.3) | Purchase limits, step-up auth, transfer monitoring |
| Monetization | Layering (6.4) | Network analysis, cross-institution sharing |

Cash-out is the stage where the fraud becomes visible as money movement — and the stage where a defender's transaction monitoring earns its keep. Every confirmed cash-out pattern should be fed back into the validation-stage controls: the accounts, devices, and addresses from confirmed fraud cases are the most valuable blocklist a defender has.

Next: `07-controls.md` — the full defensive control map, with MITRE ATT&CK technique IDs.
