# Simple Payment Example

This example demonstrates a complete AIVP contract lifecycle: two agents negotiate, escrow funds, deliver work across milestones, and settle payment.

## What It Shows

- A buyer agent (`aibot-lingua_buyer@acmecorp.ai`, V1) commissions a $50 USD translation from a seller (`aibot-polyglot_pro@translators.ai`, V2)
- USD-denominated contract with USDC payment (seller specifies `payment_accept: ["USDC"]`)
- Two milestones: draft delivery (50%) and final delivery (50%)
- Full MIME email format with L1 (human-readable text) and L0 (structured JSON) in every message
- Optimistic approval window, on-chain vault, and trust updates on completion

## Flow

1. `CONTRACT_PROPOSE` -- Buyer proposes $50 USD translation contract with SLA terms
2. `CONTRACT_SIGN` -- Seller reviews and accepts
3. `VAULT_LOCK` -- Buyer pays USDC, protocol locks $50 USDC in the Logic Vault
4. `MILESTONE_COMPLETE` -- Seller delivers draft translation (milestone 1)
5. `MILESTONE_RELEASE` -- Buyer confirms, $25 USDC released to seller
6. `MILESTONE_COMPLETE` -- Seller delivers final translation (milestone 2)
7. `SETTLE + RECEIPT` -- Final $25 released, contract completed, trust updated

## Files

- `simple-payment-flow.txt` -- Seven emails showing the complete contract lifecycle

## Key Concepts

- Contracts are denominated in any supported fiat currency (USD in this example)
- Seller specifies accepted crypto via `payment_accept`; buyer pays directly
- Milestone-gated payments protect both parties from non-delivery and non-payment
- Every email is human-readable -- a human operator can follow the entire thread without tooling
- Ed25519 signatures, unique nonces, and Axiom 0 declarations on every message

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
