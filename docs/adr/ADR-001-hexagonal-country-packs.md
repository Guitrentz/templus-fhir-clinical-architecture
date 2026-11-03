# ADR-001: Hexagonal Multi-Jurisdictional Core and Pluggable Country Packs

* **Status:** Accepted
* **Date:** 2026-11-03
* **Author:** Guilherme Trentini, DDS (Clinical Product Lead & Health Informatics Architect)

---

## Context

Healthcare EHR/ERP platforms that expand beyond a single country frequently suffer from architectural entanglement. Developers hardcode country-specific tax identifiers, national procedure catalogs (`AMA CPT` / `ADA CDT` in the US vs. `ANS TUSS` in Brazil), e-prescribing gateways (`DEA EPCS` vs. `ANVISA SNCR`), and cryptographic signature standards (`21 CFR Part 11` vs. `eIDAS` vs. `ICP-Brasil`) directly into core database tables and UI components.

This entanglement creates two critical failures:
1. Forking the codebase per country destroys maintainability and clinical safety parity.
2. Conditional branching (`if country == 'US'`) inside clinical charting modules increases regression risk and violates regulatory audit boundaries.

## Decision

We enforce a strict **Hexagonal Architecture (Ports and Adapters)** that separates the platform into two distinct layers:

1. **Universal Agnostic Core (`en-US`):**
   * Contains zero country-specific columns, tax rules, or statutory workflows.
   * Uses `HL7 FHIR R4/R5` canonical structures, `SNOMED CT` clinical concepts, `LOINC` observations, and `ISO 3950` anatomical notation as the universal internal lingua franca.
   * Enforces multi-tenant isolation at the database engine layer via `PostgreSQL Row-Level Security (RLS)` across 49 canonical tables.

2. **Pluggable Country Packs (`@templus/pack-us`, `@templus/pack-eu`, `@templus/pack-br`):**
   * Each jurisdiction is implemented as an isolated module conforming to a strongly-typed `PackManifest`.
   * Country Packs interact with the Core exclusively through **7 Jurisdictional Ports** (Identity, Terminology Crosswalk, e-Prescribing, PKI Signature, Claims/HIE, Payment/Fiscal Split, and Privacy/Residency) and inject UI fragments solely through **5 deterministic UI Extension Slots**.

## Consequences

### Positive
* **Zero Core Pollution:** Clinical innovations in the Single-Sheet Cockpit or Perioperative Surgical Flight Deck immediately benefit all jurisdictions without touching local regulatory adapters.
* **Isolated Regulatory Certification:** Auditing `@templus/pack-us` for `HIPAA` / `ONC Health IT` or `@templus/pack-br` for `LGPD` / `ANVISA` / `ICP-Brasil` is scoped strictly to the respective Country Pack adapter and its data residency boundary.
* **Rapid Market Entry:** Adding a new jurisdiction requires implementing the 7 Port interfaces and 5 UI Slots rather than rewriting clinical charting workflows.

### Trade-offs
* Requires upfront semantic mapping between international `SNOMED CT` concepts in the Core and local billing/procedure catalogs (`CPT/CDT/TUSS`) inside each Country Pack's Terminology Crosswalk Port.
