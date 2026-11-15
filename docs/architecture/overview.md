# BDPay platform architecture

## Delivery model
Start as a modular monolith in a pnpm/Turborepo monorepo. Keep provider-specific code behind adapters so a provider can be added without changing payment orchestration.

## Workspaces
- `apps/api`: NestJS API and business modules.
- Planned: `apps/merchant-web`, `apps/admin-web`, `apps/checkout`, `apps/developer-portal`.
- Planned shared packages: database schema, contracts, UI, JS/PHP SDKs.

## Core bounded contexts
Identity and RBAC; merchant onboarding/KYC; provider registry; payment orchestration; transactions and attempts; double-entry ledger; fees and commissions; settlement and reconciliation; refunds and disputes; signed webhooks; risk monitoring; audit trail; reporting.

## Payment invariants
1. A browser redirect, screenshot, or customer-supplied transaction reference is never proof of payment.
2. Verify payment status with the provider's authenticated server-to-server API.
3. Use idempotency keys and unique provider references to prevent duplicate charges and postings.
4. Treat timeouts and ambiguous provider responses as `pending_reconciliation`, never as success.
5. Post balanced, immutable ledger entries exactly once; correct mistakes with reversal entries.
6. Sign outbound webhooks, include timestamps, prevent replay, retry safely, and retain delivery logs.
7. Never request or store customer wallet PINs, OTPs, or passwords.
8. Keep merchant data isolated and audit privileged actions.

## Provider adapters
Define a stable provider contract for create, query/verify, refund (when supported), and webhook verification. bKash and Nagad integrations remain disabled until official credentials, API documentation, commercial approval, and callback verification are available. Tests must use a clearly labelled fake provider; it must never simulate real settlement.

## Production gate
Render hosting does not itself authorize payment processing. Production money movement requires the applicable regulatory authorization or a properly contracted licensed partner, provider approval, KYC/AML controls, security review, operational reconciliation, and incident procedures.
