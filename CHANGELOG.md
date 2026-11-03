# Changelog

All notable architectural and clinical engineering milestones for **Templus** are documented in this file.

## [v0.3.0-alpha] - 2026-11-03

### Added
* **Universal Agnostic Core Blueprint (`Phase I`):**
  * Defined the 100% jurisdiction-agnostic core clinical domain model across 7 integrated modules (`M1` Clinical Cockpit & Odontogram, `M2` Perioperative Surgical Safety, `M3` Ambient Clinical Scribe & CDS, `M4` Smart Scheduling, `M5` Revenue Cycle & Split Engine, `M6` Sterile Inventory & FEFO Traceability, `M7` Legacy Data Migration).
  * Established the **7 Strict Jurisdictional Hexagonal Ports** and **5 UI Extension Slots** for decoupling national regulations (`US`, `EU`, `BR`) from core clinical logic ([ADR-001](./docs/adr/ADR-001-hexagonal-country-packs.md)).
  * Designed the canonical multi-tenant persistence architecture (`49 PostgreSQL Tables`) enforced by `Row-Level Security (RLS)` tenant isolation policies and immutable audit logs.
* **Zero-Training Clinical UX & Surgical Safety Specification:**
  * Codified the **Single-Sheet Zero-Scroll Cockpit** governed by a strict **<= 3 clicks interaction budget** ([ADR-002](./docs/adr/ADR-002-zero-training-clinical-ux.md)).
  * Specified the **Perioperative Surgical Safety (Go/No-Go Flight Deck)** with automated clinical interlocks (`ASA`, `Mallampati`, anticoagulants, `SNOMED CT` allergy hierarchies) and sterile `UDI / QR / FEFO` biomaterial lot tracking.
* **Jurisdictional Country Pack Specification (`@templus/pack-br` Case Study):**
  * Completed full regulatory adapter specification for Brazil (`AWS sa-east-1` data residency, `LGPD`, `ANVISA RDC 657 & RDC 1.002/2026 SNCR`, `ICP-Brasil` cloud cryptographic signatures, `HL7 FHIR BR Core / RNDS`, `ANS TISS`, and automated healthcare provider payment split).
* **Deterministic Legacy EHR Migration Engine (`Phase IA`):**
  * Audited 65 commercial medical, dental, oral surgery, and aesthetic SaaS systems across 6 market clusters to identify data lock-in patterns.
  * Architected the **Deterministic Migration Engine** supporting 5 legacy payload archetypes (`CSV/TSV`, `SQL Dumps`, `HL7 v2 / FHIR Bundles`, `XML/JSON Exports`, and `DICOM / Clinical Media Archives`) with pre-flight schema validation, `Dry-Run` staging, and atomic `1-Click Rollback` ([ADR-003](./docs/adr/ADR-003-deterministic-migration-engine.md)).
