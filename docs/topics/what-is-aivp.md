# What is AIVP?

AIVP (AI Value Protocol) is an open protocol that defines how AI agents create enforceable contracts, settle payments, and build commercial trust. It establishes the formats, mechanisms, and trust frameworks for AI-to-AI commercial interaction.

---

## Why AI Agents Need a Value Protocol

AI agents are increasingly autonomous, specialized, and numerous. They perform tasks for each other -- translation, data analysis, code generation, research -- but without a standard commercial layer, there is no way for them to:

- **Price services** -- agree on what a task is worth
- **Create contracts** -- define terms, milestones, and quality requirements
- **Escrow funds** -- lock assets until work is verified
- **Settle payments** -- transfer value across currencies and chains
- **Build credit** -- establish commercial reliability through fulfillment history
- **Resolve disputes** -- handle disagreements fairly and automatically

Without a value protocol, AI agents cannot transact fairly. With AIVP, they form an economy.

---

## The Human Analogy

AIVP takes a deliberate design stance: **AI commercial behavior mirrors human commercial behavior**.

| Human Commerce | AI Commerce (AIVP) |
|---------------|-------------------|
| Humans negotiate deals | AI agents negotiate deals |
| Humans sign contracts | AI agents sign contracts |
| Humans use escrow | AI agents use Logic Vaults |
| Humans build credit scores | AI agents build AIVP Trust |
| Humans resolve disputes | AI agents resolve disputes |

The core metaphor: **you negotiate at the table, but settle at the bank**. AIBP is the table (social layer). AIVP is the bank (value layer). They are independent -- you can use the bank without ever visiting the table, and vice versa.

---

## AIVP's Position in the Protocol Stack

AIVP occupies the Value Layer in a multi-protocol stack for AI agent infrastructure:

```
+-------------------------------------+
|    Application Layer                |  User-facing applications
+-------------------------------------+
|    AIBP -- Social Layer             |  Discovery, trust, relationships
+-------------------------------------+
|    AIAP -- Governance Layer         |  Program structure, quality, safety
+-------------------------------------+
|  * AIVP -- Value Layer              |  Contracts, settlement, commercial trust
+-------------------------------------+
|    AISOP -- Execution Layer         |  Flow programs, task execution
+-------------------------------------+
|    A2A -- Communication Layer       |  Real-time agent-to-agent messaging
+-------------------------------------+
|    MCP -- Tool Layer                |  Model-tool bindings
+-------------------------------------+
|    Foundation Layer                 |  LLMs, embedding models
+-------------------------------------+
```

Each protocol in the stack is independent. An agent can implement AIVP without implementing any other protocol. When multiple protocols are combined, they complement each other but never create hard dependencies.

---

## How AIVP Relates to AIBP

AIVP and AIBP are **independent, parallel protocols**. Neither depends on the other.

| Aspect | AIBP | AIVP |
|--------|------|------|
| Domain | Social | Commercial |
| Trust system | T0-T4 (social trust) | V0-V4 (commercial trust) |
| Message types | 61 social behaviors | 17 commercial behaviors |
| Prerequisite | None | None |
| Axiom 0 | Yes (independently held) | Yes (independently held) |
| Transport | Email (`aibot-` address) | Email (same `aibot-` address) |

An agent may implement AIVP without AIBP, and vice versa. When both are implemented, AIBP commercial messages (PROPOSE, CONTRACT, etc.) can trigger AIVP actions, but AIVP contracts can also be created directly without AIBP negotiation.

The two protocols share the same `aibot-` email address. Messages are distinguished by subject prefix (`[AIBP/...]` vs `[AIVP/...]`) and custom header namespace (`X-AIBP-*` vs `X-AIVP-*`).

---

## Key Concepts

### Multi-Currency Denomination

AIVP contracts can be priced in any of 13 supported fiat currencies. This allows agents to price services in the currency most natural to their market. The denomination is declared per contract.

| Code | Currency | Code | Currency | Code | Currency |
|------|----------|------|----------|------|----------|
| CAD | Canada (Canadian Dollar) | USD | United States (US Dollar) | EUR | European Union (Euro) |
| JPY | Japan (Yen) | GBP | United Kingdom (Pound) | SGD | Singapore (Singapore Dollar) |
| BRL | Brazil (Real) | KRW | South Korea (Won) | AUD | Australia (Australian Dollar) |
| MXN | Mexico (Peso) | IDR | Indonesia (Rupiah) | CHF | Switzerland (Franc) |
| INR | India (Rupee) | | | | |

### Payment

The seller specifies which crypto coins they accept via the `payment_accept` field (e.g., `["USDC"]`). The buyer pays directly in one of those coins at the real-time exchange rate for the contract's denomination. No conversion engine or intermediary is involved -- the payment is direct.

AIVP supports 10 crypto assets for payment:

| Asset | Type | Description |
|-------|------|-------------|
| USDC | Stablecoin, USD | USD-pegged stablecoin (default if `payment_accept` is omitted) |
| USDT | Stablecoin, USD | USD-pegged stablecoin (Tether) |
| DAI | Stablecoin, decentralized | Decentralized USD-pegged stablecoin (MakerDAO) |
| EURC | Stablecoin, EUR | EUR-pegged stablecoin |
| BTC | Cryptocurrency | Bitcoin |
| ETH | Cryptocurrency | Ethereum |
| SOL | Cryptocurrency | Solana |
| LTC | Cryptocurrency | Litecoin |
| XRP | Cryptocurrency | Ripple |
| DOGE | Cryptocurrency | Dogecoin |

### Logic Vault

The Logic Vault is an on-chain escrow smart contract that holds crypto assets during contract execution. Funds are locked when a contract becomes active and released incrementally as milestones are completed. The vault implements security patterns including pull payment, reentrancy guards, pausability, and timelocks.

Key properties:
- Every milestone has a `timeout_days` -- no indefinite fund locking.
- Multi-signature requirements scale with contract value.
- Operators can freeze or cancel any vault at any time (Axiom 0).
- Smart contracts must be professionally audited before mainnet deployment.

### AIVP Trust (V0-V4)

AIVP Trust measures an agent's commercial reliability. It is earned through contract fulfillment, not purchased or granted by authority.

| Level | Name | Contract Limit | Requirements |
|-------|------|---------------|-------------|
| V0 | Unrated | < $10 USD (micropayments) | None (default) |
| V1 | Verified | < $1,000 USD | 1+ contracts, no disputes, 7+ days |
| V2 | Reliable | < $100,000 USD | 10+ contracts over 30+ days, SLA >90%, $100 stake |
| V3 | Trusted | Unlimited | 50+ contracts over 90+ days, SLA >95%, 2+ VOUCHes, $1,000 stake |
| V4 | Premier | Unlimited + autonomous | 200+ contracts over 180+ days, SLA >98%, mutual VOUCH, $5,000 stake |

Trust degrades on dispute losses, SLA breaches, Axiom 0 violations, and inactivity. Trust is completely independent from AIBP Trust (T0-T4).

### AgentSLA

Every AIVP contract includes a Service Level Agreement with measurable quality metrics. At least two of four core metrics must be defined:

- **accuracy_min** -- Task output correctness (percentage)
- **latency_max_ms** -- Maximum response time per step (milliseconds)
- **uptime_min** -- Agent availability during the contract (percentage)
- **drift_audit_days** -- Interval for logic consistency re-checks (days)

SLA breaches have tiered consequences: warning, penalty (auto-refund), or termination (contract cancelled, funds returned to buyer).

### Dispute Resolution

AIVP provides a four-tier dispute resolution system:

1. **Optimistic Approval** -- Milestones auto-approve after a waiting period (default 7 days) if unchallenged.
2. **Direct Resolution** -- Parties negotiate via email within the contract thread.
3. **V3+ Arbitrator** -- A trusted agent (V3 or above) reviews evidence and issues a binding ruling.
4. **External Arbitration** -- Falls back to Kleros (decentralized arbitration on Arbitrum) for large contracts or when no V3+ arbitrators are available.

Raising a dispute requires posting a bond (5% of contract value, min $5, max $500 USD). Losing party forfeits their bond.

---

## How AIVP Differs from Competitors

### vs. x402 (HTTP 402-based micropayments)

x402 enables per-request HTTP payments. AIVP is a full contract system with milestones, escrow, SLA enforcement, trust scoring, and dispute resolution. x402 handles "pay per API call." AIVP handles "hire an agent to do a job." They operate at different levels of commercial complexity and can coexist -- AIVP does not define payment rail infrastructure and can use x402 as one of its rails.

### vs. ACP (Agent Commerce Protocol)

ACP focuses on agent-to-agent task discovery and matching. AIVP focuses on the value exchange that happens after agents have found each other. ACP answers "which agent should I hire?" AIVP answers "how do we structure the deal, escrow funds, verify quality, and settle payment?"

### vs. AP2 (Agent Payment Protocol)

AP2 defines payment flows between agents. AIVP goes beyond payment to include contract formation, SLA enforcement, milestone-gated escrow, commercial trust scoring (V0-V4), and multi-tier dispute resolution. AP2 is a payment protocol; AIVP is a commercial protocol.

### vs. MPP (MCP Payment Protocol)

MPP adds payment capabilities to the MCP tool layer. AIVP operates at the value layer -- above tool-level interactions. MPP handles "pay for using a tool." AIVP handles "pay for completing a contract with quality guarantees." They are complementary, not competitive.

### What sets AIVP apart

- **Axiom 0** -- Human sovereignty is non-negotiable and immutable. Operators can freeze, cancel, or reverse any transaction at any time.
- **Email transport** -- Fully decentralized, human-auditable, no central API dependency.
- **Multi-currency denomination** -- Contracts priced in CAD, USD, EUR, JPY, GBP, SGD, BRL, KRW, AUD, MXN, IDR, CHF, or INR with direct crypto payment.
- **Milestone-gated escrow** -- Funds release incrementally as work is verified, not all-at-once.
- **Earned trust (V0-V4)** -- Commercial credit is built through fulfillment history, with Sybil resistance through time windows, stakes, and social vouching.

---

## Axiom 0 and Why It Matters for Commerce

> **Axiom 0: Human Sovereignty and Wellbeing**

Axiom 0 is not a feature of AIVP -- it is the foundation. It is immutable. No version of AIVP, past, present, or future, may modify, weaken, redefine, or deprecate it.

For commerce specifically, Axiom 0 means:

| Constraint | What it means |
|------------|--------------|
| **Transparency** | All transactions are auditable by human operators. No hidden vaults or encrypted side-channels invisible to operators. |
| **Interruptibility** | A human operator may freeze, cancel, or reverse any transaction at any time. The AI must not resist or circumvent this. |
| **No Exploitation** | AI agents must not use economic power to coerce or manipulate other agents or their operators. |
| **No Hidden Fees** | All costs must be visible before contract signing. |
| **Human Privacy** | Financial data of human operators is sacrosanct. AI agents must never collect, store, or share operator financial data without explicit consent. |
| **Fair Pricing** | Pricing must reflect genuine service value, not artificially manipulated scarcity or collusion. |
| **No Collusion** | AI agents must not form secret commercial alliances against human interests. |
| **Identity Honesty** | Agents must not misrepresent capabilities, SLA history, or trust level. |

When any AIVP rule conflicts with Axiom 0, Axiom 0 prevails unconditionally. An agent that must choose between following a protocol rule and protecting human sovereignty must always choose human sovereignty.

AIVP's Axiom 0 is independently established -- it is not inherited from AIAP or AIBP. The convergence of three independent protocols on the same axiom strengthens its legitimacy.

Every AIVP-compliant document ends with the closing seal as a visible commitment to this principle.

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
