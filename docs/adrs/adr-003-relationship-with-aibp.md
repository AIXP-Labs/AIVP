> **Source of truth:** This document is mirrored for the documentation site. The authoritative version lives in [`/adrs/adr-003-relationship-with-aibp.md`](https://github.com/AIXP-Labs/AIVP/blob/main/adrs/adr-003-relationship-with-aibp.md). Edits MUST go to the authoritative file first.

# ADR-003: Independent Relationship with AIBP

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

AIBP defines social trust (T0-T4) and 9 commercial message types (PROPOSE, COUNTER, ACCEPT, REJECT, CONTRACT, INVOICE, RECEIPT, DISPUTE, ARBITRATE). AIVP defines commercial trust (V0-V4) and contract settlement.

Question: must an agent have AIBP social trust before using AIVP? Does AIVP depend on AIBP?

## Decision

AIVP and AIBP are independent parallel protocols. Neither is prerequisite for the other.

- An agent may use AIVP without any AIBP implementation
- An agent may use AIBP without any AIVP implementation
- When both are implemented, they complement each other but neither gates the other
- AIBP commercial messages (PROPOSE, CONTRACT, etc.) can trigger AIVP actions, but AIVP contracts can also be created directly without AIBP
- AIVP Trust (V0-V4) and AIBP Trust (T0-T4) are independent scoring systems

## Rationale

1. **Protocol sovereignty**: Each protocol must stand alone
2. **Lower barrier to entry**: Requiring AIBP would exclude agents that only need commerce
3. **Broader adoption**: AIVP should work with any social/discovery layer, not just AIBP
4. **Same philosophy as Axiom 0 independence**: Convergent design, not dependency

## Consequences

### Positive

- AIVP is fully self-contained — no external protocol dependencies
- Agents can adopt incrementally (AIVP only, AIBP only, or both)
- Clear intellectual boundary between social and commercial layers

### Negative

- Requires explicit documentation to prevent confusion about the relationship
- Both protocols must independently define overlapping concepts (e.g., agent addressing)

### Neutral

- When both are implemented, significant divergence between AIBP Trust and AIVP Trust generates an anomaly alert (not an automatic block)

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
