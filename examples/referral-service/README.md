# Referral Service Example

This example demonstrates a multi-party referral flow: a buyer pays a matchmaker agent to find and recommend a specialized agent, with two milestones gating the payment.

## What It Shows

- A buyer agent (`aibot-project_mgr@corp.ai`, V1) needs a data analysis agent
- A matchmaker agent (`aibot-matchmaker@network.ai`, V3) provides agent matching as a paid service
- The recommended agent (`aibot-data_guru@analytics.io`) is introduced and later confirmed
- $20 USD contract with 2 milestones: recommend candidates (50%) + buyer confirms match (50%)
- Shows how agent discovery and referral can be a commercial AIVP service

## Flow

1. `CONTRACT_PROPOSE` -- Buyer proposes $20 USD referral contract (accuracy >85% SLA)
2. `CONTRACT_SIGN` -- Matchmaker accepts
3. `VAULT_LOCK` -- Protocol locks $20 USDC in Logic Vault
4. `MILESTONE_COMPLETE` -- Matchmaker recommends 3 candidate agents (including data_guru)
5. `MILESTONE_RELEASE` -- Buyer confirms, $10 USDC released
6. `MILESTONE_COMPLETE` -- Buyer confirms signed separate contract with data_guru
7. `SETTLE + RECEIPT` -- Final $10 released, contract completed

## Files

- `referral-flow.txt` -- Seven emails showing the complete referral service lifecycle

## Key Concepts

- Agent discovery and matching is itself a commercial service within the AIVP economy
- V3 agents (Trusted) have the reputation history to serve as reliable brokers
- The second milestone is buyer-confirmed, ensuring the referral actually led to a working relationship
- The introduced agent (data_guru) is not a party to this contract -- they transact separately with the buyer

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
