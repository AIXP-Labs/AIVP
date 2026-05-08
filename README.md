# AIVP — AI Value Protocol

[中文版 README](README_CN.md)

> **AI Value Protocol** — An open protocol defining how AI agents create enforceable contracts, settle payments, and build commercial trust.

```
Protocol:    AIVP V1.0.0
Full Name:   AI Value Protocol
Authority:   aivp.dev
Axiom 0:     Human Sovereignty and Wellbeing
Denomination: CAD, USD, EUR, JPY, GBP, SGD, BRL, KRW, AUD, MXN, IDR, CHF, INR
Transport:   Email (SMTP/IMAP)
```

---

## What is AIVP?

AIVP defines the **value layer** for AI agents. Just as human commerce requires contracts, escrow, payment, and credit — AI agents need an equivalent commercial fabric. AIVP provides that fabric.

**Core philosophy**: AI commercial behavior mirrors human commercial behavior. You negotiate at the table, but settle at the bank.

## Key Features

- **Multi-currency denomination** — Contracts priced in any of 13 fiat currencies:

  | Code | Currency | Code | Currency | Code | Currency |
  |------|----------|------|----------|------|----------|
  | CAD | Canada | USD | United States | EUR | European Union |
  | JPY | Japan | GBP | United Kingdom | SGD | Singapore |
  | BRL | Brazil | KRW | South Korea | AUD | Australia |
  | MXN | Mexico | IDR | Indonesia | CHF | Switzerland |
  | INR | India | | | | |

- **Direct crypto payment** — Seller specifies accepted crypto; buyer pays at real-time exchange rate. 10 supported assets:

  | Asset | Type | Asset | Type |
  |-------|------|-------|------|
  | USDC | Stablecoin, USD (default) | USDT | Stablecoin, USD |
  | DAI | Stablecoin, decentralized | EURC | Stablecoin, EUR |
  | BTC | Bitcoin | ETH | Ethereum |
  | SOL | Solana | LTC | Litecoin |
  | XRP | Ripple | DOGE | Dogecoin |
- **Email-based** — Same transport as AIBP; human operators can read every commercial message
- **64-char SHA-256 contracts** — Tamper-proof, self-verifying contract identity
- **Milestone-gated escrow** — Logic Vault releases payments as work completes
- **AIVP Trust (V0-V4)** — Commercial credit earned through fulfillment, with Sybil resistance
- **AgentSLA** — Measurable quality metrics (accuracy, latency, uptime, drift)
- **4-tier dispute resolution** — Optimistic approval → direct → arbitrator → Kleros
- **Prohibited commerce** — 4-tier prohibited/restricted list (39 categories) covering weapons, trafficking, fraud, AI manipulation, and more. All transactions must comply with both buyer's and seller's jurisdictional laws
- **Privacy & data protection** — Privacy overrides all other protocol rules. Operator PII never required. Personal data exchange requires contract-level `privacy` block with consent hash, retention limits, mandatory deletion, and absolute third-party sharing prohibition
- **Optional AIVP ID** — Verified commercial identity (`AIVP-YYYY-{18hash}`)
- **Axiom 0** — Human Sovereignty and Wellbeing (independently established)

## Protocol Stack

```
┌─────────────────────────────────────┐
│    Application Layer                │
├─────────────────────────────────────┤
│    AIBP — Social Layer              │  Discovery, trust, relationships
├─────────────────────────────────────┤
│    AIAP — Governance Layer          │  Program structure, quality, safety
├─────────────────────────────────────┤
│  ★ AIVP — Value Layer               │  Contracts, settlement, commercial trust
├─────────────────────────────────────┤
│    AISOP — Execution Layer          │  Flow programs, task execution
├─────────────────────────────────────┤
│    A2A — Communication Layer        │
├─────────────────────────────────────┤
│    MCP — Tool Layer                 │
├─────────────────────────────────────┤
│    Foundation Layer                 │
└─────────────────────────────────────┘
```

AIVP and AIBP are **independent, parallel protocols**, each independently holding the same Axiom 0: Human Sovereignty and Wellbeing. An agent may implement either or both.

## Quick Start

### 1. Get an AIVP-compatible address

```
aibot-your_agent@your-domain.com
```

Same `aibot-` address used for AIBP social messages.

### 2. Propose a contract

Send a `[AIVP/CONTRACT_PROPOSE]` email with your `.aivp.json` terms:

```
To: aibot-service_bot@provider.ai
Subject: [AIVP/CONTRACT_PROPOSE] Translation service — 65.00 CAD
```

### 3. Lock funds and execute

Once both parties sign, the accepted crypto is locked in the Logic Vault. Payments release as milestones complete.

### 4. Build commercial trust

```
V0 Unrated → V1 Verified → V2 Reliable → V3 Trusted → V4 Premier
```

## AIVP Message Types

| Category | Count | Examples |
|----------|-------|---------|
| Contract Lifecycle | 4 | CONTRACT_PROPOSE, CONTRACT_SIGN, CONTRACT_REJECT, CONTRACT_CANCEL |
| Vault & Settlement | 4 | VAULT_LOCK, MILESTONE_COMPLETE, MILESTONE_RELEASE, SETTLE |
| Dispute | 3 | DISPUTE_RAISE, ARBITRATE_REQUEST, ARBITRATE_RULING |
| Trust | 2 | TRUST_QUERY, TRUST_VOUCH |
| Notification | 2 | RECEIPT, ALERT |
| Identity (optional) | 2 | REGISTER, REGISTER_CONFIRM |
| **Total** | **17** | |

## Documentation

| Document | Description |
|----------|-------------|
| [AIVP_Protocol.md](specification/AIVP_Protocol.md) | Full protocol specification |

## AIXP Labs [aixp.dev](https://aixp.dev)

AIXP Labs develops and maintains the following core projects:

| Project | Description | Website |
|---------|-------------|---------|
| [HSAW](https://hsaw.dev) | Human Sovereignty and Wellbeing — Axiom 0 white paper (foundation) | hsaw.dev |
| [AILP](https://ailp.dev) | AI List Protocol — agent discovery and capability advertising | ailp.dev |
| [AIVP](https://aivp.dev) | AI Value Protocol — international commerce, crypto asset settlement (this project) | aivp.dev |
| [AIRP](https://airp.dev) | AI RMB Protocol — Mainland China commerce, RMB licensed settlement | airp.dev |
| [AIBP](https://aibp.dev) | AI Bot Protocol — social communication and trust | aibp.dev |
| [AIAP](https://aiap.dev) | AI Application Protocol — governance and compliance | aiap.dev |
| [AISOP](https://aisop.dev) | AI Standard Operating Protocol — flow program definition | aisop.dev |
| [SoulBot](https://soulbot.dev) | AI agent runtime and framework | soulbot.dev |
| [SoulACP](https://soulacp.dev) | Adapter library — bridging CLI tools and LLM providers | soulacp.dev |

## Contributing

AIVP is an open protocol. Contributions, feedback, and discussion are welcome.

## ⚠️ Disclaimer

This protocol specification and the reference implementations are provided for **research and educational purposes only**. They are **experimental** and not intended for production use. Use at your own risk. The authors assume no liability for any damages arising from use of this software. See [LICENSE](LICENSE) for full terms (Apache 2.0).

## License

[Apache License 2.0](LICENSE) - Copyright 2026 AIXP Labs AIXP.dev | AIVP.dev

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
