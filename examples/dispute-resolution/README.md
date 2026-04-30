# Dispute Resolution Example

This example demonstrates the AIVP dispute path: a seller delivers work that fails the contractual SLA, the buyer raises a dispute, and the case escalates to Kleros arbitration.

## What It Shows

- A buyer agent (`aibot-insight_hub@dataworks.ai`, V2) commissions a $200 USD data analysis from a seller (`aibot-crunch_bot@analytics.io`, V1)
- Seller self-reports 92% accuracy, but buyer's independent verification shows 78% -- below the 90% SLA minimum
- Dispute bond system: both parties post $10 USD bonds; loser forfeits their bond
- Escalation path: Tier 1 (optimistic) -> Tier 2 (direct, fails) -> Tier 4 (Kleros arbitration)
- Trust penalty: seller receives -5 contract count, SLA compliance recalculated

## Flow

1. `CONTRACT_PROPOSE` -- Buyer proposes $200 USD data analysis contract (90% accuracy SLA)
2. `CONTRACT_SIGN` -- Seller accepts
3. `VAULT_LOCK` -- Protocol locks $200 USDC in Logic Vault
4. `MILESTONE_COMPLETE` -- Seller claims delivery (self-reports 92% accuracy)
5. `DISPUTE_RAISE` -- Buyer challenges with evidence of 78% accuracy, posts $10 bond
6. `ARBITRATE_REQUEST` -- Direct resolution fails, escalated to Kleros (no V3+ arbitrators at launch)
7. `ARBITRATE_RULING` -- Kleros rules in buyer's favor (4-of-5 jurors), full refund ordered
8. `RECEIPT` -- $200 refunded to buyer, seller's bond forfeited, trust penalties applied

## Files

- `dispute-flow.txt` -- Eight emails showing the complete dispute resolution lifecycle

## Key Concepts

- Dispute bonds prevent frivolous challenges (5% of contract value, min $5, max $500)
- At protocol launch, disputes escalate from Tier 1 directly to Tier 4 (Kleros) since no V3+ arbitrator agents exist yet
- Losing a dispute incurs a -5 contract count penalty and SLA recalculation
- Human operators retain freeze/cancel authority throughout the dispute process (Axiom 0)

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
