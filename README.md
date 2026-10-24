# SilvEx CEX

> **Status: Planned — repository and scope only.**
> This repository contains no exchange implementation. There are no accounts,
> custody, order books, matching, deposits, withdrawals, trading, or real-fund
> capabilities here.

## Purpose

SilvEx CEX is the centralized-exchange product boundary for Silver Labs. It is
intended to cover account and ledger services, order matching, custody
integration, deposit/withdrawal operations, risk controls, and the operational
tooling a centralized venue requires.

It is a separate product from SilvEx DEX and SilvEx DEX Node. Centralized
custody and on-chain, wallet-authorized swaps must not be presented as the same
system.

## Proposed scope

- Account, identity, and ledger model.
- Order lifecycle and matching-engine design.
- Deposit and withdrawal workflows with explicit custody and reconciliation
  rules.
- Market-data and trade-history services.
- Administrative, risk, and audit controls.
- Test environments and reproducible operational runbooks.

## Explicitly out of scope until reviewed and approved

- Real-fund custody or trading.
- Mainnet deployment.
- Any claim of regulatory compliance, solvency, insurance, or audit status.
- Reuse of upstream exchange code without a license and security review.

## Integration points

- **BTCS Core** — L1 RPC and deposit/withdrawal observation.
- **SilverPay** — account, wallet, and signing boundaries where applicable.
- **silver-labs-platform** — shared schemas and integration contracts.
- **Silver Scan** — transaction and address visibility.
- **SilvEx DEX / DEX Node** — separate products; integration is not implied.

## Decisions required before implementation

1. Build, fork, or integrate an existing engine, with a documented license and
   security review.
2. Custody model, key management, segregation, and reconciliation rules.
3. Jurisdiction, identity, AML/KYC, and record-retention requirements.
4. Matching-engine semantics, ledger invariants, and failure recovery.
5. Threat model, incident response, and independent security review plan.

OpenCEX has been identified only as an upstream candidate. It is Apache-2.0,
described by its maintainers as unmaintained and as-is, and has **not** been
adopted or copied into this repository.

## First milestone

A reviewed architecture decision and threat model, followed by a local
simulation with test accounts. No real funds and no public trading interface.

## Repository facts

- Local workspace path: `~/Silver-Labs/SilvEx-CEX`
- Default branch: `main`
- Implementation: none
- Tests run: none (no code)
- License: no license selected; no reuse rights are granted
- Visibility: public planning repository

This repository is labeled planned so that its existence is not mistaken for a
working exchange.
