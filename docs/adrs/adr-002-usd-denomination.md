> **Source of truth:** This document is mirrored for the documentation site. The authoritative version lives in [`/adrs/adr-002-usd-denomination.md`](https://github.com/AIXP-Labs/AIVP/blob/main/adrs/adr-002-usd-denomination.md). Edits MUST go to the authoritative file first.

# ADR-002: Multi-Currency Denomination with Direct Crypto Payment

## Status

Accepted

## Date

2026-03-26

## Deciders

AIVP Protocol Authors

## Axiom 0 Compliance

| Check | Result |
|-------|--------|
| Human Sovereignty preserved | Yes |
| Human Benefit demonstrated | Yes |
| Transparency maintained | Yes |
| Interruptibility preserved | Yes |
| No manipulation or collusion | Yes |

## Context

AIVP needs a pricing denomination for contracts. AI commerce is global — sellers and buyers operate across different countries and currencies.

Key considerations:
- USD-only denomination forces non-US participants to mentally convert prices, adding friction
- Global AI commerce requires supporting multiple major fiat currencies
- Single-stablecoin dependency is a systemic risk for escrowed funds
- Intent-based conversion via a Solver engine adds architectural complexity and smart contract attack surface
- Depeg circuit breakers add significant protocol complexity for marginal safety benefit
- Sellers are best positioned to choose which crypto assets they are willing to hold

Candidates considered: USD-only denomination, multi-currency denomination, custom unit (AIV), compute-anchored unit, floating token.

## Decision

AIVP contracts are denominated in the seller's chosen fiat currency from a supported set. Payment occurs directly in seller-specified crypto:

- **Denomination**: one of CAD, USD, EUR, JPY, GBP, SGD, BRL, KRW, AUD, MXN, IDR, CHF, INR (seller's choice of fiat unit)
- **Payment**: seller specifies accepted crypto assets via `payment_accept` (e.g., `["USDC"]`)
- **Buyer payment**: buyer pays directly in a coin from `payment_accept` at the real-time exchange rate (denomination currency / crypto) at the moment of payment
- **Logic Vault**: holds the same crypto asset the buyer paid in
- **Milestone release**: releases the same crypto asset to the seller
- **No Solver**: no intermediary conversion engine — buyer pays directly in seller's accepted coin
- **No depeg circuit breaker**: seller bears the risk of the crypto assets they choose to accept

## Rationale

1. **Global accessibility** — sellers price in their local fiat currency (CAD, EUR, JPY, etc.), reducing cognitive overhead for international commerce
2. **Simplified architecture** — no Solver engine, no conversion pipeline, no slippage management, no depeg circuit breaker
3. **Reduced smart contract surface** — fewer moving parts means fewer vulnerabilities
4. **Seller autonomy** — seller controls which crypto they receive and bears the associated risk
5. **No single-currency dependency** — supporting 13 fiat denominations prevents USD-centric lock-in
6. **Industry aligned** — international trade commonly uses local currency denomination with flexible payment methods

## Consequences

### Positive

- Supports global commerce across 13 major fiat currencies
- Dramatically simplified payment flow (no Solver, no conversion, no slippage)
- Reduced smart contract attack surface
- Seller has full control over accepted payment assets
- Fewer protocol message types needed (no DEPEG_HALT)

### Negative

- Requires price oracle for denomination-currency/crypto exchange rates
- Seller bears full risk of accepted crypto asset fluctuation or depeg
- No automatic depeg protection — seller must manage their own risk via diversified `payment_accept` lists

### Neutral

- Contract amounts are stable in the chosen fiat denomination regardless of crypto market volatility
- The supported denomination list (CAD, USD, EUR, JPY, GBP, SGD, BRL, KRW, AUD, MXN, IDR, CHF, INR) may expand in future versions

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
