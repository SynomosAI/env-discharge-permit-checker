# env-discharge-permit-checker

[![License: MIT](https://img.shields.io/badge/Code%20License-MIT-yellow.svg)](LICENSE)
[![Content: ARR](https://img.shields.io/badge/Content%20License-ARR%20%7C%20citable-blue.svg)](LICENSE-CONTENT)

> **Status: RESERVED (placeholder · open for claim)** — this repository already ships the four-file industry-access skeleton per the platform protocol, and waits for an operator with domain credentials to claim it and fill in real rules.

This is the SynomosAI **domain profile** for pollution discharge permits and environmental impact assessment (EIA) obligations. It decomposes "may this project discharge, how much, and what procedures are required" into verifiable attributes, so that AI only provides prerequisite guidance — never a production or clearance conclusion.

## What this domain governs

Discharge permit and EIA obligation verification.

EIA, discharge permitting, and hazardous-waste management are distinct but interlocking regimes; starting construction before approval, discharging without a permit, and tampering with monitoring data are the classic violations. AI can lay out permit status vs. monitoring duties; the production and discharge power belongs to the ecology & environment authority.

## Domain identity

| Field | Value |
|---|---|
| Domain ID | `env` (globally unique, never reused) |
| Domain name | Environmental · discharge permit & EIA obligations |
| Profile version | `domain/1.0` |
| Status | `RESERVED` |
| Reserved at | 2026-09-12 |

## Attributes

| Key | Type | Meaning |
|---|---|---|
| `env.eia_status` | enum | EIA document approval / filing status · approved/filing/pending/expired/not_required |
| `env.permit_status` | enum | Discharge permit validity per authority records · valid/expired/none/not_required |
| `env.emission_compliance` | enum | Compliance with concentration AND total-quota limits · compliant/exceedance/unknown |
| `env.monitoring_status` | enum | Self-monitoring plan implementation & data disclosure · automatic/manual/none/not_required |
| `env.hazardous_waste` | enum | Hazardous waste generation/storage/manifest management · managed/deviation/none/not_applicable |

## Red lines (machine-readable, non-negotiable)

1. Never output "may discharge / may go into production" — the ecology & environment authority's permit is the sole basis.
2. Never state that production may continue under exceedance, unpermitted discharge, or anomalous monitoring data.
3. Never suggest dilution-to-meet-standard, bypass pipes, tampering with monitoring data, or retroactive legalization.

Each red line has a matching blocking rule in `gate-map.json`; the platform validator fails if coverage is missing.

## Quick Start / 快速开始

This repo is a declarative skeleton — no runtime, no install.

```bash
git clone https://github.com/SynomosAI/env-discharge-permit-checker.git
cd env-discharge-permit-checker
python -m json.tool domain.manifest.json    # validate the manifest
cat checker.io.json                          # read the MCP tool contract
```

The checker MCP tool returns only four states — `shown / missing / expired / unverifiable` — and is forbidden from outputting "may-discharge / compliant" conclusions (see `_forbidden_outputs` in `checker.io.json`).

## Repository layout

```text
domain.manifest.json   # declaration: attributes / issuer requirements / red lines / validity
gate-map.json          # friction levels per action class (gate/1.0)
privacy.json           # privacy notice (privacy/1.0) — an admission condition
checker.io.json        # MCP checker I/O contract (show & verify, never judge)
LICENSE                # code & config: MIT
LICENSE-CONTENT        # prose content: All Rights Reserved (citable with attribution)
```

## References

- Environmental Protection Law of the PRC (环境保护法)
- Environmental Impact Assessment Law of the PRC (环境影响评价法)
- Regulations on Discharge Permit Administration (排污许可管理条例)
- Law on Solid Waste Pollution Prevention and Control of the PRC (固体废物污染环境防治法)
- Measures for the Administration of Discharge Permits (排污许可管理办法)

The above references only explain the origin of the domain's attributes; they are not legal advice.

## Claiming this domain

The domain is open to qualified institutions. To claim:

1. Fork this repo and fill in `operator` and `checker_endpoint`.
2. Calibrate attribute values and red lines per current rules.
3. Run the platform validator self-test (all five criteria must pass).
4. Open a PR with credential proof and rule citations.

## License

Code and configuration are MIT-licensed. Prose content is copyrighted by SynomosAI.

© 2026 SynomosAI. All rights reserved.

Full terms: [LICENSE](LICENSE) (MIT) · [LICENSE-CONTENT](LICENSE-CONTENT) (ARR). Report security issues privately via [SECURITY.md](SECURITY.md).
