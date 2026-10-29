# Billing, payments and order-to-cash

Billing is where bugs cost real money and trust, so fintech and SaaS companies probe for domain depth: correctness under retries, money modelling, reconciliation and audit. Principal/Staff Engineer and Software Architect roles in fintech, payments and B2B SaaS ask most, and it is a strong differentiator for Forward Deployed Engineer roles with finance customers.

> Related: [System design](../topics/03-system-design/README.md) · [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md) · [Customer scenarios](../topics/09-customer-scenarios/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Walk through order-to-cash end to end: quote, order, provisioning, invoice, payment, collections, revenue.
- [ ] Why should you never store money as a floating-point number? What do you use instead?
- [ ] What is double-entry bookkeeping, and why does every transaction need balancing debits and credits?
- [ ] Authorisation vs. capture vs. settlement vs. payout for card payments.
- [ ] What is an idempotency key, and why does every payment API need one?
- [ ] Subscription concepts: plans, prices, billing cycles, trials, usage-based vs. seat-based pricing.
- [ ] What is proration? Give an example of a mid-cycle upgrade.
- [ ] Refund vs. credit note vs. chargeback.

## B. Working depth
- [ ] Model a subscription billing schema: customers, subscriptions, line items, invoices, payments, credits.
- [ ] Proration edge cases: downgrade mid-cycle, quantity changes, annual-to-monthly switch, time zones.
- [ ] How do you handle payment provider webhooks that arrive late, twice or out of order?
- [ ] Reconciliation: matching your ledger against the provider's settlement reports and the bank statement.
- [ ] Dunning: retry schedules, card updaters, grace periods and when to suspend access.
- [ ] Invoice immutability: why you never edit a finalised invoice, and what you do instead.
- [ ] Multi-currency: storing amounts with currency, FX rates at transaction time, rounding rules.
- [ ] Tax basics: GST in India (CGST/SGST/IGST, place of supply), VAT, tax-inclusive vs. exclusive prices *(verify)*.
- [ ] Usage-based billing: metering, aggregation, late-arriving usage events and invoice cut-off.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Revenue recognition basics: billing vs. revenue, deferred revenue, recognising an annual plan monthly (ASC 606 / IFRS 15 at a high level).
- [ ] Ledger design: append-only entries, accounts, balances as derived state, and correcting mistakes with reversals.
- [ ] Exactly-once effects for payments: idempotency keys, outbox pattern, and what happens when the provider times out.
- [ ] Build vs. buy for billing (Stripe Billing, Chargebee, Zuora, in-house): where do teams regret each choice? *(verify)*
- [ ] Migrating live customers from one billing system to another without double-charging or missing invoices.
- [ ] PCI-DSS basics: how tokenisation and hosted payment fields shrink your scope (SAQ A vs. SAQ D) *(verify)*.
- [ ] India-specific payment rules: recurring mandates, RBI e-mandate and tokenisation rules, UPI Autopay *(verify)*.
- [ ] Auditability: how do you answer "why was this customer charged this amount" two years later?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design a subscription billing system for a B2B SaaS with seats, usage add-ons, annual and monthly plans.
- [ ] A bug double-charged 2,000 customers last night. Walk through detection, refunds, communication and prevention.
- [ ] Design a daily reconciliation job between your ledger, a payment provider and bank statements; how are mismatches handled?
- [ ] Design a payment flow that survives provider timeouts, client retries and webhook delays without double-charging.
- [ ] Finance says monthly revenue in the dashboard does not match the books. How do you investigate?

## E. Hands-on checks (things to be able to DO)
- [ ] Write a function that prorates a plan change mid-cycle, with tests for leap years, time zones and rounding.
- [ ] Implement an idempotent `POST /payments` endpoint with an idempotency-key table in PostgreSQL.
- [ ] Write SQL that checks a double-entry ledger balances and finds unmatched settlement lines.
- [ ] Handle a payment provider webhook in sandbox mode with signature verification and deduplication *(verify)*.

---

**Good answers show:**
- Correctness and auditability first: immutable records, reversals instead of edits, a clear trail for every amount.
- Fluency in finance vocabulary, so you can work directly with finance and accounting teams.
- Idempotency and reconciliation treated as core design, not afterthoughts.
- Awareness of local rules (tax, payment regulation) and knowing when to bring in experts.
- Realistic migration and incident stories with customer impact and how it was contained.
- Pragmatic build-vs-buy judgement based on pricing complexity and team size.

**Prove it:** Build a mini billing engine in TypeScript and PostgreSQL: plans, subscriptions with proration, invoices, an append-only double-entry ledger, an idempotent payment endpoint wired to a payment provider's sandbox with webhook handling, and a nightly reconciliation report. Write tests for the nastiest proration and retry cases.
