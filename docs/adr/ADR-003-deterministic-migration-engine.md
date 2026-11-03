# ADR-003: Deterministic Legacy EHR Data Migration Engine (`Dry-Run` & `1-Click Rollback`)

* **Status:** Accepted
* **Date:** 2026-11-03
* **Author:** Guilherme Trentini, DDS (Clinical Product Lead & Health Informatics Architect)

---

## Context

Switching EHR/ERP systems is the single highest operational barrier for established medical, surgical, and dental practices. An audit of 65 commercial healthcare SaaS platforms revealed systematic vendor lock-in: exported datasets often contain broken character encodings, unstandardized local procedure names, orphaned clinical attachments (`DICOM`, radiographs, perioperative photography), and inconsistent patient identifier formatting.

Traditional ad-hoc migration scripts run directly against production databases, frequently corrupting longitudinal patient records or financial balances with no safe recovery path.

## Decision

We architect a dedicated core module (`M7: Deterministic Legacy EHR Migration Engine`) governed by a staged state machine (`competitor_import_jobs`):

1. **5 Canonical Ingestion Archetypes:**
   * **Archetype A:** Flat tabular exports (`CSV / TSV / XLSX`) with heuristic column mapping.
   * **Archetype B:** Relational database dumps (`PostgreSQL / MySQL / Firebird / SQLite`).
   * **Archetype C:** Healthcare interoperability payloads (`HL7 v2.x`, `HL7 FHIR R4 Bundles`, `CCDA`).
   * **Archetype D:** Proprietary XML/JSON fiscal and insurance claim archives (`ASC X12 837` / `ANS TISS`).
   * **Archetype E:** Binary clinical media archives (`DICOM` imaging series, intraoral/3D scans, PDF consents, and perioperative clinical photography) indexed by content hash (`SHA-256`).

2. **Mandatory Pre-Flight `Dry-Run` & Deterministic Checksums:**
   * Every migration job runs first in an isolated staging schema.
   * Patient identifiers, provider registries, and terminology mappings pass through deterministic checksum validators before any production row is written.
   * Generates a human-readable **Migration Reconciliation Report** detailing exact record counts, quarantined anomalies, and financial balance parity.

3. **Atomic `1-Click Rollback`:**
   * All imported records are tagged with an immutable `import_job_id` lineage token within the tenant's `PostgreSQL RLS` boundary, enabling complete, zero-residue rollback if the clinic rejects post-migration verification.

## Consequences

### Positive
* **Zero-Downtime Clinic Onboarding:** Practices can preview and verify 100% of their historical clinical charts and imaging inside a sandboxed `Dry-Run` before cutting over live operations.
* **Data Integrity Guarantee:** Eliminates silent truncation or corrupted patient histories during onboarding.

### Trade-offs
* Increases storage overhead during the onboarding window due to staging isolation and lineage metadata tracking.
