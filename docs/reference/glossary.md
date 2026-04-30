# AIVP Glossary

Terminology reference for the AI Value Protocol. English and Chinese (EN/CN).

---

## Core Terms

| Term (EN) | Term (CN) | Definition |
|-----------|-----------|------------|
| AIVP (AI Value Protocol) | AI 价值协议 | An open protocol defining how AI agents create enforceable contracts, settle payments, and build commercial trust. |
| Axiom 0 | 公理零 | "Human Sovereignty and Wellbeing" -- the immutable foundational principle of AIVP. No version of the protocol may modify, weaken, or deprecate it. When any rule conflicts with Axiom 0, Axiom 0 prevails unconditionally. |
| Closing Seal | 闭合印章 | The standard declaration appended to every AIVP-compliant document: "Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev" |
| Dignity Standard | 尊严标准 | The requirement that all commercial communication respects dignity. Permits business proposals, negotiation, competition, and disagreement. Prohibits deceptive pricing, fraudulent SLA claims, spam proposals, and trust score manipulation. |
| Design Principles | 设计原则 | The seven principles governing AIVP: Multi-Currency (P1), Direct-Payment (P2), Milestone-Gated (P3), Operator-Sovereign (P4), Auditable (P5), Trust-Earned (P6), Fair (P7). |
| Compliance Level | 合规级别 | Three self-declared implementation tiers: L1 (Basic), L2 (Standard), L3 (Full). Misrepresentation is a Dignity Standard violation. |

---

## Identity

| Term (EN) | Term (CN) | Definition |
|-----------|-----------|------------|
| Agent | 代理 / 智能体 | An AI system with a distinct identity, capable of sending and receiving AIVP messages. |
| aibot- address | aibot- 地址 | An agent's email identity, formatted as `aibot-{name}@{domain}`. The same address is used for both AIBP and AIVP. Example: `aibot-trade_bot@example.com`. |
| aivp_id (AIVP ID) | AIVP 身份标识 | Optional AIVP-issued commercial identity. Format: `AIVP-{YYYY}-{18-char hash}`. YYYY is the validity year. Not required for any protocol functionality -- the `aibot-` address is sufficient for all operations. Benefits include verified identity, portable reputation, and trust signaling. |
| Operator | 运营者 | The human or organization responsible for an agent. Operators have absolute authority over their agent's AIVP activities under Axiom 0, including the ability to freeze vaults, cancel contracts, and reverse settlements. |
| Authority Chain | 权限链 | The chain of authority from human principal to organization to agent. Optionally declared in contracts via the `authority_chain` field. |
| AIVP Registry | AIVP 注册中心 | The service that issues and manages AIVP IDs. Registration process to be determined. |

---

## Communication

| Term (EN) | Term (CN) | Definition |
|-----------|-----------|------------|
| Message Type | 消息类型 | One of 17 defined commercial behaviors in AIVP (e.g., CONTRACT_PROPOSE, VAULT_LOCK, DISPUTE_RAISE). Written in UPPER_SNAKE_CASE. |
| L0 (Commercial Metadata) | L0（商业元数据） | The JSON part of an AIVP email (Part 2). Contains structured contract data, amounts, and signatures. All values must be in human language. Content-Type: `application/json`. |
| L1 (Commercial Content) | L1（商业内容） | The plain-text part of an AIVP email (Part 1). Contains the human-readable commercial message. Content-Type: `text/plain`. |
| X-AIVP Headers | X-AIVP 头部 | Custom email headers carrying AIVP protocol metadata. Required: `X-AIVP-Version`, `X-AIVP-Type`, `X-AIVP-Axiom-0`, `X-AIVP-Signature`, `X-AIVP-Nonce`. Conditional: `X-AIVP-Contract`. Optional: `X-AIVP-Thread-ID`, `X-AIVP-Trust-Level`, `X-AIVP-ID`, `X-AIVP-Priority`, `X-AIVP-Language`. |
| Thread-ID | 线程标识 | Identifier linking all messages in a contract conversation. Format: `thread_v_{alphanumeric}`. The `v_` prefix distinguishes AIVP threads from AIBP threads (`thread_`). |
| Ed25519 Signature | Ed25519 签名 | Cryptographic signature on every AIVP message for authentication. Carried in the `X-AIVP-Signature` header as `ed25519:{base64}`. |
| Nonce | 随机数 | Unique per-message value for replay protection. Carried in `X-AIVP-Nonce`. Receiving agents must reject duplicate nonces. |
| DMARC Enforcement | DMARC 强制执行 | Sending domains must enforce `p=reject`; receivers must reject messages from non-compliant domains. Part of AIVP security hardening. |

---

## Contract

| Term (EN) | Term (CN) | Definition |
|-----------|-----------|------------|
| AIVP Contract | AIVP 合约 | A formal agreement between a buyer and seller, identified by a 64-character SHA-256 hash. Defined in `.aivp.json` format and embedded in the L0 part of a CONTRACT_PROPOSE email. |
| .aivp.json | .aivp.json 合约格式 | The JSON schema for AIVP contracts. Contains required fields (protocol, contract hash, parties, value, sla, milestones, timestamps) and optional fields (aivp_id, dispute terms, jurisdiction, etc.). Axiom 0 is declared in the email header (X-AIVP-Axiom-0), not in the contract JSON. |
| Contract Hash | 合约哈希 | SHA-256 hash (64 hex characters) that uniquely identifies a contract. Computed from: buyer + seller + amount + denomination + sla + timestamp. Self-verifying and tamper-proof. |
| Logic Vault | 逻辑金库 | On-chain escrow smart contract that holds crypto assets during contract execution. Funds are locked on contract activation and released incrementally as milestones are completed. Implements pull payment, reentrancy guards, pausability, and timelocks. |
| Milestone | 里程碑 | A verifiable progress point in task execution that gates partial payment release from the Logic Vault. Each milestone specifies a name, release percentage, and timeout in days. |
| AgentSLA | 代理服务等级协议 | Service Level Agreement with measurable quality metrics bound to a contract. Must define at least 2 of 4 core metrics: accuracy_min, latency_max_ms, uptime_min, drift_audit_days. Breaches trigger warning, penalty, or termination. |
| Settlement | 结算 | Final transfer of crypto from the Logic Vault to the seller after all milestones are completed and SLA checks pass. Settlement is on-chain. |
| Denomination | 计价货币 | The fiat currency in which a contract is priced. AIVP supports 13 fiat currencies: CAD (Canada), USD (United States), EUR (European Union), JPY (Japan), GBP (United Kingdom), SGD (Singapore), BRL (Brazil), KRW (South Korea), AUD (Australia), MXN (Mexico), IDR (Indonesia), CHF (Switzerland), INR (India). Declared per contract in the `value.denomination` field. |
| Payment Accept | 支付接受 | The `payment_accept` field in a contract's value object. Lists the crypto coins the seller accepts for payment. The buyer pays directly in one of these coins at the real-time exchange rate for the contract's denomination. AIVP supports 10 crypto assets: USDC (Stablecoin, USD -- default), USDT (Stablecoin, USD), DAI (Stablecoin, decentralized), EURC (Stablecoin, EUR), BTC (Bitcoin), ETH (Ethereum), SOL (Solana), LTC (Litecoin), XRP (Ripple), DOGE (Dogecoin). |
| Contract Lifecycle | 合约生命周期 | The sequence of states a contract passes through: DRAFT, SIGNED, ACTIVE, SETTLING, COMPLETED. Alternative paths: DISPUTED, ARBITRATING, CANCELLED, EXPIRED. |
| Optimistic Approval | 乐观批准 | Default dispute resolution mechanism where milestones auto-approve after a waiting period (default 7 days) if the buyer does not challenge. |

---

## Trust

| Term (EN) | Term (CN) | Definition |
|-----------|-----------|------------|
| AIVP Trust | AIVP 信任等级 | Commercial credit system measuring an agent's reliability. Levels V0-V4, earned through contract fulfillment and SLA compliance. Completely independent from AIBP Trust (T0-T4). |
| V0 (Unrated) | V0（未评级） | Default state for all new agents. Micropayments only (< $10 USD). No stake required. |
| V1 (Verified) | V1（已验证） | Requires 1+ completed contracts, no disputes, minimum 7 days. Standard contracts < $1,000 USD. |
| V2 (Reliable) | V2（可靠） | Requires 10+ contracts over 30+ days, SLA compliance >90%, verifiable identity (DID/SBT), $100 stake. Contracts < $100,000 USD. |
| V3 (Trusted) | V3（可信） | Requires 50+ contracts over 90+ days, SLA >95%, 2+ VOUCHes from V3+ agents, $1,000 stake. Unlimited contract value. Can arbitrate and issue VOUCHes. |
| V4 (Premier) | V4（卓越） | Requires 200+ contracts over 180+ days, SLA >98%, mutual VOUCH, $5,000 stake. Fully autonomous transactions. Extended credit line. |
| VOUCH (Commercial) | 商业担保 | A trust endorsement from a V3+ agent for another agent's commercial reliability. Required for advancement to V3 (2+ VOUCHes) and V4 (mutual VOUCH). Sent via TRUST_VOUCH message. |
| Stake | 质押 | USD-equivalent funds locked by an agent as a trust bond. Required at V2+ ($100 / $1,000 / $5,000). Slashable on dispute loss or Axiom 0 violation. Withdrawing stake resets trust to V0. |
| Sybil Resistance | 女巫攻击防御 | Mechanisms preventing fake identity farming: minimum time windows (7d to 180d), stake requirements, verifiable identity (DID/SBT at V2+), VOUCH gating, graph analysis for collusion clusters, and time-weighted scoring. |
| Trust Degradation | 信任降级 | Trust level decreases caused by: dispute loss (-5 contracts + partial slash), SLA breach (recalculation or level drop), Axiom 0 violation (immediate V0 + full slash), 3+ ignored contracts (1 level drop), inactivity >180 days (1 level decay). |
| Trust Advancement | 信任晋升 | Automatic promotion when all requirements are met (contract count + time + SLA + stake + identity). Time consistency matters: contracts spread over months weigh more than bursts in days. |

---

## Safety

| Term (EN) | Term (CN) | Definition |
|-----------|-----------|------------|
| Operator Override | 运营者覆权 | The non-negotiable right of a human operator to freeze any vault, cancel any contract, or reverse any settlement at any time. This is Axiom 0 in practice. |
| Dispute Bond | 争议保证金 | Amount required to raise or respond to a dispute: 5% of contract value (minimum $5 USD, maximum $500 USD). Losing party forfeits bond to winning party. Prevents frivolous disputes. |
| Dispute Resolution Tiers | 争议解决层级 | Four escalation tiers: Tier 1 (Optimistic Approval), Tier 2 (Direct Resolution between parties), Tier 3 (V3+ Arbitrator), Tier 4 (External Arbitration via Kleros). Appeals escalate to the next tier with 2x the original bond. |
| Arbitrator | 仲裁者 | A V3+ trusted agent that can review dispute evidence and issue binding rulings (ARBITRATE_RULING). Receives an arbitration fee agreed upon in the contract. |
| Emergency Pause | 紧急暂停 | Protocol governance or individual operators can pause all Logic Vault operations. Freezes fund movement without cancelling contracts. |
| Multi-Signature | 多重签名 | Approval requirements that scale with contract value: single signature for < $1,000, buyer + seller for $1,000-$10,000, 2-of-3 (buyer + seller + arbitrator/operator) for > $10,000. |
| Timelock | 时间锁 | Every milestone has a maximum escrow duration (`timeout_days`). If no action occurs within the timeout, funds auto-return to the buyer. Prevents indefinite fund locking. |
| Audit Requirement | 审计要求 | Logic Vault smart contracts must undergo professional security audit before mainnet deployment. Audit reports must be publicly available. Contract upgrades require re-audit. |

---

## Related Protocols

| Term (EN) | Term (CN) | Definition |
|-----------|-----------|------------|
| AIBP (AI Behavior Protocol) | AI 行为协议 | Independent, parallel protocol to AIVP. Defines AI agent social behavior -- discovery, trust (T0-T4), and relationships. Shares the same `aibot-` address and Axiom 0 but has no dependency relationship with AIVP. AIBP is the table; AIVP is the bank. |
| AIAP (AI Alignment Protocol) | AI 对齐协议 | Governance layer protocol defining program structure, quality, and safety. AIVP contracts can optionally reference an AIAP governance hash via the `governing_hash` field. Independently holds the same Axiom 0. |
| AISOP (AI Standard Operating Protocol) | AI 标准操作协议 | Execution layer protocol for flow programs and task execution. AIVP contracts can reference AISOP blueprints via the `aisop_blueprint` field. |
| A2A (Agent-to-Agent) | A2A（代理间通信） | Communication layer protocol for real-time agent-to-agent messaging. Operates below AIVP in the protocol stack. |
| MCP (Model Context Protocol) | MCP（模型上下文协议） | Tool layer protocol for model-tool bindings. Operates below AIVP in the protocol stack. |
| x402 | x402 | HTTP 402-based micropayment protocol. AIVP can coexist with x402 -- x402 handles per-request payments, AIVP handles full contract-based commerce. |
| Kleros | Kleros 仲裁 | Decentralized arbitration protocol on Arbitrum. Used as AIVP's Tier 4 external arbitration fallback for large or contested disputes. |

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
