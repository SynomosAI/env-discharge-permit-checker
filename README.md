# env-discharge-permit-checker

[![License: MIT](https://img.shields.io/badge/Code%20License-MIT-yellow.svg)](LICENSE)
[![Content: ARR](https://img.shields.io/badge/Content%20License-ARR%20%7C%20citable-blue.svg)](LICENSE-CONTENT)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED%20%E2%80%94%20open%20for%20claim-orange.svg)](#认领方式)
[![Domain: env](https://img.shields.io/badge/domain-env-2ea44f.svg)](domain.manifest.json)
[![MCP Checker](https://img.shields.io/badge/checker-MCP%20tool-8a2be2.svg)](checker.io.json)

> **状态：RESERVED（占位 · 开放认领）** — 本仓已按平台协议规范建好行业接入四件套骨架，
> 等待具备本域资质的运营方认领并填充真实规则。

把「这个项目能不能排、排多少、要办哪些手续」拆成可核验的属性，让 AI 只做要件提示，不做投产与排放结论。

**English:** This repository is the SynomosAI *domain profile* for pollution discharge permits and environmental impact assessment (EIA) obligations. It is a declarative, machine-readable skeleton (manifest / gate-map / privacy / checker contract). The embedded checker MCP tool only **shows and verifies provenance**; it never outputs "may discharge / may go into production" clearances — the production and discharge power belongs to the ecology & environment authority.

## 这个域管什么

排污许可与环境影响评价义务核验

环评、排污许可、危废管理分属不同制度且相互衔接，未批先建、无证排污、篡改监测数据是典型违法情形。AI 能做的是把许可状态与监测义务摆清楚，投产与排放权在生态环境主管部门。

## 域标识

| 项 | 值 |
|---|---|
| 域 ID | `env`（全局唯一，一经分配不复用） |
| 域名称 | 环保 · 排污许可与环评义务 |
| Profile 版本 | `domain/1.0` |
| 当前状态 | `RESERVED` |
| 占位时间 | 2026-09-12 |

## 属性清单

| 属性键 | 类型 | 说明 |
|---|---|---|
| `env.eia_status` | enum | 环境影响评价文件审批或登记备案状态 · 取值 approved/filing/pending/expired/not_required |
| `env.permit_status` | enum | 排污许可证有效性，以生态环境部门公示为准 · 取值 valid/expired/none/not_required |
| `env.emission_compliance` | enum | 是否达标排放，含许可浓度与总量双控 · 取值 compliant/exceedance/unknown |
| `env.monitoring_status` | enum | 自行监测方案落实与数据公开情况 · 取值 automatic/manual/none/not_required |
| `env.hazardous_waste` | enum | 危险废物产生、贮存、转移联单管理情况 · 取值 managed/deviation/none/not_applicable |

## 本域红线（不可逾越，机器可读）

1. 不得输出「可排放」「可投产」放行结论，以生态环境主管部门许可为唯一依据
2. 超标排放、无证排污或监测数据异常时不得给出可继续生产的表述
3. 不得建议稀释达标、私设旁路、篡改监测数据或补办手续追溯合法化

> 红线在 `gate-map.json` 中均有对应阻断规则。平台校验器会检查「每条红线都有规则覆盖」，
> 缺失即校验失败——**制度与系统不允许不同步**。

## 行业接入四件套

| 文件 | 作用 |
|---|---|
| `domain.manifest.json` | 本域声明：属性清单、签发方要求、有效期、红线 |
| `gate-map.json` | 本域「什么动作要多少摩擦」：silent / warn / confirm / block / require-owner |
| `privacy.json` | 本域隐私声明：默认关闭、最小必要、可撤回、可删除 |
| `checker` | 本域核验器（MCP 工具，**只出示核验，不下判定**） |

## 核心原则

**平台只当擂台，不当货架。** 本域的核验器只回答「这条声明是否可核验、缺什么要件」，
不回答「这件事是否合规、该不该做」。判定权在本域的资质方、监管方与人。

**隐私是准入条件，不是整改事项。** 缺失 `privacy.json` 或任一必填字段不符，
符合性校验直接失败——不是警告，是拒绝接入。

## 🚀 快速开始 / Quick Start

> 本仓为声明式域骨架，**无需安装、无需运行时**；认领方填注 `operator` 与 `checker_endpoint` 后才接入平台校验器。

```bash
# 1. 克隆本仓
git clone https://github.com/SynomosAI/env-discharge-permit-checker.git
cd env-discharge-permit-checker

# 2. 通读四件套（纯声明文件，无依赖）
cat domain.manifest.json   # 属性清单 / 签发方要求 / 红线 / 有效期
cat gate-map.json          # 动作摩擦五级：silent/warn/confirm/block/require-owner
cat privacy.json           # 隐私声明（缺失或字段不符 → 校验直接失败）
cat checker.io.json        # MCP 核验器输入输出契约
```

核验器（认领方实现后）的一次典型输入，见 `checker.io.json` 的示例：

```json
{
  "subject_ref": "mpp:cn:9f2c1a7b",
  "attribute_keys": ["env.eia_status", "env.permit_status"],
  "evidence": ["tsa:2026-09-12#sha256:3b1f..."],
  "request_id": "req-20260912-0001"
}
```

返回结果只会是 `shown / missing / expired / unverifiable` 四态之一，**绝无**「可排放 / 可投产 / 已合规」类结论（见 `checker.io.json` 的 `_forbidden_outputs`）。

## 📁 仓库结构 / Repository layout

```text
env-discharge-permit-checker/
├── domain.manifest.json   # 域声明：属性 / 签发方要求 / 红线 / 有效期（domain/1.0）
├── gate-map.json          # 动作摩擦五级表（gate/1.0），每条红线必须有对应规则
├── privacy.json           # 隐私声明（privacy/1.0），准入条件
├── checker.io.json        # MCP 核验器输入输出契约（只出示核验，不下判定）
├── CODEOWNERS             # @SynomosAI/synomosai-gov
├── LICENSE                # 代码与配置：MIT
├── LICENSE-CONTENT        # 文档知识版权：All Rights Reserved（可注明出处引用）
├── SECURITY.md            # 安全策略
├── AGENTS.md              # 给 AI Agent 的工作指引
├── llms.txt               # 给 AI 的最小索引
├── llms-full.txt          # 给 AI 的完整文本
└── README.en.md           # English README
```

## 参考依据

- 中华人民共和国环境保护法
- 中华人民共和国环境影响评价法
- 排污许可管理条例
- 中华人民共和国固体废物污染环境防治法
- 排污许可管理办法

> 上列依据仅用于说明本域属性的来源与口径，不构成法律意见。具体适用以现行有效文本与主管部门解释为准。

## 认领方式

本域面向具备相应资质的机构开放。认领后请：

1. Fork 本仓，填注 `operator` 与 `checker_endpoint`
2. 按本域现行有效规则校准属性取值与红线表述
3. 跑平台侧校验器自测（五项判据全过方可提交）
4. 提 PR，附资质证明与规则依据

## 许可与署名

代码与配置按 MIT 许可使用。文档的知识版权归 SynomosAI 所有。

© 2026 SynomosAI. All rights reserved.

完整条款：代码与配置见 [LICENSE](LICENSE)（MIT）；文档知识版权见 [LICENSE-CONTENT](LICENSE-CONTENT)（All Rights Reserved，学术/新闻/评估引用请注明出处与条目）。安全问题请私下联系 [SECURITY.md](SECURITY.md)。
