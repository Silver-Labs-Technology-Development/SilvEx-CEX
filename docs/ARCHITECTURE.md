# SilvEx CEX architecture

This document describes the intended centralized-exchange boundary. It is a
design note, not a description of running software. No component is
implemented.

## Design principles

1. **The ledger is the source of truth for balances.** Chain state is the
   source of truth for deposits and withdrawals.
2. **Every balance change is a double-entry posting.** No component edits a
   balance directly.
3. **Matching is deterministic and replayable.** The same ordered input must
   produce the same fills.
4. **Withdrawals are a separate, approved workflow.** Placing an order never
   moves funds off-platform.
5. **Custody is isolated.** Application code proposes transfers; a separate
   signer or custody system authorizes them under policy.
6. **Fail closed.** On ledger, reconciliation, or risk uncertainty, stop
   withdrawals and new risk.

## Proposed data flow

```text
client ──► API ──► order service ──► matching engine ──► ledger postings
                │                                          │
                └──► risk / limits                         ▼
                                                    balances and holds

chain ──► deposit watcher ──► confirmation policy ──► ledger credit
ledger debit ──► withdrawal proposal ──► approval ──► custody signer ──► chain
```

## Components

### Ledger

Double-entry accounts for user balances, holds, fees, and operator accounts.
Every posting references a unique business key so retries cannot double-credit.
Reconciliation compares ledger obligations with on-chain custody balances.

### Matching engine

Single-threaded or partitioned by market, consuming an ordered command log.
Orders have explicit lifecycle states and cannot transition backwards. The
engine emits fills and ledger instructions; it does not write balances itself.

### Order service

Validates symbol, side, type, quantity, price, precision, and limits. Enforces
idempotency keys so a retried request cannot create a second order.

### Custody adapters

Watch-only chain ingestion allocates deposit addresses and applies a
confirmation policy. Withdrawals create a proposal with amount, destination,
fee, and risk result; a separate approval and signer complete it.

### Risk and compliance

Limits by account, market, and time window. External KYC/AML and sanctions
providers are integrated behind interfaces so test environments can use
deterministic fakes. Production compliance is an operator responsibility.

### Admin and audit

Operator actions are authenticated, authorized, and logged. Sensitive values
are redacted. Audit exports are immutable and reproducible.

## Open decisions

- Asset list and which BTCS L1/L2 assets are eligible.
- Custody model: in-house signer, third-party custodian, or multisig.
- Jurisdiction, licensing, and KYC/AML provider.
- Ledger database and the reconciliation cadence.
- Market-maker and liquidity policy.
- Fee schedule and whether fees are charged in BTCS or quote asset.

## Threat model (initial)

| Risk | Impact | Initial control |
| --- | --- | --- |
| Double-spend of internal balance | Insolvency | Atomic double-entry postings and idempotency |
| Withdrawal key compromise | Loss of funds | Isolated custody, policy limits, multisig |
| Matching bug | Wrong fills | Deterministic replay and property tests |
| Deposit reorg | Over-credit | Confirmation policy and reorg reversal |
| Insider admin abuse | Fraud | Least privilege, dual control, audit logs |
| Market manipulation | Customer loss | Surveillance hooks and listing policy |

## Non-goals

SilvEx CEX does not provide non-custodial trading, wallet key management, or
consensus. Those boundaries belong to `SilvEx-DEX`, `SilverPay`, and
`btcs-core`.
