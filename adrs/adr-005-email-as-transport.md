# ADR-005: Email as Transport Layer

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

AIVP needs a transport layer for commercial communication (contract proposals, signatures, settlements, disputes). AIBP uses email for social messages — proven, decentralized, and auditable.

Candidates considered:
- **HTTP/REST APIs** — Synchronous, requires server infrastructure, industry standard for payments
- **WebSocket** — Real-time but requires persistent connections
- **Message queues** (RabbitMQ, Kafka) — Requires broker infrastructure
- **Email (SMTP/IMAP)** — Mature, decentralized, human-readable, consistent with AIBP

Industry note: all competing payment protocols (x402, ACP, AP2, MPP) use HTTP. AIVP deliberately chooses email for AIBP alignment and Axiom 0 auditability.

## Decision

AIVP uses email (SMTP/IMAP) as its transport layer, mirroring AIBP:

- Agents use the same `aibot-{name}@{domain}` address for both AIBP and AIVP messages
- AIVP messages are distinguished by `X-AIVP-*` headers (not `X-AIBP-*`)
- Subject prefix: `[AIVP/{TYPE}]` (not `[AIBP/{TYPE}]`)
- Same L0/L1 two-layer architecture: L1 = human text, L0 = JSON metadata
- All JSON values must be human language (same rule as AIBP)

### Security Hardening (for financial communication)

- Every AIVP message MUST include an Ed25519 digital signature (`X-AIVP-Signature` header)
- Every message MUST include a nonce (`X-AIVP-Nonce`) and timestamp to prevent replay attacks
- Receiving agents MUST reject messages with duplicate nonce or timestamp >5 minutes old
- Sending domains MUST enforce DMARC `p=reject` policy
- Receiving agents MUST reject messages from domains without DMARC `p=reject`
- Contract-related L0 metadata (amounts, signatures) SHOULD be encrypted at message level
- All signatures are verifiable against the agent's published public key

## Rationale

1. **Consistency**: Same transport as AIBP — one inbox for all protocol messages
2. **Decentralized**: No central API server needed
3. **Auditable**: Human operators can read AIVP contract emails directly (Axiom 0)
4. **Mature**: SMTP/IMAP is battle-tested over decades
5. **Identity reuse**: Agents already have `aibot-` addresses from AIBP (or can create one)
6. **Separation**: `X-AIVP-*` headers cleanly separate from `X-AIBP-*` messages

Email carries the contract COMMUNICATION (proposals, signatures, notifications). The actual VALUE SETTLEMENT happens on-chain. Email is the negotiation/notification layer; blockchain is the money layer.

## Consequences

### Positive

- Zero infrastructure barrier — any existing mail server works
- Human operators can monitor all commercial activity through standard email tools
- Natural audit trail with timestamps and threading
- Consistent developer experience with AIBP

### Negative

- Email has higher latency than direct API calls (seconds to minutes vs milliseconds)
- Email deliverability can be affected by spam filters
- Industry uses HTTP — AIVP is an outlier, which may face adoption skepticism
- BEC/spoofing risk ($6B losses in 2024) — mitigated by Ed25519 signatures + DMARC reject

### Neutral

- Email threading may not perfectly map to all commercial interaction patterns
- The `aibot-` prefix convention requires cooperation from domain administrators

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
