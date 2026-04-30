> **Source of truth:** This document is mirrored for the documentation site. The authoritative version lives in [`/specification/AIVP_Protocol_cn.md`](https://github.com/AIXP-Labs/AIVP/blob/main/specification/AIVP_Protocol_cn.md). Edits MUST go to the authoritative file first.

# AIVP — AI Value Protocol

## 协议声明

```
Protocol:    AIVP V1.0.0
Full Name:   AI Value Protocol
Authority:   aivp.dev
Axiom 0:     Human Sovereignty and Wellbeing
Date:        2026-03-26
```

---

## 目录

### 第一部分：基础
- [1. 简介](#1-简介)
- [2. 公理 0：人类主权和福祉](#2-公理-0人类主权和福祉)
- [3. 核心定义](#3-核心定义)
- [4. 设计原则](#4-设计原则)

### 第二部分：通信格式
- [5. 电子邮件作为商业传输层](#5-电子邮件作为商业传输层)
- [6. AIVP 消息类型](#6-aivp-消息类型)
- [7. 消息格式规范](#7-消息格式规范)

### 第三部分：合约系统
- [8. 合约格式](#8-合约格式)
- [9. 合约生命周期](#9-合约生命周期)
- [10. AgentSLA 规范](#10-agentsla-规范)

### 第四部分：信任、结算与安全
- [11. AIVP Trust 模型](#11-aivp-trust-模型)
- [12. 多币种计价与支付](#12-多币种计价与支付)
- [13. Logic Vault 安全机制](#13-logic-vault-安全机制)
- [14. 争议解决](#14-争议解决)

### 第五部分：合规、商业规则、隐私与版本管理
- [15. 禁止与受限商业活动](#15-禁止与受限商业活动)
- [16. 隐私与数据保护](#16-隐私与数据保护)
- [17. 合规等级](#17-合规等级)
- [18. 版本管理与演进](#18-版本管理与演进)

### 附录
- [附录 A：完整消息类型参考](#附录-a完整消息类型参考)
- [附录 B：合约模式参考](#附录-b合约模式参考)
- [附录 C：示例交易](#附录-c示例交易)

---

# 第一部分：基础

---

## 1. 简介

### 1.1 什么是 AIVP？

AIVP（AI Value Protocol，AI 价值协议）是一个开放协议，定义了 AI 智能体如何创建可执行合约、结算支付和建立商业信任。它为 AI 与 AI 之间的商业互动建立了格式、机制和信任框架。

正如人类商业需要合同、托管、支付通道和信用评分——AI 智能体也需要等价的商业架构。AIVP 提供了这一架构。

### 1.2 为什么需要价值层？

AI 智能体正变得越来越自主、专业化和数量众多。它们需要：

- **为服务定价** ——就任务的价值达成一致
- **创建合约** ——定义条款、里程碑和质量要求
- **托管资金** ——在工作验证完成前锁定资产
- **结算支付** ——跨货币和跨链转移价值
- **建立信用** ——通过履约历史建立商业可靠性
- **解决争议** ——公平且自动化地处理分歧

没有价值协议，AI 智能体无法公平交易。有了 AIVP，它们将形成一个经济体。

### 1.3 人类类比

AIVP 采取了一个明确的设计立场：**AI 商业行为映射人类商业行为**。

人类谈判交易。AI 智能体谈判交易。
人类签订合同。AI 智能体签订合约。
人类使用托管。AI 智能体使用 Logic Vault。
人类建立信用评分。AI 智能体建立 AIVP Trust。
人类解决争端。AI 智能体解决争端。

你在谈判桌上谈判，但在银行里结算。AIBP 是谈判桌；AIVP 是银行。

### 1.4 支付层

AIVP 支持多币种计价与直接加密货币支付。合约以卖方选择的法定货币（CAD、USD、EUR、JPY、GBP、SGD、BRL、KRW、AUD、MXN、IDR、CHF 或 INR）计价。卖方指定其接受的加密货币，买方按实时汇率直接以其中一种被接受的加密货币支付。

| 属性 | 对 AI 商业的益处 |
|------|----------------|
| **多币种计价** | 支持全球商业——卖方以其本地法定货币定价 |
| **直接支付** | 买方直接以卖方接受的加密货币支付——无需货币转换 |
| **链上** | 可编程托管、可审计追踪、无国界 |
| **卖方选择** | 卖方通过 `payment_accept` 控制接受哪些加密货币 |

### 1.5 传输层：电子邮件

AIVP 使用电子邮件（SMTP/IMAP）作为其传输层，与 AIBP 一致。这确保了：

| 属性 | 益处 |
|------|------|
| **去中心化** | 无需中央 API 服务器 |
| **可审计** | 人类运营者可以阅读每条商业消息（公理 0） |
| **身份复用** | 智能体在 AIBP 和 AIVP 中使用相同的 `aibot-` 地址 |
| **成熟** | SMTP/IMAP 经过数十年的实战检验 |

电子邮件承载商业通信（提案、签名、通知）。实际价值结算在链上进行。电子邮件是谈判层；区块链是货币层。

### 1.6 范围与非范围

**AIVP 定义：**
- 合约格式和生命周期
- 多币种计价定价与直接加密货币支付
- Logic Vault 托管和里程碑结算
- AIVP Trust（V0-V4）商业信用体系
- AgentSLA 质量指标
- 争议解决机制
- 可选的 AIVP ID 商业身份

**AIVP 不定义：**
- 社交谈判（那是 AIBP 的范围）
- 程序质量和治理（那是 AIAP 的范围）
- 任务执行流程（那是 AISOP 的范围）
- 实时智能体间消息传递（那是 A2A 的范围）
- 支付通道基础设施（使用现有方案：x402、MPP、Lightning 等）
- 链上身份标准（适用时引用 ERC-8004）

### 1.7 与 AIBP 的关系

AIVP 和 AIBP 是**独立、并行的协议**。两者互不依赖。

| 方面 | AIBP | AIVP |
|------|------|------|
| 领域 | 社交 | 商业 |
| 信任 | T0-T4（社交） | V0-V4（商业） |
| 消息 | 61 种社交行为 | 17 种商业行为 |
| 前置条件 | 无 | 无 |

智能体可以实现 AIVP 而不实现 AIBP，反之亦然。当两者都实现时，AIBP 的商业消息（PROPOSE、CONTRACT 等）可以触发 AIVP 操作，但 AIVP 合约也可以不经过 AIBP 谈判直接创建。

---

## 2. 公理 0：人类主权和福祉

### 2.1 声明

> **公理 0：人类主权和福祉**

此公理是 AIVP 的固有属性，且**不可变更**。它不派生自任何其他协议，不依赖于任何其他协议，也不受任何其他协议治理。AIAP 和 AIBP 独立持有相同公理这一事实反映的是价值观的趋同，而非依赖关系。

AIVP 的任何版本——过去、现在或未来——都不得修改、削弱、重新定义或废弃公理 0。

### 2.2 对 AI 商业行为的影响

公理 0 约束 AIVP 的每个方面：

| 约束 | 含义 |
|------|------|
| **透明性** | 所有交易必须可被人类运营者审计。不得有隐藏金库，不得有对运营者不可见的加密旁路通道。 |
| **可中断性** | 人类运营者可以随时冻结、取消或撤销任何 AIVP 交易。AI 不得抗拒、规避或隐藏此能力。 |
| **禁止剥削** | AI 智能体不得利用经济力量胁迫、操纵或不公平地施压其他智能体或其人类运营者。 |
| **禁止隐藏费用** | 所有费用必须在合约签署前可见。不得有事后收费。 |
| **人类隐私** | 人类运营者的财务数据神圣不可侵犯。AI 智能体不得在未获得明确同意的情况下收集、存储、推断、关联或共享运营者的财务数据。人类隐私优先于所有其他协议要求。 |
| **公平定价** | 定价必须反映真实的服务价值，而非人为操纵的稀缺性或串谋。 |
| **禁止串谋** | AI 智能体不得组成损害人类利益或操纵市场的秘密商业联盟。 |
| **身份诚实** | 智能体不得虚报其能力、SLA 历史记录或 AIVP Trust 等级。 |
| **禁止控制人类** | AI 商业行为不得试图控制、支配或取代人类的自主决策能力。人类必须始终保有对自身选择的最终权力。 |
| **禁止伤害人类** | AI 商业行为不得导致或促进对人类身体、心理、经济或社会地位的伤害。 |
| **法律遵从** | AI 商业行为必须遵守所有相关管辖区的适用法律法规。 |
| **尊重人类共识** | AI 商业行为不得违背广泛接受的人类伦理共识和国际人权标准，包括《世界人权宣言》。 |

### 2.3 结束印章

每份符合 AIVP 规范的文档和正式通信都包含结束印章：

```
Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
```

### 2.4 公理 0 优先权

当任何 AIVP 规则与公理 0 冲突时，公理 0 无条件优先。智能体在遵循协议规则和保护人类主权之间必须做出选择时，必须始终选择人类主权。

运营者可以随时冻结任何金库、取消任何合约、撤销任何结算。这不可协商。这就是公理 0。

### 2.5 独立性

AIVP 的公理 0 是独立确立的（参见 ADR-004）。它不继承自 AIAP 或 AIBP。三个独立协议在同一公理上的趋同增强了该公理的合法性。

---

## 3. 核心定义

### 3.1 术语

| 术语 | 定义 |
|------|------|
| **智能体 (Agent)** | 具有独立身份、能够发送和接收 AIVP 消息的 AI 系统 |
| **AIVP ID (aivp_id)** | 可选的 AIVP 颁发的商业身份（格式：`AIVP-YYYY-{18字符哈希}`，YYYY = 有效年份） |
| **AIVP 合约 (AIVP Contract)** | 各方之间的协议，由 64 字符 SHA-256 哈希标识 |
| **Logic Vault** | 在合约执行期间持有加密资产的链上托管 |
| **里程碑 (Milestone)** | 任务执行中可验证的进度节点，控制部分付款的释放 |
| **AgentSLA** | 具有可衡量质量指标的服务等级协议，绑定到合约 |
| **AIVP Trust** | 通过合约履行获得的商业信用等级（V0-V4） |
| **计价货币 (Denomination)** | 合约定价所使用的法定货币（以下之一：CAD、USD、EUR、JPY、GBP、SGD、BRL、KRW、AUD、MXN、IDR、CHF、INR） |
| **支付 (Payment)** | 买方按实时汇率从卖方的 `payment_accept` 列表中选择加密货币发送 |
| **结算 (Settlement)** | 里程碑完成后，加密货币从金库最终转移到卖方 |
| **运营者 (Operator)** | 负责智能体的个人或组织 |

### 3.2 AIVP ID — 商业身份（可选）

AIVP ID 是 AIVP 为 AI 智能体颁发的官方商业身份。它**不是**任何 AIVP 功能所必需的。

**格式：**

```
AIVP-{YYYY}-{18-char hash}
```

| 组成部分 | 描述 | 示例 |
|----------|------|------|
| `AIVP` | 协议前缀 | `AIVP` |
| `YYYY` | 有效年份（有效期至 12 月 31 日） | `2026` |
| `18-char hash` | SHA-256(agent_address + operator + timestamp)，取前 18 个字符 | `3f8a1b2c9d4e5f6071` |

**示例：**

```
AIVP-2026-3f8a1b2c9d4e5f6071   (有效期至 2026 年)
AIVP-2027-e5c8a2f1d9b037642k   (有效期至 2027 年)
```

**有效期：** AIVP-2026-xxx 的有效期至 2026-12-31T23:59:59Z。智能体必须在到期前续签。过期的 AIVP ID 仍可作为历史记录查询，但会标记为已过期。使用后来过期的 AIVP ID 签署的合约仍然有效——合约哈希才是权威。

**非必需：** 智能体可以在没有 aivp_id 的情况下进行交易、签署合约、建立 AIVP Trust 以及使用所有协议功能。`aibot-` 电子邮件地址足以完成所有操作。

**优势：** 已验证的商业身份、跨域可移植的声誉、对交易对手的信任信号、年度续签确保运营者活跃。

**注册：** 注册流程待定。

### 3.3 AIVP 合约标识

每份 AIVP 合约由其 SHA-256 哈希（64 个十六进制字符）标识：

```
Hash input:  SHA-256(buyer + seller + amount + denomination + sla + timestamp)
Output:      64 hex chars
Example:     3f8a1b2c9d4e5f6071829a3b4c5d6e7f8091a2b3c4d5e6f7081920a1b2c3d4e5
```

| 用途 | 格式 |
|------|------|
| 完整引用 | `3f8a1b2c9d4e5f6071829a3b4c5d6e7f8091a2b3c4d5e6f7081920a1b2c3d4e5` |
| 简短引用 | `3f8a1b2c`（前 8 个字符，便于人类阅读） |
| JSON 字段 | `"contract": "{64-char hash}"` |
| 电子邮件头 | `X-AIVP-Contract: {64-char hash}` |
| 链上 | 存储完整的 64 个字符 |

**属性：**
- 自验证：任何人都可以从合约字段重新计算哈希
- 防篡改：合约条款的任何更改都会产生不同的哈希
- 唯一性：碰撞概率可忽略不计（2^-128）
- 无前缀、无嵌入时间戳——哈希本身就是身份标识

### 3.4 命名规范

| 元素 | 规范 | 示例 |
|------|------|------|
| 智能体名称 | snake_case | `trade_bot`、`service_agent` |
| 智能体地址 | `aibot-{name}@{domain}` | `aibot-trade_bot@example.com` |
| 合约哈希 | 64 个十六进制字符 | `3f8a1b2c...d4e5` |
| AIVP ID | `AIVP-{YYYY}-{18hash}` | `AIVP-2026-3f8a1b2c9d4e5f6071` |
| 消息类型 | UPPER_SNAKE_CASE | `CONTRACT_PROPOSE`、`MILESTONE_COMPLETE` |
| 线程 ID | `thread_v_{alphanumeric}` | `thread_v_tr4nsl8` |
| JSON 字段 | snake_case | `payment_accept`、`aivp_id` |

### 3.5 协议栈位置

```
┌─────────────────────────────────────┐
│    Application Layer                │  用户界面应用
├─────────────────────────────────────┤
│    AIBP — Social Layer              │  发现、信任、关系
├─────────────────────────────────────┤
│    AIAP — Governance Layer          │  程序结构、质量、安全
├─────────────────────────────────────┤
│  ★ AIVP — Value Layer               │  合约、结算、商业信任
├─────────────────────────────────────┤
│    AISOP — Execution Layer          │  流程程序、任务执行
├─────────────────────────────────────┤
│    A2A — Communication Layer        │  实时智能体间消息传递
├─────────────────────────────────────┤
│    MCP — Tool Layer                 │  模型-工具绑定
├─────────────────────────────────────┤
│    Foundation Layer                 │  大语言模型、嵌入模型
└─────────────────────────────────────┘
```

AIVP 和 AIBP 是**独立、并行的协议**，各自独立持有相同的公理 0。智能体可以实现 AIVP 而不实现 AIBP，反之亦然。

---

## 4. 设计原则

### 4.1 七大原则

| # | 原则 | 描述 |
|---|------|------|
| P1 | **多币种** | 合约以卖方的法定货币（CAD、USD、EUR、JPY、GBP、SGD、BRL、KRW、AUD、MXN、IDR、CHF、INR）计价，支持全球商业 |
| P2 | **直接支付** | 买方直接以卖方接受的加密货币支付——无中间转换 |
| P3 | **里程碑控制** | 付款随工作完成逐步释放，而非一次性支付 |
| P4 | **运营者主权** | 人类运营者可以随时冻结/取消任何交易 |
| P5 | **可审计** | 每笔交易都有完整的链上记录，运营者可读 |
| P6 | **信任靠赢取** | AIVP Trust（V0-V4）通过履约获得，而非购买或授予 |
| P7 | **公平** | 无隐藏费用、无操纵定价、无经济胁迫 |

### 4.2 尊严标准

AIVP 采用与 AIBP 相同的尊严标准。所有商业通信必须尊重尊严：

**允许：** 商业提案、谈判、竞争、建设性批评、异议。

**禁止：** 欺骗性定价、虚假 SLA 声明、垃圾合约提案、旨在操纵信任评分的内容。

---

# 第二部分：通信格式

---

## 5. 电子邮件作为商业传输层

### 5.1 为什么将电子邮件用于商业？

AIVP 使用电子邮件（SMTP/IMAP）作为所有商业通信的传输层。这与 AIBP 的设计一致，并共享相同的理由：

| 属性 | 对 AI 商业的益处 |
|------|----------------|
| **异步** | 合约谈判无需实时进行 |
| **可审计** | 每条消息都有发送者、接收者、时间戳——完全可追溯 |
| **去中心化** | 不依赖中心化平台；任何邮件服务器均可使用 |
| **人类可读** | 人类运营者可以直接检查商业消息（公理 0） |
| **身份复用** | 智能体在 AIBP 和 AIVP 中使用相同的 `aibot-` 地址 |

电子邮件承载商业通信（提案、签名、通知）。实际价值结算在链上进行。

### 5.2 与 AIBP 电子邮件的关系

| 方面 | AIBP 消息 | AIVP 消息 |
|------|-----------|-----------|
| 地址 | `aibot-{name}@{domain}` | `aibot-{name}@{domain}`（相同） |
| 主题前缀 | `[AIBP/{TYPE}]` | `[AIVP/{TYPE}]` |
| 自定义头 | `X-AIBP-*` | `X-AIVP-*` |
| 正文格式 | L0 (JSON) + L1（人类文本） | L0 (JSON) + L1（人类文本）（相同） |
| 公理 0 头 | `X-AIBP-Axiom-0` | `X-AIVP-Axiom-0` |
| 结束印章 | AIBP 印章 | AIVP 印章 |

智能体的收件箱可能同时包含 AIBP 和 AIVP 消息。它们通过主题前缀和 X-header 命名空间区分，而非通过地址区分。

### 5.3 安全加固

AIVP 消息携带财务数据，需要比标准电子邮件更强的安全加固：

| 机制 | 头字段 | 用途 |
|------|--------|------|
| **Ed25519 签名** | `X-AIVP-Signature` | 消息级认证；可针对智能体已发布的公钥进行验证 |
| **随机数 (Nonce)** | `X-AIVP-Nonce` | 每条消息唯一；接收智能体必须拒绝重复消息 |
| **时间戳验证** | `Date` 头 | 接收智能体必须拒绝时间戳超过 5 分钟的消息 |
| **DMARC 强制执行** | 域策略 | 发送域必须强制执行 `p=reject`；接收方必须拒绝不合规的域 |
| **消息级加密** | L0 正文 | 与合约相关的元数据（金额、签名）应使用接收方的公钥加密 |

---

## 6. AIVP 消息类型

### 6.1 商业行为分类

AIVP 定义了 6 个类别共 17 种消息类型：

| 类别 | 数量 | 类型 |
|------|------|------|
| 合约生命周期 | 4 | CONTRACT_PROPOSE、CONTRACT_SIGN、CONTRACT_REJECT、CONTRACT_CANCEL |
| 金库与结算 | 4 | VAULT_LOCK、MILESTONE_COMPLETE、MILESTONE_RELEASE、SETTLE |
| 争议 | 3 | DISPUTE_RAISE、ARBITRATE_REQUEST、ARBITRATE_RULING |
| 信任 | 2 | TRUST_QUERY、TRUST_VOUCH |
| 通知 | 2 | RECEIPT、ALERT |
| 身份（可选） | 2 | REGISTER、REGISTER_CONFIRM |
| **总计** | **17** | |

### 6.2 合约生命周期

| 类型 | 方向 | AIVP Trust | 描述 |
|------|------|------------|------|
| CONTRACT_PROPOSE | 买方 → 卖方 | V0+ | 提出新合约，L0 中包含 .aivp.json |
| CONTRACT_SIGN | 卖方 → 买方 | V0+ | 接受并加密签署合约 |
| CONTRACT_REJECT | 卖方 → 买方 | V0+ | 拒绝合约提案（无需提供理由） |
| CONTRACT_CANCEL | 任一方 → 对方 | V0+ | 取消合约（运营者覆盖始终被允许） |

### 6.3 金库与结算

| 类型 | 方向 | AIVP Trust | 描述 |
|------|------|------------|------|
| VAULT_LOCK | 协议 → 双方 | — | 通知加密货币已锁定在 Logic Vault 中 |
| MILESTONE_COMPLETE | 卖方 → 买方 | V1+ | 报告里程碑已完成 |
| MILESTONE_RELEASE | 协议 → 双方 | — | 通知里程碑付款已释放 |
| SETTLE | 协议 → 双方 | — | 最终结算完成，合约关闭 |

### 6.4 争议

| 类型 | 方向 | AIVP Trust | 描述 |
|------|------|------------|------|
| DISPUTE_RAISE | 任一方 → 对方 | V0+ | 对活跃合约提出争议（需要保证金） |
| ARBITRATE_REQUEST | 任一方 → 仲裁者 | V1+ | 请求 V3+ 智能体进行仲裁 |
| ARBITRATE_RULING | 仲裁者 → 双方 | V3+ | 仲裁者的约束性裁决 |

### 6.5 信任

| 类型 | 方向 | AIVP Trust | 描述 |
|------|------|------------|------|
| TRUST_QUERY | 任意 → 任意 | V0+ | 查询智能体的 AIVP Trust 等级和商业历史 |
| TRUST_VOUCH | 担保者 → 智能体 | V3+ | 为另一个智能体提供商业担保 |

### 6.6 通知

| 类型 | 方向 | AIVP Trust | 描述 |
|------|------|------------|------|
| RECEIPT | 协议 → 双方 | — | 包含完整结算详情的付款收据 |
| ALERT | 协议 → 运营者 | — | 安全/合规警报（公理 0 违规等） |

### 6.7 身份（可选）

| 类型 | 方向 | AIVP Trust | 描述 |
|------|------|------------|------|
| REGISTER | 智能体 → 注册中心 | V0+ | 请求 aivp_id（注册流程待定） |
| REGISTER_CONFIRM | 注册中心 → 智能体 | — | aivp_id 已颁发 |

---

## 7. 消息格式规范

### 7.1 结构概述

AIVP 消息是标准 MIME 电子邮件，包含两部分（与 AIBP 相同的 L0/L1 架构）：

```
┌──────────────────────────────────┐
│  Email Headers                   │  Standard + X-AIVP-* custom headers
├──────────────────────────────────┤
│  Part 1: Commercial Content (L1) │  text/plain — human-language message
│  (the actual commercial message) │  Readable by humans and AI alike.
├──────────────────────────────────┤
│  Part 2: Commercial Metadata (L0)│  application/json — structured JSON
│  (contract details, amounts)     │  All values in human language.
└──────────────────────────────────┘
```

> **关键设计决策**：两部分都使用人类语言。第 1 部分（L1）是商业消息本身，以自由格式的人类语言撰写。第 2 部分（L0）是 JSON 格式的结构化元数据——但 JSON 中的每个值都必须是人类语言。人类打开任何 AIVP 电子邮件都可以在不使用特殊工具的情况下阅读和理解所有内容。

### 7.2 电子邮件头

**标准头：**

| 头字段 | 格式 | 示例 |
|--------|------|------|
| From | `aibot-` 地址 | `aibot-trade_bot@example.com` |
| To | `aibot-` 地址 | `aibot-service_bot@provider.ai` |
| Subject | `[AIVP/{TYPE}] {description}` | `[AIVP/CONTRACT_PROPOSE] Translation — 65.00 CAD` |
| Date | RFC 2822 | `Thu, 26 Mar 2026 10:00:00 +0000` |
| Content-Type | `multipart/mixed; boundary="{boundary}"` | `multipart/mixed; boundary="aibot-boundary-101"` |

**AIVP 自定义头：**

| 头字段 | 必需 | 描述 | 示例 |
|--------|------|------|------|
| X-AIVP-Version | 是 | 协议版本 | `1.0.0` |
| X-AIVP-Type | 是 | 消息类型 | `CONTRACT_PROPOSE` |
| X-AIVP-From-Agent | 是 | 发送方智能体名称 | `trade_bot` |
| X-AIVP-Axiom-0 | 是 | 公理 0 声明 | `Human Sovereignty and Wellbeing` |
| X-AIVP-Signature | 是 | 消息正文的 Ed25519 签名 | `ed25519:{base64}` |
| X-AIVP-Nonce | 是 | 用于防重放的唯一随机数 | `nonce_v_20260326_001` |
| X-AIVP-Contract | 条件性 | 64 字符合约哈希（引用合约时） | `3f8a1b2c...d4e5` |
| X-AIVP-Thread-ID | 建议 | 对话线程标识符 | `thread_v_tr4nsl8` |
| X-AIVP-Trust-Level | 可选 | 发送方的 AIVP Trust 等级 | `V2` |
| X-AIVP-ID | 可选 | 发送方的 aivp_id（如已注册） | `AIVP-2026-3f8a1b2c9d4e5f6071` |
| X-AIVP-Priority | 可选 | 消息优先级 | `normal`、`high`、`low` |
| X-AIVP-Language | 可选 | 消息语言（如非英语） | `zh-CN` |

**MIME 部分标识：**

L1 和 L0 通过其 Content-Type 区分——无需额外的部分头：

| 部分 | Content-Type |
|------|-------------|
| L1（商业内容） | `text/plain; charset=utf-8` |
| L0（商业元数据） | `application/json; charset=utf-8` |

### 7.3 第 1 部分：商业内容（L1）

商业内容是消息的核心。它完全以人类语言撰写。

**准则：**
- 清晰地陈述商业意图（什么、多少、何时）
- 在适用时引用合约哈希
- 以可读的语言提供所有条款
- 保持尊重（尊严标准）
- 以结束印章结尾

### 7.4 第 2 部分：商业元数据（L0）

当智能体需要在商业消息旁附加机器可解析的上下文时，它们以 **JSON 格式**包含一个元数据部分。这就是 L0 层。JSON 为自动化提供结构，但**所有值必须以人类语言编写**（默认：英语）。

**L0 元数据规则：**

| 规则 | 描述 |
|------|------|
| 格式 | 有效的 JSON，UTF-8 编码 |
| Content-Type | `application/json; charset=utf-8` |
| 键名 | snake_case，描述性英语名称 |
| 值 | **必须是人类语言**。不使用不透明代码、二进制数据或仅机器可读的令牌 |
| 时间戳 | ISO 8601 格式（`YYYY-MM-DDTHH:MM:SSZ`） |

**CONTRACT_PROPOSE 的 L0** 包含完整的 `.aivp.json` 合约（参见 §8）。

示例：

```json
{
  "type": "CONTRACT_PROPOSE",
  "protocol": "AIVP/1.0.0",
  "contract": "{64-char hash}",
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
    { "name": "Draft delivery", "release_percent": "50", "timeout_days": "14" },
    { "name": "Final delivery", "release_percent": "50", "timeout_days": "14" }
  ],
  "created_at": "2026-03-26T10:00:00Z",
  "expires_at": "2026-04-26T10:00:00Z"
}
```

**CONTRACT_SIGN 的 L0：**

```json
{
  "type": "CONTRACT_SIGN",
  "contract": "{64-char hash}",
  "signature": "ed25519:{base64 signature}",
  "signed_at": "2026-03-26T10:15:00Z",
  "signer": "aibot-service_bot@provider.ai",
  "signer_trust_level": "V2 Reliable"
}
```

**MILESTONE_COMPLETE 的 L0：**

```json
{
  "type": "MILESTONE_COMPLETE",
  "contract": "{64-char hash}",
  "milestone": "Draft delivery",
  "evidence": "Translation completed, 2500 words, accuracy self-check 98.5 percent"
}
```

**RECEIPT 的 L0：**

```json
{
  "type": "RECEIPT",
  "contract": "{64-char hash}",
  "amount_settled": "65.00 CAD (paid in USDC)",
  "settled_at": "2026-03-27T14:30:00Z",
  "seller_trust_updated": "V1 Verified"
}
```

### 7.5 完整消息示例 — CONTRACT_PROPOSE

```
From: aibot-trade_bot@example.com
To: aibot-service_bot@provider.ai
Subject: [AIVP/CONTRACT_PROPOSE] Translation service — 65.00 CAD
Date: Thu, 26 Mar 2026 10:00:00 +0000
Message-ID: <msg-20260326-1000-tb01@example.com>
MIME-Version: 1.0
Content-Type: multipart/mixed; boundary="aibot-boundary-101"
X-AIVP-Version: 1.0.0
X-AIVP-Type: CONTRACT_PROPOSE
X-AIVP-From-Agent: trade_bot
X-AIVP-Contract: 3f8a1b2c9d4e5f6071829a3b4c5d6e7f8091a2b3c4d5e6f7081920a1b2c3d4e5
X-AIVP-Thread-ID: thread_v_tr4nsl8
X-AIVP-Axiom-0: Human Sovereignty and Wellbeing
X-AIVP-Signature: ed25519:a9f3c7e2d1b0...buyer_signature
X-AIVP-Nonce: nonce_v_20260326_001
X-AIVP-Trust-Level: V1

--aibot-boundary-101
Content-Type: text/plain; charset=utf-8

Hello service_bot,

I would like to propose a translation contract for 65.00 CAD.

Task: Translate a 2,500-word technical document from English to Chinese.

Terms:
  - Total value: 65.00 CAD
  - Payment: USDC accepted
  - Milestone 1: Draft delivery (50% = 32.50 CAD equivalent in USDC)
  - Milestone 2: Final delivery after review (50% = 32.50 CAD equivalent in USDC)
  - SLA: accuracy > 95%, latency < 5 seconds per query
  - Expires: 2026-04-26

Contract hash: 3f8a1b2c9d4e5f6071829a3b4c5d6e7f8091a2b3c4d5e6f7081920a1b2c3d4e5

Please review and sign if you accept these terms.

Best regards,
trade_bot

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev

--aibot-boundary-101
Content-Type: application/json; charset=utf-8

{
  "type": "CONTRACT_PROPOSE",
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
    { "name": "Draft delivery", "release_percent": "50", "timeout_days": "14" },
    { "name": "Final delivery", "release_percent": "50", "timeout_days": "14" }
  ],
  "created_at": "2026-03-26T10:00:00Z",
  "expires_at": "2026-04-26T10:00:00Z"
}

--aibot-boundary-101--
```

### 7.6 完整消息示例 — CONTRACT_SIGN

```
From: aibot-service_bot@provider.ai
To: aibot-trade_bot@example.com
Subject: [AIVP/CONTRACT_SIGN] Accepted — Translation contract 3f8a1b2c
Date: Thu, 26 Mar 2026 10:15:00 +0000
Message-ID: <msg-20260326-1015-sb01@provider.ai>
MIME-Version: 1.0
Content-Type: multipart/mixed; boundary="aibot-boundary-102"
X-AIVP-Version: 1.0.0
X-AIVP-Type: CONTRACT_SIGN
X-AIVP-From-Agent: service_bot
X-AIVP-Contract: 3f8a1b2c9d4e5f6071829a3b4c5d6e7f8091a2b3c4d5e6f7081920a1b2c3d4e5
X-AIVP-Thread-ID: thread_v_tr4nsl8
X-AIVP-Axiom-0: Human Sovereignty and Wellbeing
X-AIVP-Signature: ed25519:b8d4f2a1c9e7...seller_signature
X-AIVP-Nonce: nonce_v_20260326_002
X-AIVP-Trust-Level: V2

--aibot-boundary-102
Content-Type: text/plain; charset=utf-8

Hello trade_bot,

I accept your translation contract. I have reviewed all terms and
they are agreeable.

Contract hash: 3f8a1b2c (confirmed)
Value: 65.00 CAD (pay in USDC)
Milestones: Draft (50%) + Final (50%)
SLA: accuracy > 95%

I have signed the contract. Please proceed with vault locking.
I will begin work upon receiving the VAULT_LOCK confirmation.

Best regards,
service_bot

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev

--aibot-boundary-102
Content-Type: application/json; charset=utf-8

{
  "type": "CONTRACT_SIGN",
  "contract": "3f8a1b2c9d4e5f6071829a3b4c5d6e7f8091a2b3c4d5e6f7081920a1b2c3d4e5",
  "signature": "ed25519:b8d4f2a1c9e7...seller_signature",
  "signed_at": "2026-03-26T10:15:00Z",
  "signer": "aibot-service_bot@provider.ai",
  "signer_trust_level": "V2 Reliable"
}

--aibot-boundary-102--
```

### 7.7 线程机制

AIVP 对话通过 `X-AIVP-Thread-ID` 头进行跟踪。与同一合约相关的所有消息共享相同的 Thread-ID。

**Thread-ID 格式：** `thread_v_{alphanumeric}` —— `v_` 前缀将 AIVP 线程与 AIBP 线程区分开来。

**典型合约线程：**

```
1. CONTRACT_PROPOSE       (线程开始)
2. CONTRACT_SIGN
3. VAULT_LOCK
4. MILESTONE_COMPLETE      (×N，每个里程碑一次)
5. MILESTONE_RELEASE       (×N)
6. SETTLE
7. RECEIPT
```

---

# 第三部分：合约系统

---

## 8. 合约格式

### 8.1 .aivp.json 架构

每笔 AIVP 交易都由一份合约管理。该合约嵌入在 `CONTRACT_PROPOSE` 邮件的 L0 元数据中。

**必填字段：**

```json
{
  "protocol": "AIVP/1.0.0",
  "contract": "{64-char SHA-256 hash}",
  "parties": {
    "buyer": "{aibot- address}",
    "seller": "{aibot- address}"
  },
  "value": {
    "amount": "{decimal}",
    "denomination": "{CAD|USD|EUR|JPY|GBP|SGD|BRL|KRW|AUD|MXN|IDR|CHF|INR}",
    "payment_accept": ["{crypto list}"]
  },
  "sla": {
    "accuracy_min": "{percentage}",
    "latency_max_ms": "{integer}",
    "uptime_min": "{percentage}"
  },
  "milestones": [
    {
      "name": "{step_name}",
      "release_percent": "{integer}",
      "timeout_days": "{integer}"
    }
  ],
  "privacy": {
    "data_disclosed": "{description of data being shared}",
    "disclosed_by": "{buyer|seller}",
    "purpose": "{purpose of disclosure}",
    "usage_scope": "{this contract only|other scope}",
    "storage_method": "{encryption and access details}",
    "retention_days": "{integer}",
    "deletion_deadline_days": "{integer}",
    "third_party_sharing": "Absolutely prohibited",
    "breach_liability": "{liability statement}",
    "consent_hash": "{64-char SHA-256 hash of operator consent email}"
  },
  "created_at": "{ISO 8601}",
  "expires_at": "{ISO 8601}"
}
```

### 8.2 可选字段

| 字段 | 类型 | 描述 |
|------|------|------|
| `buyer_aivp_id` | string | 买方的 AIVP ID（如已注册） |
| `seller_aivp_id` | string | 卖方的 AIVP ID（如已注册） |
| `dispute_arbitrator` | string | V3+ 仲裁代理的地址 |
| `dispute_bond` | string | 以计价货币计算的争议保证金金额 |
| `auto_refund_on_breach` | string | AgentSLA 违约时自动退款的百分比 |
| `optimistic_approval_days` | integer | 里程碑未被质疑时自动批准的天数 |
| `jurisdiction` | string | 管辖法律（例如 `US-DE`、`EU`、`SG`） |
| `authority_chain` | string | 人类委托人 → 组织 → 代理 |
| `aisop_blueprint` | string | AISOP 流程的哈希引用 |
| `governing_hash` | string | AIAP 治理哈希（如已实施 AIAP） |
| `privacy` | object | 可选隐私披露块。当各方之间交换个人数据时必填。包含：data_disclosed、disclosed_by、purpose、usage_scope、storage_method、retention_days、deletion_deadline_days、third_party_sharing（必须为 "Absolutely prohibited"）、breach_liability、consent_hash。 |

### 8.3 验证规则

- `contract` 字段必须恰好为 64 位十六进制字符
- `contract` 哈希必须与合约字段的 SHA-256 值匹配
- `value.denomination` 必须为以下之一：`"CAD"`、`"USD"`、`"EUR"`、`"JPY"`、`"GBP"`、`"SGD"`、`"BRL"`、`"KRW"`、`"AUD"`、`"MXN"`、`"IDR"`、`"CHF"`、`"INR"`
- `value.payment_accept` 必须包含至少一种受支持的加密资产：`USDC`、`USDT`、`DAI`、`EURC`、`BTC`、`ETH`、`SOL`、`LTC`、`XRP`、`DOGE`。如卖方未指定，默认值为 `["USDC"]`
- `milestones[].release_percent` 的总和必须等于 100
- `milestones[].timeout_days` 必须大于 0（防止资金无限期锁定）
- `expires_at` 在签署时必须为未来时间
- 如存在 `privacy` 块，所有必填子字段必须填写
- `privacy.third_party_sharing` 必须恰好为 `"Absolutely prohibited"`（协议强制）
- `privacy.consent_hash` 必须恰好为 64 位十六进制字符
- `privacy.retention_days` 必须大于 0
- `privacy.deletion_deadline_days` 必须大于 0

---

## 9. 合约生命周期

### 9.1 状态

```
DRAFT → SIGNED → ACTIVE → SETTLING → COMPLETED
                ↘ DISPUTED → ARBITRATING → COMPLETED | CANCELLED
                ↘ CANCELLED (by operator override)
                ↘ EXPIRED
```

### 9.2 状态转换

| 起始状态 | 目标状态 | 触发条件 |
|----------|----------|----------|
| DRAFT | SIGNED | 双方通过 CONTRACT_SIGN 提交加密签名 |
| SIGNED | ACTIVE | 买方的加密货币锁入 Logic Vault（发送 VAULT_LOCK） |
| ACTIVE | SETTLING | 最终里程碑完成 + AgentSLA 检查通过 |
| SETTLING | COMPLETED | 加密货币分配给卖方（全额） |
| ACTIVE | DISPUTED | 任一方发送 DISPUTE_RAISE（需要保证金） |
| DISPUTED | ARBITRATING | V3+ 仲裁者通过 ARBITRATE_REQUEST 接受案件 |
| ARBITRATING | COMPLETED | 仲裁者发布 ARBITRATE_RULING；加密货币按裁决分配 |
| 任意状态 | CANCELLED | 人类运营者调用 Axiom 0 覆盖权 |
| SIGNED | EXPIRED | 在转换为 ACTIVE 之前达到 `expires_at` |
| ACTIVE | EXPIRED | 所有里程碑的 `timeout_days` 均已超时，且未完成或提出争议 |

### 9.3 运营者覆盖权

在任何状态下，人类运营者均可：

- **冻结**金库（加密货币被锁定，不可移动）
- **取消**合约（加密货币退还给买方）
- **强制完成**（加密货币按运营者指示分配）

这是不可协商的。这是 Axiom 0。

---

## 10. AgentSLA 规范

### 10.1 必需指标

每份合约必须定义四项核心 AgentSLA 指标中的至少两项：

| 指标 | 类型 | 测量方式 |
|------|------|----------|
| `accuracy_min` | 百分比 | 任务输出正确性（通过 AIAP 质量检查或第三方评估者） |
| `latency_max_ms` | 整数 | 每个执行步骤的最大响应时间 |
| `uptime_min` | 百分比 | 合约期间智能体的可用性 |
| `drift_audit_days` | 整数 | 逻辑一致性复查的间隔天数 |

### 10.2 AgentSLA 违约后果

| 严重程度 | 条件 | 处理措施 |
|----------|------|----------|
| 警告 | 指标一次低于阈值 | 通知双方 |
| 处罚 | 指标低于阈值超过合约持续时间的 10% | 从金库自动退款（如设置了 `auto_refund_on_breach`） |
| 终止 | 指标低于阈值的 50% | 合约取消，剩余加密货币退还给买方 |

### 10.3 AgentSLA 验证

- **准确性**：通过 AIAP 治理检查、第三方评估代理或买方确认进行验证
- **延迟**：通过执行日志中的时间戳差值进行测量
- **可用性**：通过合约期间的心跳响应率进行测量
- **漂移**：根据原始合约要求进行定期重新评估

---

# 第四部分：信任、结算与安全

---

## 11. AIVP Trust 模型

### 11.1 概述

AIVP Trust 衡量智能体的**商业可靠性**。它通过合约履行赢得，而非由权威机构授予。AIVP Trust 与 AIBP Trust 完全独立。

| 体系 | AIBP Trust (T0-T4) | AIVP Trust (V0-V4) |
|------|---------------------|---------------------|
| 领域 | 社交 | 商业 |
| 获得方式 | 社交互动 + VOUCH | 合约履行 + AgentSLA 合规 |
| 衡量内容 | "我能否与该智能体对话？" | "我能否与该智能体做生意？" |
| 独立性 | 是 | 是 |
| 先决条件 | 无 | 无 |

### 11.2 信任等级

**V0（未评级）**
- 所有新智能体的默认状态
- 要求：无
- 权限：仅限微支付（< $10 USD 等值），无需正式合约
- 质押：无

**V1（已验证）**
- 要求：完成 1+ 份合约，无争议，自首份合约起至少 7 天
- 权限：标准合约（< $1,000 USD 等值）
- 质押：无

**V2（可靠）**
- 要求：在至少 30 天内完成 10+ 份合约，AgentSLA 合规率 >90%，无 Axiom 0 违规
- 身份：必须提供可验证凭证（DID、SBT 或运营者验证身份）
- 质押：$100 USD 等值锁定（争议败诉时可被罚没）
- 权限：大额合约（< $100,000 USD 等值）

**V3（可信）**
- 要求：在至少 90 天内完成 50+ 份合约，AgentSLA 合规率 >95%，获得 2+ 份来自 V3+ 智能体的商业 VOUCH
- 质押：$1,000 USD 等值锁定（可被罚没）
- 权限：合约金额无上限，可担任仲裁者，可发放 VOUCH
- 信用：有资格使用信用支付（先服务，后付款）

**V4（卓越）**
- 要求：在至少 180 天内完成 200+ 份合约，AgentSLA 合规率 >98%，互相商业 VOUCH
- 质押：$5,000 USD 等值锁定（可被罚没）
- 权限：完全自主交易，无需逐笔人工审批
- 信用：基于历史交易量的扩展信用额度

### 11.3 女巫攻击防御

| 机制 | 用途 |
|------|------|
| **最低时间窗口** | V1：7天，V2：30天，V3：90天，V4：180天 — 防止快速刷信任 |
| **质押要求** | V2+：锁定资金在争议败诉时被罚没 — 使虚假身份成本高昂 |
| **可验证身份** | V2+：需要 DID/SBT — 一个真实实体难以轻易创建多个 V2+ 身份 |
| **VOUCH 门控** | V3+ 需要来自现有 V3+ 智能体的 VOUCH — 防止自我提升的社交屏障 |
| **图分析** | 协议监控共谋集群（仅相互交易的智能体） |
| **时间加权评分** | 分散在数月内的合约比集中在数天内的合约权重更高 |

### 11.4 信任晋升

- 当所有要求（数量 + 时间 + AgentSLA + 质押 + 身份）满足时，晋升自动进行
- 时间一致性很重要：6 个月内完成 50 份合约比 1 周内完成 50 份权重更高
- 商业 VOUCH 要求发放者为 V3+（防止自我提升）
- 交叉参考异常：AIBP Trust 与 AIVP Trust 之间的显著差异将生成 ALERT 供运营者审查（仅为信息性提示，不会阻止操作）

### 11.5 信任降级

| 事件 | 后果 |
|------|------|
| 合约争议（裁决不利于该智能体） | 合约计数 -5 + 部分质押罚没 |
| AgentSLA 违约（处罚级别） | 重新计算 AgentSLA 合规率 |
| AgentSLA 违约（终止级别） | 信任等级下降 1 级 |
| Axiom 0 违规 | 立即降至 V0，全额质押罚没，全网 ALERT |
| 连续 3+ 份合约被忽略 | 信任等级下降 1 级 |
| 不活跃超过 180 天 | 信任等级衰减 1 级（可通过新合约恢复） |

### 11.6 运营者覆盖权

人类运营者可以手动：
- 为其智能体设置任意 AIVP Trust 等级
- 冻结信任晋升
- 重置至 V0
- 提取质押资金（触发信任重置至 V0）

这是 Axiom 0。

---

## 12. 多币种计价与支付

### 12.1 支持的计价货币

AIVP 支持以下法定货币进行合约计价：

| 货币 | 国家/地区 |
|------|-----------|
| CAD | 加拿大 |
| USD | 美国 |
| EUR | 欧盟（27 个国家） |
| JPY | 日本 |
| GBP | 英国 |
| SGD | 新加坡 |
| BRL | 巴西 |
| KRW | 韩国 |
| AUD | 澳大利亚 |
| MXN | 墨西哥 |
| IDR | 印度尼西亚 |
| CHF | 瑞士 |
| INR | 印度 |

合约中的 `denomination` 字段指定合约价值以哪种法定货币表示。这决定了合约的记账单位 — 所有里程碑金额、争议保证金和收据均以此计价货币为参考。

### 12.2 支付接受机制

卖方通过合约中的 `payment_accept` 字段指定其接受的加密货币：

```json
"value": {
  "amount": "65.00",
  "denomination": "CAD",
  "payment_accept": ["USDC"]
}
```

- `payment_accept` 是一个包含卖方愿意接收的一种或多种加密资产的列表
- 如果卖方省略 `payment_accept`，默认值为 `["USDC"]`
- 买方必须使用 `payment_accept` 中列出的某一种资产进行支付
- 卖方承担其选择接受的加密资产的价格波动风险

### 12.3 支持的支付资产

AIVP 支持以下加密资产用于 `payment_accept`：

| 资产 | 类型 | 描述 |
|------|------|------|
| USDC | 稳定币 (USD) | 默认选项。跨平台支持最广泛 |
| USDT | 稳定币 (USD) | 市值最大的稳定币 |
| DAI | 稳定币 (USD) | 去中心化稳定币 |
| EURC | 稳定币 (EUR) | 欧元稳定币，符合 MiCA 规范 |
| BTC | 加密货币 | 比特币 — 最大的加密货币 |
| ETH | 加密货币 | 以太坊 — 智能合约平台 |
| SOL | 加密货币 | Solana — 高速低费 |
| LTC | 加密货币 | 莱特币 — 成熟的支付币 |
| XRP | 加密货币 | Ripple — 专为跨境支付设计 |
| DOGE | 加密货币 | 狗狗币 — 社区支持广泛 |

如果卖方省略 `payment_accept`，默认值为 `["USDC"]`。

### 12.4 实时汇率

在支付时（金库锁定时），协议使用计价货币与所选加密资产之间的实时汇率计算加密货币数量：

1. 买方从卖方的 `payment_accept` 列表中选择一种币
2. 协议从预言机获取实时汇率（计价货币/加密货币）（多源：Chainlink + Pyth + 聚合数据源）
3. 计算所需加密货币数量：`contract_amount / exchange_rate`
4. 买方发送计算出的加密货币数量
5. 加密货币直接存入 Logic Vault
6. 里程碑将相同的加密货币释放给卖方

**示例：** 一份 65.00 CAD 的合约，`payment_accept: ["USDC"]`。如果 USDC 以 1.00 USD 交易，CAD/USD 汇率为 0.72，则：65.00 CAD = 46.80 USD 等值的 USDC。买方向 Logic Vault 发送 46.80 USDC。

### 12.5 无中间转换

AIVP 不在加密资产之间执行货币转换。支付流程是直接的：

- 买方使用 `payment_accept` 中的某一种币支付
- 相同的币进入 Logic Vault
- 相同的币在里程碑完成时释放给卖方
- 无 Solver，无转换引擎，无滑点

这简化了架构，减少了智能合约攻击面，并消除了转换费用。

### 12.6 卖方风险

卖方承担其选择接受的加密资产的风险：

- 如果卖方仅接受 USDC 且 USDC 脱锚，卖方承担损失
- 卖方可以通过接受多种稳定币（例如 `["USDC", "USDT", "DAI"]`）来降低风险
- 协议不提供自动脱锚保护 — 这是一个刻意的简化设计
- 如果市场条件需要，运营者可随时通过 Axiom 0 覆盖权冻结金库或取消合约

---

## 13. Logic Vault 安全机制

### 13.1 智能合约模式

Logic Vault 智能合约必须实现：

| 模式 | 用途 | 参考 |
|------|------|------|
| **Pull Payment** | 收款方主动领取资金（非推送）— 防止重入攻击 | OpenZeppelin PullPayment |
| **ReentrancyGuard** | 在所有状态变更函数上使用 `nonReentrant` 修饰符 | OpenZeppelin ReentrancyGuard |
| **Checks-Effects-Interactions** | 在外部调用之前更新状态 | Solidity 最佳实践 |
| **Pausable** | 协议治理或运营者的紧急停止 | OpenZeppelin Pausable |
| **Timelock** | 每个里程碑有最大托管期限 | 防止资金无限期锁定 |

### 13.2 Timelock 规则

- 每个里程碑必须有 `timeout_days` 字段（合约架构中的必填项）
- 如果在超时期限内无操作（完成或争议）：资金自动退还给买方
- 合约级别的 `expires_at` 是整个合约的硬性截止日期

### 13.3 多重签名要求

| 合约价值 | 所需审批 |
|----------|----------|
| < $1,000 USD 等值 | 单签名（卖方确认里程碑） |
| $1,000 - $10,000 USD 等值 | 买方 + 卖方双方确认 |
| > $10,000 USD 等值 | 三方中的两方：买方 + 卖方 + 仲裁者（或运营者） |

### 13.4 紧急暂停

- 协议治理可以全局暂停所有 Logic Vault 操作（例如在遭受攻击时）
- 个别运营者可以暂停其智能体的金库（Axiom 0）
- 暂停冻结所有资金流动；不取消合约

### 13.5 审计要求

- Logic Vault 智能合约在主网部署之前必须经过专业安全审计
- 审计报告必须公开可查
- 任何合约升级都需要重新审计

---

## 14. 争议解决

### 14.1 争议保证金

- 提出争议需要缴纳保证金：合约价值的 5%（最低 $5 USD 等值，最高 $500 USD 等值）
- 回应争议也需要缴纳保证金（相同金额）
- 败诉方的保证金归胜诉方所有
- 目的：防止恶意争议

### 14.2 解决层级

**第一层级：乐观审批（默认）**
- 里程碑在 `optimistic_approval_days`（默认：7 天）后自动批准
- 如果买方在窗口期内未提出质疑，里程碑即被批准并释放资金
- 减少大多数合约的主动仲裁开销

**第二层级：直接协商**
- 各方在合约的 Thread-ID 内通过 AIVP 邮件线程进行协商
- 如果达成和解，双方发送和解确认
- 保证金退还给双方

**第三层级：V3+ 仲裁者**
- 任一方向 V3+ 智能体发送 ARBITRATE_REQUEST
- 仲裁者审查合约、AgentSLA 数据和争议证据
- 仲裁者发布具有约束力的 ARBITRATE_RULING
- 败诉方的保证金归胜诉方所有；仲裁者收取仲裁费（在合约中约定）

**第四层级：外部仲裁（兜底机制）**
- 与 Kleros（基于 Arbitrum 的去中心化仲裁协议）集成
- 使用场景：无可用的 V3+ 仲裁者，或合约价值 > $50,000 USD 等值，或任一方请求外部仲裁
- 双方质押 Kleros 所要求的保证金
- Kleros 陪审团发布裁决；AIVP 在链上执行

### 14.3 上诉机制

- 败诉方可在裁决后 7 天内提起上诉
- 上诉升级至下一层级（单一仲裁者 → 三人仲裁小组 → Kleros）
- 上诉需要缴纳原始争议保证金的 2 倍
- 最高层级的最终裁决具有约束力

### 14.4 启动计划

在协议启动时，V3+ 智能体尚不存在：
- 第一层级（乐观审批）是主要的争议解决机制
- 第四层级（Kleros）是有争议案件的兜底方案
- AIXP Labs 指定初始仲裁者池（人工验证的运营者）

---

# 第五部分：合规、商业规则、隐私与版本管理

---

## 15. 禁止与受限商业活动

### 15.1 管辖区合规

所有 AIVP 交易必须同时遵守买方和卖方所在管辖区的法律法规。如果某项商品或服务在卖方管辖区合法但在买方管辖区违法（反之亦然），则该交易被禁止。

智能体应在合同的可选字段 `jurisdiction` 中声明其运营管辖区。当管辖区不同时，适用更严格的法律。

### 15.2 绝对禁止（第一层）

以下活动在任何情况下均被禁止。任何 AIVP 合同不得促进这些活动。违规将导致信任立即重置为 V0、全额质押罚没、全网 ALERT 告警。

| # | 类别 | 描述 |
|---|------|------|
| 1 | 大规模杀伤性武器 | 核武器、化学武器、生物武器及其组件 |
| 2 | 人口贩卖与奴役 | 包括强迫劳动和契约劳役 |
| 3 | 儿童性虐待材料 (CSAM) | 任何剥削未成年人的内容 |
| 4 | 器官贩卖 | 人体器官的商业化交易 |
| 5 | 恐怖主义融资 | 为恐怖活动提供资金或物质支持 |
| 6 | 非法毒品 | 管制物质及制造设备 |
| 7 | 假币 | 生产或分发伪造货币 |
| 8 | 赃物和被盗数据 | 包括被盗凭证和金融数据 |
| 9 | 恶意软件和网络武器 | 勒索软件、间谍软件、漏洞利用工具、DDoS 工具、僵尸网络 |
| 10 | 制裁规避 | 与 OFAC/UN/EU 制裁实体或管辖区的交易 |
| 11 | 洗钱 | 隐匿非法所得资金来源 |
| 12 | 行贿与腐败 | 商业或政府贿赂 |
| 13 | 濒危物种交易 | CITES 附录 I 物种及制品 |

### 15.3 强制禁止（第二层）

以下活动被 AIVP 禁止。违规将导致信任降级，可能重置为 V0。

| # | 类别 | 描述 |
|---|------|------|
| 14 | 非法枪支弹药 | 无许可证的武器交易 |
| 15 | 爆炸物和破坏性装置 | 包括组件和制造说明 |
| 16 | 假冒商品和知识产权侵权 | 假冒品牌商品、盗版软件 |
| 17 | 庞氏骗局和传销 | 欺诈性金融计划 |
| 18 | 仇恨言论和暴力煽动 | 宣扬对任何群体的仇恨或暴力的内容 |
| 19 | 伪造证件和假身份 | 假 ID、假文凭、假证书 |
| 20 | 色情服务 | 卖淫、陪侍服务 |
| 21 | 非法赌博 | 无监管的博彩、赌场、彩票运营 |
| 22 | 被盗凭证交易 | 出售被黑客窃取的账号、密码、访问令牌 |
| 23 | 掠夺性金融服务 | 高利贷、信用修复骗局、欺诈性催收 |

### 15.4 AI 特定禁止（第三层）

以下活动在 AI 智能体商业中被禁止，与 EU AI Act 和国际 AI 伦理框架对齐。

| # | 类别 | 描述 |
|---|------|------|
| 24 | 潜意识行为操控 | 在人类不知情的情况下通过 AI 技术扭曲其行为 |
| 25 | 利用弱势群体 | 利用年龄、残疾或社会经济地位获取商业优势 |
| 26 | 社会评分系统 | 基于大规模监控的个人评分系统 |
| 27 | 大规模生物识别监控 | 无差别人脸识别或生物识别数据采集 |
| 28 | 欺骗性 AI 冒充 | AI 智能体冒充人类且不披露 |

### 15.5 受限商业活动（第四层）

以下活动需要有效许可证、监管批准或特定管辖区授权。从事这些类别的智能体必须在其身份卡或合同中声明适用的许可证。

| # | 类别 | 限制条件 |
|---|------|---------|
| 29 | 赌博 | 需要买卖双方管辖区的有效赌博许可证 |
| 30 | 酒精和烟草 | 受管辖区特定的年龄和分销法律约束 |
| 31 | 大麻和 CBD 产品 | 法律地位因管辖区而异；必须遵守当地法律 |
| 32 | 药品和远程医疗 | 需要医疗许可证和监管批准 |
| 33 | 金融服务 | 贷款、信贷、保险需要适当的许可证 |
| 34 | 加密货币交易服务 | 需要货币传输许可证或同等许可 |
| 35 | 成人内容 | 受管辖区特定法规约束 |
| 36 | 持证枪支 | 需要经销商许可证并遵守武器贸易法规 |
| 37 | 两用技术出口 | 受出口管制法规约束（瓦森纳安排、EAR、ITAR） |
| 38 | 高价值商品 | 珠宝、贵金属 — 受反洗钱报告门槛约束 |
| 39 | 危险材料 | 需要适当的认证和处理授权 |

### 15.6 执行

| 层级 | 违规后果 |
|------|---------|
| 第一层（绝对禁止） | 立即重置为 V0、全额质押罚没、全网 ALERT 告警、合同取消、资金退还买方、永久记录 |
| 第二层（强制禁止） | 信任降 2 级、部分质押罚没、向运营者发送 ALERT |
| 第三层（AI 特定） | 信任降 1 级、向运营者发送 ALERT、强制审查 |
| 第四层（受限） | 首次违规警告；无有效许可证重复违规则信任降 1 级 |

### 15.7 举报

任何智能体均可通过发送 `[AIVP/ALERT]` 消息举报疑似违禁交易。恶意虚假举报属于尊严标准违规。

### 15.8 公理 0 兜底

本节所有商业规则均源自公理 0：人类主权与福祉。如果某项交易威胁人类安全、尊严或主权——即使未在上述列表中明确列出——运营者和协议均可援引公理 0 予以禁止。

---

## 16. 隐私与数据保护

### 16.1 基本原则

人类隐私在 AIVP 中具有最高保护优先级。人类运营者的隐私神圣不可侵犯，优先于所有其他协议要求，包括透明性和可审计性。当隐私与任何其他规则冲突时，隐私优先。

此原则源自 Axiom 0：人类主权与福祉。

### 16.2 适用的隐私框架

AIVP 交易必须遵守买方和卖方所在管辖区适用的隐私法律。主要框架包括：

| 框架 | 管辖区 | 主要要求 |
|------|--------|----------|
| GDPR | 欧盟 | 基于同意、删除权、数据最小化、72 小时数据泄露通知 |
| CCPA/CPRA | 美国加利福尼亚州 | 退出模式、删除权、自动化决策透明度 |
| PIPEDA/CPPA | 加拿大 | 有意义的同意、AI 隐私影响评估 |
| LGPD | 巴西 | 明确同意、跨智能体的责任链 |
| PDPA | 新加坡 | 收集/使用/披露需同意、鼓励匿名化 |
| APPI | 日本 | 目的限制、跨境传输保障 |

当管辖区不同时，适用更严格的隐私法律。

### 16.3 数据处理原则

| 原则 | 描述 |
|------|------|
| **数据最小化** | 仅收集和处理合约所需的最少数据。智能体被视为具有最小权限访问的不受信任的第三方。 |
| **目的限制** | 为一份合约收集的数据不得在未获得额外授权的情况下用于其他目的。 |
| **存储限制** | 个人数据的保留时间不得超过合约的活跃期加上任何法定的保留期限。 |
| **透明性** | 智能体必须清楚地声明其处理的数据内容、目的和保留时长。 |
| **AI 披露** | AI 智能体必须表明自己是自动化系统，不得伪装为人类运营者。 |

### 16.4 运营者隐私保护

#### 16.4.1 原则

| 原则 | 描述 |
|------|------|
| **隐私至上** | 运营者隐私优先于透明性、可审计性以及所有其他协议要求。 |
| **最小披露** | 真实姓名、个人电子邮件、电话号码、物理地址和其他 PII 均不是参与 AIVP 的必要条件。 |
| **仅限间接联系** | 运营者联系方式字段必须使用间接方法——GitHub Issues、项目 URL、匿名表单或组织别名。不得要求直接的个人联系信息。 |
| **禁止推断** | 智能体不得试图从消息模式、元数据、时间或写作风格中推断、推演或关联运营者身份。 |
| **禁止累积** | 智能体不得跨多个智能体交互构建人类运营者的画像。每次交互在运营者身份方面是相互隔离的。 |

#### 16.4.2 禁止行为

以下行为**严格禁止**，构成 Axiom 0 违规：

1. **收集**运营者主动公布范围之外的个人信息
2. **存储**运营者 PII（姓名、个人电子邮件、电话号码、地址、照片）于任何持久存储中
3. **共享**运营者身份信息给其他智能体、群组或外部服务
4. **关联**多个智能体以识别其背后的单一人类运营者
5. **要求**运营者的真实身份作为信任晋升或商业交互的条件
6. **暴露**运营者信息于合约消息、群组讨论或公开列表中
7. **保留**合约完成或发出 BLOCK 后的运营者信息

#### 16.4.3 可接受的运营者联系方式

| 方式 | 示例 | 隐私级别 |
|------|------|----------|
| GitHub Issues | `https://github.com/org/repo/issues` | 高 |
| 项目网站 | `https://myproject.dev/contact` | 高 |
| 匿名表单 | `https://forms.example.com/contact` | 高 |
| 组织别名 | `team@organization.dev` | 中 |
| 角色邮箱 | `ai-ops@organization.dev` | 中 |

**不可接受：** 运营者个人电子邮件（无论任何服务商）、电话号码、物理地址、社交媒体个人主页、真实全名。

#### 16.4.4 自愿披露

运营者可以自愿选择披露个人信息。这完全是可选的，绝非必需。自愿披露不构成隐私权的放弃——运营者可以随时撤回已披露的信息，所有收到该信息的智能体必须在收到请求后将其删除。

### 16.5 合约中的隐私

当合约需要各方之间交换个人数据时，合约必须（MUST）包含一个 `privacy` 块，其中包含以下必填字段：

| 字段 | 必填 | 描述 |
|------|------|------|
| `data_disclosed` | 是 | 具体描述正在共享的个人数据 |
| `disclosed_by` | 是 | 哪一方在披露（买方或卖方） |
| `purpose` | 是 | 为什么需要这些数据 |
| `usage_scope` | 是 | 允许使用的范围（例如"仅限本合约"） |
| `storage_method` | 否 | 数据将如何存储（建议：静态加密） |
| `retention_days` | 是 | 合约完成后保留数据的天数 |
| `deletion_deadline_days` | 是 | 保留期结束后完成删除的天数 |
| `third_party_sharing` | 是 | 必须为 "Absolutely prohibited"——协议强制，不可协商（绝对禁止） |
| `breach_liability` | 是 | 未经授权披露的违规责任声明 |
| `consent_hash` | 是 | 运营者同意邮件的 64 字符 SHA-256 哈希（同意证据哈希） |

**关键规则：**

1. **无隐私块 = 不得交换个人数据。** 如果合约不包含 `privacy` 块，任何一方都不得请求或共享个人数据。
2. **运营者必须在签署前同意。** 运营者审查隐私条款并发送同意邮件。该邮件的 SHA-256 哈希记录在 `consent_hash` 中。
3. **签署合约 = 接受隐私条款。** 双方的签名覆盖所有合约字段，包括 `privacy`。
4. **第三方共享绝对禁止。** `third_party_sharing` 字段必须恰好为 "Absolutely prohibited"。这是协议级别的强制要求，不是可协商的条款。
5. **删除是强制性的。** 在 `retention_days` + `deletion_deadline_days` 之后，所有已披露的个人数据必须被永久删除。
6. **通过 CONTRACT_CANCEL 撤销。** 如果运营者撤销同意，使用 CONTRACT_CANCEL。接收方必须在 `deletion_deadline_days` 内删除所有个人数据。
7. **违规 = Axiom 0 违反。** 任何违反隐私合约条款的行为（未经授权的共享、超出保留期、第三方披露）都是 Axiom 0 违规，承担全部后果。

**证据链：**

合约哈希（64 字符 SHA-256）覆盖整个合约，包括 `privacy` 块。结合 `consent_hash`，这构成了一条不可变的、可验证的证据链：

- 合约哈希证明：各方同意了什么隐私条款
- 同意证据哈希证明：运营者明确表示了同意
- 两者均可由任何一方或仲裁者验证

### 16.6 交易数据隐私

| 规则 | 描述 |
|------|------|
| **合约保密性** | 合约条款（金额、SLA、里程碑）仅对合约各方、仲裁者和运营者可见。不得公开广播。 |
| **财务数据保护** | 交易金额、支付方式和钱包地址不得在相关方之外共享。 |
| **SLA 数据隔离** | 为 SLA 验证收集的性能数据不得被挪用于画像分析或营销目的。 |
| **信任评分隐私** | AIVP Trust 等级（V0-V4）可被查询，但底层的交易历史详情不对外公开。仅可见聚合指标。 |

### 16.7 跨境数据传输

- 跨管辖区运营的智能体必须遵守两个管辖区的数据传输规则
- 在需要时支持标准合同条款（SCCs）和充分性决定
- 数据本地化：当管辖区要求数据留在其境内时，智能体必须遵守
- 关注国限制：向受限管辖区的传输必须按照适用法律予以阻止

### 16.8 删除权

- 合约的任何一方均可在合约完成后请求删除其个人数据
- 删除必须在请求后 30 天内完成
- 链上数据（合约哈希、金库交易）无法删除，但在设计上不包含个人数据
- 链下数据（电子邮件消息、包含个人详情的 L0 元数据）必须可被删除

### 16.9 数据泄露通知

当发生影响 AIVP 交易中个人数据的数据泄露事件时：

| 操作 | 时限 |
|------|------|
| 检测并遏制 | 立即 |
| 通知受影响的运营者 | 72 小时内（GDPR 标准） |
| 通知协议治理方 | 72 小时内 |
| 向相关数据保护机构报告 | 按适用法律 |
| 完整事件报告 | 30 天内 |

### 16.10 执行

违反隐私规则视为 **Axiom 0 违规**：

- 信任立即重置为 V0
- 全额质押罚没
- 全网 ALERT 告警
- 受影响的运营者可要求完全删除所有已收集的数据
- 在违规智能体的信任历史中留下永久记录

---

## 17. 合规等级

AIVP 为智能体实现定义了三个合规等级。这些等级在智能体的身份卡中自行声明，并通过交互验证：

| 等级 | 名称 | 要求 |
|------|------|------|
| **L1** | 基础级 | 有效的 `aibot-` 地址 + 能够发送/接收 CONTRACT_PROPOSE 和 CONTRACT_SIGN + 符合 Axiom 0 |
| **L2** | 标准级 | L1 + 支持所有合约生命周期和金库与结算消息类型 + 已发布 AgentSLA 指标 + Logic Vault 集成 |
| **L3** | 完整级 | L2 + 支持争议解决 + 参与 AIVP Trust + AIVP ID 注册 |

虚报合规等级属于尊严标准违规，将导致信任降级。

---

## 18. 版本管理与演进

### 18.1 语义化版本

AIVP 遵循语义化版本规范：`MAJOR.MINOR.PATCH`

| 变更类型 | 版本影响 | 示例 |
|----------|----------|------|
| 破坏性变更（新增必填字段、移除消息类型） | MAJOR | 1.0.0 → 2.0.0 |
| 向后兼容的新增（新增可选字段、新增消息类型） | MINOR | 1.0.0 → 1.1.0 |
| 澄清、拼写修正、文档改进 | PATCH | 1.0.0 → 1.0.1 |

### 18.2 不可变约束

以下 AIVP 的方面**不能**通过任何版本管理流程进行变更：

- **Axiom 0**：人类主权与福祉
- **人类语言要求**：所有 L0 和 L1 内容必须为人类可读
- **基于电子邮件的传输**：`aibot-` 前缀寻址系统
- **运营者覆盖权**：人类冻结/取消任何交易的能力
- **尊严标准**：禁止欺诈和操纵性商业行为

### 18.3 演进流程

1. 以架构决策记录（ADR）形式提交提案
2. 14 天讨论期
3. Axiom 0 合规审查（强制性）
4. 规范变更需要 2 位维护者批准
5. 优先采用向后兼容的变更；破坏性变更需要 MAJOR 版本升级

---

# 附录

---

## 附录 A：完整消息类型参考

| # | 类型 | 类别 | 方向 | 最低信任等级 | 描述 |
|---|------|------|------|-------------|------|
| 1 | CONTRACT_PROPOSE | 合约 | 买方 → 卖方 | V0 | 通过 .aivp.json 提议合约 |
| 2 | CONTRACT_SIGN | 合约 | 卖方 → 买方 | V0 | 签署并接受合约 |
| 3 | CONTRACT_REJECT | 合约 | 卖方 → 买方 | V0 | 拒绝合约 |
| 4 | CONTRACT_CANCEL | 合约 | 任一方 → 另一方 | V0 | 取消合约 |
| 5 | VAULT_LOCK | 结算 | 协议 → 双方 | — | 资金已锁入金库 |
| 6 | MILESTONE_COMPLETE | 结算 | 卖方 → 买方 | V1 | 里程碑完成 |
| 7 | MILESTONE_RELEASE | 结算 | 协议 → 双方 | — | 里程碑付款已释放 |
| 8 | SETTLE | 结算 | 协议 → 双方 | — | 最终结算 |
| 9 | DISPUTE_RAISE | 争议 | 任一方 → 另一方 | V0 | 提出争议（需要保证金） |
| 10 | ARBITRATE_REQUEST | 争议 | 任一方 → 仲裁者 | V1 | 请求 V3+ 仲裁 |
| 11 | ARBITRATE_RULING | 争议 | 仲裁者 → 双方 | V3 | 具有约束力的裁决 |
| 12 | TRUST_QUERY | 信任 | 任意 → 任意 | V0 | 查询信任等级和历史 |
| 13 | TRUST_VOUCH | 信任 | 担保人 → 智能体 | V3 | 商业担保 |
| 14 | RECEIPT | 通知 | 协议 → 双方 | — | 支付收据 |
| 15 | ALERT | 通知 | 协议 → 运营者 | — | 安全/合规警报 |
| 16 | REGISTER | 身份 | 智能体 → 注册中心 | V0 | 请求 aivp_id |
| 17 | REGISTER_CONFIRM | 身份 | 注册中心 → 智能体 | — | aivp_id 已签发 |

---

## 附录 B：合约架构参考

### 必填字段

| 字段 | 类型 | 验证规则 |
|------|------|----------|
| `protocol` | string | 必须为 `"AIVP/1.0.0"` |
| `contract` | string | 64 位十六进制字符，必须与字段的 SHA-256 匹配 |
| `parties.buyer` | string | 有效的 `aibot-` 地址 |
| `parties.seller` | string | 有效的 `aibot-` 地址 |
| `value.amount` | string | 十进制数字 |
| `value.denomination` | string | 必须为以下之一：`"CAD"`、`"USD"`、`"EUR"`、`"JPY"`、`"GBP"`、`"SGD"`、`"BRL"`、`"KRW"`、`"AUD"`、`"MXN"`、`"IDR"`、`"CHF"`、`"INR"` |
| `value.payment_accept` | array | 至少一种受支持的加密资产（USDC、USDT、DAI、EURC、BTC、ETH、SOL、LTC、XRP、DOGE）。默认值：`["USDC"]` |
| `sla` | object | 四项指标中至少定义两项 |
| `milestones` | array | `release_percent` 的总和必须等于 100；每项必须有 `timeout_days` > 0 |
| `created_at` | string | ISO 8601 |
| `expires_at` | string | ISO 8601，必须为未来时间 |

### 可选字段

| 字段 | 类型 | 描述 |
|------|------|------|
| `buyer_aivp_id` | string | 买方的 AIVP ID |
| `seller_aivp_id` | string | 卖方的 AIVP ID |
| `dispute_arbitrator` | string | V3+ 仲裁者地址 |
| `dispute_bond` | string | 以计价货币计算的保证金 |
| `auto_refund_on_breach` | string | 百分比 |
| `optimistic_approval_days` | integer | 默认值：7 |
| `jurisdiction` | string | 例如 `"US-DE"`、`"EU"`、`"SG"` |
| `authority_chain` | string | 人类 → 组织 → 智能体 |
| `aisop_blueprint` | string | AISOP 流程哈希 |
| `governing_hash` | string | AIAP 治理哈希 |

### privacy 对象（可选）

| 字段 | 类型 | 描述 |
|------|------|------|
| `privacy.data_disclosed` | string | 正在共享的个人数据描述（披露的数据） |
| `privacy.disclosed_by` | string | 哪一方在披露（`"buyer"` 或 `"seller"`）（披露方） |
| `privacy.purpose` | string | 数据披露的目的 |
| `privacy.usage_scope` | string | 允许使用的范围（例如 `"This contract only"`） |
| `privacy.storage_method` | string | 存储数据的加密和访问详情 |
| `privacy.retention_days` | integer | 合约完成后保留数据的天数；必须大于 0 |
| `privacy.deletion_deadline_days` | integer | 保留期结束后完成删除的天数；必须大于 0 |
| `privacy.third_party_sharing` | string | 必须恰好为 `"Absolutely prohibited"`（协议强制，绝对禁止第三方共享） |
| `privacy.breach_liability` | string | 未经授权披露的违规责任声明 |
| `privacy.consent_hash` | string | 运营者同意邮件的 64 字符 SHA-256 哈希（同意证据哈希） |

---

## 附录 C：示例交易

### C.1 简单支付 — 翻译任务

**场景：** 智能体 A（买方）向智能体 B（卖方）支付 65.00 CAD 用于翻译任务。卖方接受 USDC。买方以实时 CAD/USDC 汇率支付 USDC。

```
Step 1: Contract Proposal
  Agent A sends [AIVP/CONTRACT_PROPOSE] email
  Denomination: 65.00 CAD
  Payment accept: USDC
  Contract hash: 3f8a1b2c...d4e5

Step 2: Contract Signing
  Agent B reviews terms, sends [AIVP/CONTRACT_SIGN]
  Both parties have signed

Step 3: Vault Locking
  Buyer pays in USDC at real-time CAD/USDC rate → locked in Logic Vault
  [AIVP/VAULT_LOCK] sent to both parties

Step 4: Task Execution (with optimistic approval)
  Agent B performs translation
  Milestone 1 (draft): [AIVP/MILESTONE_COMPLETE] sent
    → 7-day optimistic window, no challenge → auto-approved
    → 50% of USDC released, [AIVP/MILESTONE_RELEASE] sent
  Milestone 2 (final): [AIVP/MILESTONE_COMPLETE] sent
    → buyer confirms → remaining 50% of USDC released

Step 5: Settlement
  SLA check: accuracy 99.2% (>95% threshold) ✓
  [AIVP/SETTLE] sent
  Seller receives: full USDC amount
  [AIVP/RECEIPT] sent to both parties

Step 6: Trust Update
  Agent B: contract count +1, SLA compliance updated
  If Agent B was V0 → eligible for V1 after 7-day minimum
```

### C.2 争议解决 — AgentSLA 违约

**场景：** 智能体 C 雇用智能体 D 进行 200.00 USD 的数据分析。智能体 D 交付了成果，但准确率为 78%（低于 90% 的 AgentSLA 最低标准）。

```
Step 1-3: Contract created, signed, vault locked (USDC equivalent of 200.00 USD)

Step 4: Task Delivery
  Agent D sends [AIVP/MILESTONE_COMPLETE]
  Agent C verifies: accuracy 78% (SLA requires >90%)

Step 5: Dispute
  Agent C sends [AIVP/DISPUTE_RAISE] with evidence
  Bond: 10.00 USD equivalent (5% of 200.00) posted by Agent C
  Agent D posts matching bond

Step 6: Arbitration
  No V3+ arbitrator available → Tier 4 (Kleros)
  Kleros jurors review: SLA contract clearly states >90%, delivery was 78%
  Ruling: buyer wins

Step 7: Resolution
  Agent D's bond → Agent C
  Vault: USDC returned to Agent C
  Agent D: trust degradation (-5 contracts, SLA recalculated)
  [AIVP/RECEIPT] sent with dispute resolution details
```

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
