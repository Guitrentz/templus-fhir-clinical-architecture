# ADR-005: Multi-Resource GiST Scheduling, Zero-PHI CRM Segregation & Post-Op Recall (`Module 2: 8 Atomic Units / 16 Paired Specs`)

* **Status:** Accepted
* **Date:** 2026-11-04
* **Author:** Guilherme Trentini, DDS (Clinical Product Lead & Health Informatics Practitioner, directing AI agents via Clinical Macro/Micromanagement)

---

## Context

In outpatient surgical centers, oral & maxillofacial practices, and multi-specialty clinics, scheduling and patient communication systems routinely fail on three fronts:
1. **Resource Collisions:** Traditional calendars only check clinician availability, allowing double-booking of shared operating rooms, dental chairs, or mobile surgical equipment (such as lasers, piezo-surgery units, or 3D scanners).
2. **PHI Leakage in Commercial CRM Pipelines:** Most healthcare CRMs store prospective leads and clinical patients in the same database table, exposing sensitive Protected Health Information (PHI) and medical histories to commercial call-center and marketing staff.
3. **Disconnected Post-Operative Follow-Up:** After a surgical procedure, patient messages arrive in unmonitored WhatsApp or SMS threads without structured clinical triage or immutable audit trails.

## Decision

For **Phase II, Module 2 (Multi-Resource Scheduling, Chair/OR Flow, Digital Check-In, Zero-PHI Growth CRM & Unified Inbox)**, we decomposed the architecture into **8 Atomic Units (`2.1.A` through `2.4.B`)**, locking **16 Paired Specifications** (`8 Universal Core contracts` in `en-US` paired 1:1 with `8 Country Pack adapters`):

### The 8 Atomic Units of Module 2

| Unit | Universal Core Contract (`en-US`) | Country Pack Adapter (`@templus/pack-*`) |
| :--- | :--- | :--- |
| **`2.1.A`** | Multi-resource calendar (`provider + room + equipment`) enforced by PostgreSQL `EXCLUDE USING gist` range constraints | National banking/statutory holiday calendars and conversational booking slot adapters |
| **`2.1.B`** | Real-time Chair/OR flow state machine, smart waitlist backfill, and no-show deposit ledger | Instant payment deposit rails (e.g., `ACH/Card` or `Pix`) and statutory cancellation/refund rules |
| **`2.2.A`** | Digital pre-arrival check-in, tablet kiosk mode, voice intake & side-by-side diff reconciliation | National ID/postal validation, biometric/voice consent terms, and statutory privacy disclosures |
| **`2.2.B`** | Pre-procedure clinical risk assessment (`ASA`, allergies, anticoagulants) & delegation interlocks | Regional council responsibility rules and cryptographic pre-op sign-off (`21 CFR Part 11` / `ICP-Brasil`) |
| **`2.3.A`** | **Zero-PHI Commercial CRM**: strict table segregation between `crm_leads` and clinical `patients` until conversion | Data subject rights workflows (`HIPAA` / `GDPR` / `LGPD Art. 11 & 18`) and marketing opt-in audit |
| **`2.3.B`** | Longitudinal clinical recall & automated post-operative surgical monitoring (`D+1`, `D+3`, `D+7` red-flag triage) | Specialty council rules on ethical clinical follow-up and multi-disciplinary recall cadence |
| **`2.4.A`** | Unified Patient Inbox with `WORM` immutable message logs, clinical confidentiality gates & `Magic Link` delivery | Sensitive health data masking in external notifications and bring-your-own messaging account FinOps |
| **`2.4.B`** | Hexagonal Messaging & Postal Port (`Port 3`), idempotent webhook state machine & dead-letter recovery | WhatsApp Business Coexistence (`CoEx`), SMS gateways, and national postal/municipality code engines |

## Consequences

### Positive
* **Mathematical Elimination of Double-Booking:** Database-level `GiST` exclusion constraints prevent overlapping appointments across clinicians, surgical suites, and sterilized equipment even under concurrent high-volume booking.
* **Privacy by Design in Clinic Growth:** Sales and reception teams manage commercial leads with zero access to clinical charts or PHI, satisfying strict `HIPAA` Minimum Necessary and `GDPR/LGPD` health data segregation rules.
* **Active Post-Surgical Safety Net:** Automated post-op check-ins detect pain, swelling, fever, or bleeding early and escalate directly to the operating surgeon's cockpit.

### Trade-offs
* Enforcing strict table segregation between `crm_leads` and `patients` requires an atomic conversion transaction when a lead arrives for their first clinical encounter.
