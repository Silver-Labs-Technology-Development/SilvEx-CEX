# Security policy

## Scope

This repository is a **planned development scaffold** for a centralized
exchange. There is no runnable service, no custody, no customer data, and no
authorization for real-value trading.

## Reporting

Do not open a public issue containing exploit details, credentials, customer
data, or a working withdrawal bypass. Report privately to the Silver Labs
maintainers through the organization account
<https://github.com/Silver-Labs-Technology-Development> and allow time for a
fix before disclosure.

## Security boundaries for future work

- Customer funds, keys, and withdrawal approvals must never live in application
  source or logs.
- Every balance change is a double-entry posting with an idempotency key.
- Withdrawals require dual control, limits, and reconciliation.
- Admin actions require strong authentication, authorization, and audit logs.
- Test environments use deterministic fake assets and fake compliance
  providers. Never copy production data into this repository.
- Never commit `.env` files, wallet data, database dumps, or KYC material.

## Known limitations

- No implementation, test suite, independent audit, or license exists.
- No custody, ledger, or matching design has been validated in operation.
