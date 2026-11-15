# BDPay — Payment Aggregation Platform

A Bangladesh-focused, multi-merchant payment orchestration platform. The codebase is being built as a modular monolith with provider adapters, auditable financial records, and a Render deployment target.

## Current foundation
- pnpm + Turborepo workspace
- NestJS API bootstrap
- `GET /v1/health` health endpoint
- Strict TypeScript defaults
- GitHub Actions type-check workflow
- Architecture and payment-safety invariants

## Planned capabilities
Merchant onboarding and KYC, merchant/admin dashboards, hosted checkout, provider adapters (bKash and Nagad first subject to approval), transaction lifecycle, double-entry ledger, commissions, refunds, settlement, reconciliation, signed webhooks, reporting, and JS/PHP SDKs.

## Development
Requires Node.js 22+ and pnpm 10.

```bash
corepack enable
pnpm install
pnpm --filter @bdpay/api start:dev
```

API health check: `GET http://localhost:4000/v1/health`.

## Environment
Copy `.env.example` to `.env` and set local development values. Never commit secrets. Provider credentials must only be configured after formal provider onboarding.

## Important
This repository currently contains foundation code, not a live payment gateway. Fake/sandbox providers are for automated tests only. Do not accept real customer funds until the applicable regulatory requirements and provider contracts are satisfied and production security/reconciliation checks have passed.
