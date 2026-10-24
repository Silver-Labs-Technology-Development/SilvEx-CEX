# SilvEx CEX

Centralized exchange services for the Silver Labs ecosystem.

**Status: planned — scaffold only.** No account ledger, order book, matching
engine, custody adapter, or admin surface exists in this repository yet. This
project is a high-risk custody product and must not be operated with real funds
without legal, security, and operations review.

## Purpose

Provide a centralized venue where users can trade listed assets against an
operator-managed ledger. The exchange owns the order book and the account
balances; on-chain settlement happens only at deposit and withdrawal
boundaries.

## Scope

Planned work, in dependency order:

1. **Accounts and ledger** — a double-entry ledger with idempotent transfers,
   holds, fees, and an audit trail.
2. **Order lifecycle** — validation, placement, cancel, expiry, and partial
   fills with deterministic identifiers.
3. **Matching engine** — price-time priority matching with replayable input and
   output logs.
4. **Market data** — public ticker, trades, and depth derived from the engine.
5. **Deposit and withdrawal adapters** — chain watchers, confirmation policy,
   address allocation, and operator-approved withdrawals.
6. **Risk and compliance** — limits, velocity checks, sanctions/KYC hooks, and
   suspicious-activity reporting interfaces.
7. **Operations** — reconciliation, cold/hot wallet policy, incident runbooks,
   metrics, and audit exports.

## Out of scope

- Non-custodial swaps or wallet-authorized settlement (that is `SilvEx-DEX`
  and `SilvEx-DEX-Node`).
- Consensus changes or a replacement BTCS node.
- A consumer wallet or payment processor (that is `SilverPay`).
- Real-value operation before licensing, custody, and audit decisions.

## Planned components

| Component | Responsibility | Notes |
| --- | --- | --- |
| `ledger/` | Double-entry balances, holds, fees | Money-critical |
| `matching/` | Deterministic order matching | Replayable logs |
| `orders/` | Order validation and lifecycle | Idempotent API |
| `marketdata/` | Ticker, trades, depth | Public, read-only |
| `custody/` | Deposit watchers and withdrawal proposals | No keys in app code |
| `risk/` | Limits and compliance hooks | External providers |
| `admin/` | Operator controls and audit views | Strong auth required |
| `api/` | Client-facing endpoints | Authenticated |

The directory names are proposed. They are not created yet.

## Integration points

- [btcs-core](https://github.com/Silver-Labs-Technology-Development/Silver-Labs-Bitcoin-Silver-Development)
  — BTCS L1 deposits, withdrawals, and confirmation policy.
- [btcs-l2](https://github.com/Silver-Labs-Technology-Development/btcs-l2)
  — L2 asset deposits and withdrawals, if listed.
- [SilverPay](https://github.com/Silver-Labs-Technology-Development/SilverPay)
  — optional client-side wallet connection, never custody.
- [Silver-Scan](https://github.com/Silver-Labs-Technology-Development/Silver-Scan)
  — public transaction references and operator reconciliation.
- [silver-labs-platform](https://github.com/Silver-Labs-Technology-Development/silver-labs-platform)
  — shared schemas and client conventions.

## Local development

There is no runnable code yet. Future development must use a disposable local
ledger and test assets only. No mainnet keys, real customer data, or production
wallet files may be used in this repository.

## Safety and release status

This is a development scaffold. It is **not** a licensed exchange, not a
custody service, and not authorized for real-value trading. Required before any
real-value use:

- Legal and regulatory review for each operating jurisdiction.
- Independent security audit of ledger, matching, custody, and withdrawal
  paths.
- KYC/AML and sanctions program, with an accountable operator.
- Proof-of-reserves and reconciliation procedures.
- Incident response and withdrawal-limit policy.

See [`SECURITY.md`](SECURITY.md).

## Repository layout

```text
.
├── README.md
├── LICENSE
├── SECURITY.md
├── .gitignore
└── docs/
    └── ARCHITECTURE.md
```

## License

No open-source license is granted. See [`LICENSE`](LICENSE). The repository is
public for review and development, not for reuse or redistribution.

## Related repositories

- [SilvEx DEX](https://github.com/Silver-Labs-Technology-Development/SilvEx-DEX)
- [SilvEx DEX Node](https://github.com/Silver-Labs-Technology-Development/SilvEx-DEX-Node)
- [Silver Labs workspace](https://github.com/orgs/Silver-Labs-Technology-Development/repositories)
