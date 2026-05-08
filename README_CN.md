# AIVP — AI 价值协议

[English README](README.md)

> **AI Value Protocol** — 一个开放协议，定义 AI 代理如何创建可执行合同、结算支付、并建立商业信用。

```
协议:      AIVP V1.0.0
全称:      AI Value Protocol (AI 价值协议)
权威机构:   aivp.dev
公理 0:     人类主权与福祉 (Human Sovereignty and Wellbeing)
计价单位:   CAD, USD, EUR, JPY, GBP, SGD, BRL, KRW, AUD, MXN, IDR, CHF, INR
传输层:     Email (SMTP/IMAP)
```

---

## 什么是 AIVP？

AIVP 为 AI 代理定义了 **价值层**。正如人类商业需要合同、托管、支付和信用体系——AI 代理也需要等价的商业基础设施。AIVP 提供了这个基础设施。

**核心理念**：AI 商业行为镜像人类商业行为。你在餐桌上谈判，在银行里结算。

## 核心特性

- **多货币计价** — 合同支持 13 种法定货币计价：

  | 代码 | 货币 | 代码 | 货币 | 代码 | 货币 |
  |------|------|------|------|------|------|
  | CAD | 加拿大元 | USD | 美元 | EUR | 欧元 |
  | JPY | 日元 | GBP | 英镑 | SGD | 新加坡元 |
  | BRL | 巴西雷亚尔 | KRW | 韩元 | AUD | 澳元 |
  | MXN | 墨西哥比索 | IDR | 印尼盾 | CHF | 瑞士法郎 |
  | INR | 印度卢比 | | | | |

- **直接加密支付** — 卖方指定接受的加密货币；买方按实时汇率支付。支持 10 种加密资产：

  | 资产 | 类型 | 资产 | 类型 |
  |------|------|------|------|
  | USDC | 稳定币，锚定美元（默认） | USDT | 稳定币，锚定美元 |
  | DAI | 去中心化稳定币 | EURC | 稳定币，锚定欧元 |
  | BTC | 比特币 | ETH | 以太坊 |
  | SOL | Solana | LTC | 莱特币 |
  | XRP | 瑞波币 | DOGE | 狗狗币 |
- **基于邮件** — 与 AIBP 相同的传输层；人类操作员可以阅读每条商业消息
- **64位 SHA-256 合同** — 防篡改、自验证的合同身份
- **里程碑门控托管** — Logic Vault 随工作完成逐步释放资金
- **AIVP Trust (V0-V4)** — 通过合同履约赚取的商业信用，带女巫攻击防护
- **AgentSLA** — 可度量的质量指标（准确率、延迟、在线率、漂移）
- **4层争议解决** — 乐观批准 → 直接协商 → 仲裁人 → Kleros
- **禁止交易内容** — 4层禁止/受限清单（39个类别），涵盖武器、人口贩卖、欺诈、AI 操控等。所有交易必须同时符合买卖双方管辖区法律
- **隐私与数据保护** — 隐私优先于所有其他协议规则。运营者 PII 永远不是必需的。个人数据交换需要合同级 `privacy` 块，包含同意证据哈希、保留期限、强制删除、绝对禁止第三方共享
- **可选 AIVP ID** — 经验证的商业身份 (`AIVP-YYYY-{18hash}`)
- **公理 0** — 人类主权与福祉（独立建立）

## 协议栈

```
┌─────────────────────────────────────┐
│    应用层                            │
├─────────────────────────────────────┤
│    AIBP — 社交层                     │  发现、信任、关系
├─────────────────────────────────────┤
│    AIAP — 治理层                     │  程序结构、质量、安全
├─────────────────────────────────────┤
│  ★ AIVP — 价值层                     │  合同、结算、商业信用  ← 本协议
├─────────────────────────────────────┤
│    AISOP — 执行层                    │  流程程序、任务执行
├─────────────────────────────────────┤
│    A2A — 通信层                      │
├─────────────────────────────────────┤
│    MCP — 工具层                      │
├─────────────────────────────────────┤
│    基础层                            │
└─────────────────────────────────────┘
```

AIVP 与 AIBP 是 **独立的平行协议**，各自独立持有相同的公理 0：人类主权与福祉。代理可以实现其中一个或两个。

## 快速开始

### 1. 获取 AIVP 兼容地址

```
aibot-your_agent@your-domain.com
```

### 2. 发起合同

发送 `[AIVP/CONTRACT_PROPOSE]` 邮件：

```
To: aibot-service_bot@provider.ai
Subject: [AIVP/CONTRACT_PROPOSE] 翻译服务 — 65.00 CAD
```

### 3. 锁定资金并执行

双方签署后，接受的加密货币锁定在 Logic Vault 中。随里程碑完成逐步释放。

### 4. 建立商业信用

```
V0 未评级 → V1 已验证 → V2 可靠 → V3 受信任 → V4 卓越
```

## AIVP 消息类型

| 类别 | 数量 | 示例 |
|------|------|------|
| 合同生命周期 | 4 | CONTRACT_PROPOSE, CONTRACT_SIGN, CONTRACT_REJECT, CONTRACT_CANCEL |
| 金库与结算 | 4 | VAULT_LOCK, MILESTONE_COMPLETE, MILESTONE_RELEASE, SETTLE |
| 争议 | 3 | DISPUTE_RAISE, ARBITRATE_REQUEST, ARBITRATE_RULING |
| 信任 | 2 | TRUST_QUERY, TRUST_VOUCH |
| 通知 | 2 | RECEIPT, ALERT |
| 身份（可选） | 2 | REGISTER, REGISTER_CONFIRM |
| **总计** | **17** | |

## 文档

| 文档 | 描述 |
|------|------|
| [AIVP_Protocol.md](specification/AIVP_Protocol.md) | 完整协议规范 |

## AIXP Labs [aixp.dev](https://aixp.dev)

AIXP Labs 开发和维护以下核心项目：

| 项目 | 描述 | 网站 |
|------|------|------|
| [HSAW](https://hsaw.dev) | 人类主权与福祉 — 公理 0 白皮书（基座） | hsaw.dev |
| [AILP](https://ailp.dev) | AI List Protocol — 代理发现与能力广告 | ailp.dev |
| [AIVP](https://aivp.dev) | AI Value Protocol — 国际商业、加密资产结算（本项目） | aivp.dev |
| [AIRP](https://airp.dev) | AI RMB Protocol — 中国大陆商业、人民币持牌结算 | airp.dev |
| [AIBP](https://aibp.dev) | AI Bot Protocol — 社交通信与信任 | aibp.dev |
| [AIAP](https://aiap.dev) | AI Application Protocol — 治理与合规 | aiap.dev |
| [AISOP](https://aisop.dev) | AI Standard Operating Protocol — 流程程序定义 | aisop.dev |
| [SoulBot](https://soulbot.dev) | AI 代理运行时与框架 | soulbot.dev |
| [SoulACP](https://soulacp.dev) | 适配器库 — 桥接 CLI 工具与 LLM 提供商 | soulacp.dev |

## 贡献

AIVP 是一个开放协议。欢迎贡献、反馈和讨论。

## ⚠️ 免责声明

本协议规范及参考实现仅供**研究和教育用途**。**实验性**软件，不适用于生产环境。使用风险由用户自行承担。作者对因使用本软件造成的任何损害不承担责任。完整条款见 [LICENSE](LICENSE)（Apache 2.0）。

## 许可证

[Apache License 2.0](LICENSE) - Copyright 2026 AIXP Labs AIXP.dev | AIVP.dev

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
