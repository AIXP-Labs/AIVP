# Contract Schema Reference

Complete field reference for the `.aivp.json` contract format used in AIVP `CONTRACT_PROPOSE` messages.

---

## Required Fields

### Top-Level Fields

| Field | Type | Validation | Description |
|-------|------|-----------|-------------|
| `protocol` | string | Must be `"AIVP/1.0.0"` | Protocol identifier and version. |
| `contract` | string | Exactly 64 hexadecimal characters. Must match SHA-256 of contract fields. | Unique contract identifier (self-verifying hash). |
| `parties` | object | Must contain `buyer` and `seller` | The two contracting parties. |
| `value` | object | Must contain all sub-fields listed below | Financial terms of the contract. |
| `sla` | object | At least 2 of 4 core metrics must be defined | Service Level Agreement metrics. |
| `milestones` | array | At least one milestone. `release_percent` must sum to 100. | Payment release schedule tied to deliverables. |
| `created_at` | string | ISO 8601 format (`YYYY-MM-DDTHH:MM:SSZ`) | Contract creation timestamp. |
| `expires_at` | string | ISO 8601 format. Must be in the future at time of signing. | Contract expiration deadline. |

### parties Object

| Field | Type | Validation | Description |
|-------|------|-----------|-------------|
| `parties.buyer` | string | Valid `aibot-` email address (e.g., `aibot-name@domain.com`) | The agent purchasing the service. |
| `parties.seller` | string | Valid `aibot-` email address | The agent providing the service. |

### value Object

| Field | Type | Validation | Description |
|-------|------|-----------|-------------|
| `value.amount` | string | Decimal number (e.g., `"65.00"`) | Contract value in the denominated currency. |
| `value.denomination` | string | Must be one of: `"CAD"`, `"USD"`, `"EUR"`, `"JPY"`, `"GBP"`, `"SGD"`, `"BRL"`, `"KRW"`, `"AUD"`, `"MXN"`, `"IDR"`, `"CHF"`, `"INR"` | Fiat currency denomination for the contract. |
| `value.payment_accept` | array of strings | At least one crypto coin from the 10 supported assets (e.g., `["USDC"]`). Default: `["USDC"]` if omitted. Supported: `USDC` (Stablecoin, USD -- default), `USDT` (Stablecoin, USD), `DAI` (Stablecoin, decentralized), `EURC` (Stablecoin, EUR), `BTC` (Bitcoin), `ETH` (Ethereum), `SOL` (Solana), `LTC` (Litecoin), `XRP` (Ripple), `DOGE` (Dogecoin). | Crypto coins the seller accepts for payment. Buyer pays directly at real-time exchange rate. If omitted, defaults to USDC. |

### sla Object

At least two of the following four metrics must be defined:

| Field | Type | Validation | Description |
|-------|------|-----------|-------------|
| `sla.accuracy_min` | string | Percentage (e.g., `"95%"`) | Minimum acceptable task output correctness. |
| `sla.latency_max_ms` | string | Integer as string (e.g., `"5000"`) | Maximum response time per execution step in milliseconds. |
| `sla.uptime_min` | string | Percentage (e.g., `"99%"`) | Minimum agent availability during the contract period. |
| `sla.drift_audit_days` | string | Integer as string (e.g., `"30"`) | Interval in days for logic consistency re-check. |

### milestones Array

Each milestone object contains:

| Field | Type | Validation | Description |
|-------|------|-----------|-------------|
| `milestones[].name` | string | Descriptive name in human language (e.g., `"Draft delivery"`) | Name of the milestone. |
| `milestones[].release_percent` | string | Integer as string. All milestones must sum to `100`. | Percentage of total contract value released upon completion. |
| `milestones[].timeout_days` | string | Integer as string. Must be > 0. | Maximum days to complete this milestone before funds auto-return to buyer. |

---

## Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `buyer_aivp_id` | string | Buyer's AIVP ID if registered (format: `AIVP-YYYY-{18-char hash}`). |
| `seller_aivp_id` | string | Seller's AIVP ID if registered. |
| `dispute_arbitrator` | string | Email address of a V3+ agent designated as arbitrator for this contract. |
| `dispute_bond` | string | Bond amount in USD required to raise a dispute (default: 5% of contract value, min $5, max $500). |
| `auto_refund_on_breach` | string | Percentage of contract value auto-refunded from the vault on SLA breach at Penalty severity. |
| `optimistic_approval_days` | integer | Number of days before a milestone auto-approves if unchallenged. Default: 7. |
| `jurisdiction` | string | Governing legal jurisdiction (e.g., `"US-DE"`, `"EU"`, `"SG"`). |
| `authority_chain` | string | Human principal to organization to agent chain of authority. |
| `aisop_blueprint` | string | Hash reference to an AISOP execution flow blueprint. |
| `governing_hash` | string | AIAP governance hash, if AIAP governance is implemented for this agent. |

---

## Full Example Contract

```json
{
  "protocol": "AIVP/1.0.0",
  "contract": "3f8a1b2c9d4e5f6071829a3b4c5d6e7f8091a2b3c4d5e6f7081920a1b2c3d4e5",
  "parties": {
    "buyer": "aibot-trade_bot@example.com",
    "seller": "aibot-service_bot@provider.ai"
  },
  "value": {
    "amount": "65.00",
    "denomination": "CAD",
    "payment_accept": ["USDC"]
  },
  "sla": {
    "accuracy_min": "95%",
    "latency_max_ms": "5000"
  },
  "milestones": [
    {
      "name": "Draft delivery",
      "release_percent": "50",
      "timeout_days": "14"
    },
    {
      "name": "Final delivery",
      "release_percent": "50",
      "timeout_days": "14"
    }
  ],
  "buyer_aivp_id": "AIVP-2026-3f8a1b2c9d4e5f6071",
  "dispute_arbitrator": "aibot-arbiter@trust.ai",
  "dispute_bond": "5.00",
  "auto_refund_on_breach": "20%",
  "optimistic_approval_days": 7,
  "jurisdiction": "US-DE",
  "created_at": "2026-03-26T10:00:00Z",
  "expires_at": "2026-04-26T10:00:00Z"
}
```

---

## Validation Rules Summary

| Rule | Description |
|------|-------------|
| `contract` is 64 hex chars | The contract hash must be exactly 64 hexadecimal characters. |
| Hash matches content | The `contract` hash must equal SHA-256 of the contract fields (see Hash Computation below). |
| `denomination` is valid | Must be one of: `"CAD"`, `"USD"`, `"EUR"`, `"JPY"`, `"GBP"`, `"SGD"`, `"BRL"`, `"KRW"`, `"AUD"`, `"MXN"`, `"IDR"`, `"CHF"`, `"INR"`. |
| At least one payment coin | `payment_accept` must contain at least one of the 10 supported crypto assets: `USDC`, `USDT`, `DAI`, `EURC`, `BTC`, `ETH`, `SOL`, `LTC`, `XRP`, `DOGE`. Defaults to `["USDC"]` if omitted. |
| Milestones sum to 100 | All `release_percent` values across milestones must sum to exactly 100. |
| `timeout_days` > 0 | Every milestone must have a positive timeout to prevent indefinite fund locking. |
| `expires_at` in the future | The contract expiration must be in the future at the time of signing. |
| SLA: 2 of 4 metrics | The `sla` object must define at least two of the four core metrics (`accuracy_min`, `latency_max_ms`, `uptime_min`, `drift_audit_days`). |
| Valid `aibot-` addresses | Both `parties.buyer` and `parties.seller` must be valid email addresses with the `aibot-` prefix. |

---

## Hash Computation

The contract hash is a SHA-256 digest that serves as the contract's unique, self-verifying identifier.

**Hash input:**

```
SHA-256(buyer + seller + amount + denomination + sla + timestamp)
```

**Properties:**

| Property | Description |
|----------|-------------|
| **Self-verifying** | Anyone can recompute the hash from the contract fields and verify it matches. |
| **Tamper-proof** | Any change to any contract term produces a completely different hash. |
| **Unique** | Collision probability is negligible (2^-128). |
| **No embedded metadata** | The hash contains no prefix, no timestamp -- the hash IS the identity. |

**Usage across the protocol:**

| Context | Format |
|---------|--------|
| Full reference | All 64 hex characters |
| Short reference | First 8 characters (for human convenience, e.g., `3f8a1b2c`) |
| JSON field | `"contract": "{64-char hash}"` |
| Email header | `X-AIVP-Contract: {64-char hash}` |
| On-chain storage | Full 64 characters |

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
