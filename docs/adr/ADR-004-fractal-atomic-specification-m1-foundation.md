# ADR-004: Fractal Atomic Decomposition for Module 1 Foundation (`13 Atomic Units / 26 Paired Specs`)

* **Status:** Accepted
* **Date:** 2026-11-03
* **Author:** Guilherme Trentini, DDS (Clinical Product Lead & Health Informatics Practitioner, directing AI agents via Clinical Macro/Micromanagement)

---

## Context

When orchestrating autonomous AI coding agents on large-scale clinical software, monolithic module specifications fail. If a single document attempts to specify multi-tenant authentication, cryptographic PHI encryption, `HL7 FHIR` data types, `SNOMED CT` terminology tables, and national tax/regulatory rules at once, AI agents suffer from context dilution and cross-contaminate universal core tables with local country rules.

## Decision

For **Phase II (Modular Deep-Dive)**, we decomposed **Module 1 (Platform Foundation, Security & Clinical Governance)** into **13 Atomic Units (`1.1.A` through `1.6.B`)**. Each atomic unit is governed by a strict **1:1 Paired Specification Contract** (`26 specifications total`): one 100% jurisdiction-agnostic **Universal Core specification (`en-US`)** paired with its corresponding **Country Pack adapter specification**.

### The 13 Atomic Units of Module 1

| Unit | Universal Core Contract (`en-US`) | Country Pack Adapter (`@templus/pack-*`) |
| :--- | :--- | :--- |
| **`1.1.A`** | Monorepo workspace topology, `PackManifest` schema, 7 Hexagonal Ports & 5 UI Extension Slots | National identity port bindings, tax ID validators, and localized UI slot injections |
| **`1.1.B`** | `HL7 FHIR R5 MonetaryComponent` (6-decimal precision) & `UCUM` physical units | Local currency rules, address lookup schemas, and statutory rounding policies |
| **`1.1.C`** | `SNOMED CT` Verhoeff dihedral $D_5$ check, `ISO 3950` dental/craniofacial notation & terminology SQL | National procedure crosswalks (`AMA CPT` / `ADA CDT` / `ANS TUSS` / `CBHPM`) and device registries |
| **`1.2.A`** | `PostgreSQL Row-Level Security (RLS)` tenant isolation & `WORM` hash-chained immutable audit logs | Statutory health data retention windows (e.g., 20-year longitudinal medical record preservation) |
| **`1.2.B`** | `AES-256-GCM` envelope encryption with AAD, `HMAC-SHA256` blind indexing for searchable PHI & `Argon2id` | National PKI hardware/cloud signature adapters (`21 CFR Part 11` / `eIDAS` / `ICP-Brasil`) |
| **`1.3.A`** | Edge routing, Zero-Training Clinical Design System (`<= 3 clicks` budget) & latency budgets | Regional cloud edge deployment (`AWS us-east-1` / `eu-central-1` / `sa-east-1`) and locale formatting |
| **`1.3.B`** | Global 7-Pillar Clinical Shell, single-sheet workspace & `Cmd+K` instant command palette | Localized clinical keyboard shortcuts and statutory terminology labels |
| **`1.4.A`** | Dual-Auth clinical session architecture (fast chairside/OR `PIN` switch) & fine-grained `RBAC` | Role privacy boundaries for external accounting and statutory audit personnel |
| **`1.4.B`** | Provider license verification engine, clinical supervision & procedure delegation gates | Regional medical/dental council verification (`State License` / `NPI` / `DEA` / `CRM` / `CRO`) |
| **`1.5.A`** | Multi-entity clinic hierarchy & "Chameleon" specialty-adaptive onboarding | Healthcare corporate tax regimes, hospital-equivalence rules, and multi-TIN structures |
| **`1.5.B`** | Regulatory compliance matrix and cryptographic patient/provider waiver governance | Statutory specialty council rules and sanitary surveillance licensing checks |
| **`1.6.A`** | Operating rooms, dental chairs, and medical equipment registry with maintenance interlocks | Sanitary device registration validation (`FDA UDI` / `ANVISA`) and municipal service taxonomy |
| **`1.6.B`** | SaaS FinOps metering, multi-clinic HQ governance & 20-year longitudinal continuity | Localized subscription billing rails, consumer protection compliance, and archival rules |

## Consequences

### Positive
* **Zero Context Dilution for AI Agents:** Coding agents execute against small, self-contained atomic contracts where SQL schemas, cryptographic invariants, and port interfaces are explicitly locked.
* **Mathematical Parity Between Core and Country Packs:** Every universal capability has a guaranteed, auditable adapter contract for local regulations without leaking a single country-specific column into the Core.

### Trade-offs
* Requires rigorous upfront architectural discipline (writing and auditing 26 paired specifications for Module 1 alone before generating application code).
