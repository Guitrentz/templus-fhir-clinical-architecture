# ADR-002: Zero-Training Clinical UX (`<= 3 Clicks`) and Perioperative Surgical Go/No-Go Flight Deck

* **Status:** Accepted
* **Date:** 2026-11-03
* **Author:** Guilherme Trentini, DDS (Clinical Product Lead & Health Informatics Architect)

---

## Context

Commercial EHR systems are historically designed around administrative billing capture rather than clinical ergonomics. In high-acuity outpatient surgery, oral & maxillofacial procedures, and facial aesthetic interventions, clinicians face two severe hazards caused by legacy software:
1. **Cognitive Overload & Click Fatigue:** Navigating across 10+ nested tabs and pop-up modals to review allergies, medical history, imaging, and surgical notes diverts attention away from the patient.
2. **Perioperative Oversight:** Critical pre-surgical risk factors (`ASA` classification, `Mallampati` airway score, active anticoagulants or bisphosphonates, latex/anesthetic allergies, and unsigned procedural consents) are passively stored in text fields without active pre-incision interlocks.

## Decision

We establish two mandatory product engineering invariants across all clinical modules:

1. **Zero-Training Clinical UX (`<= 3 Clicks` Single-Sheet Cockpit):**
   * **Single-Sheet Zero-Scroll Architecture:** The patient's clinical encounter operates on a unified single-sheet workspace where medical alerts, anatomical charting (`ISO 3950` odontogram and craniofacial layers), active medications, and clinical timeline are simultaneously visible without tab switching.
   * **Strict Interaction Budget:** Every high-frequency clinical action must be executable in **3 clicks or fewer**, validated during UI design and automated end-to-end testing.

2. **Perioperative Surgical Safety (Go/No-Go Flight Deck):**
   * Any surgical or invasive encounter triggers a mandatory **Go/No-Go Flight Deck** check before procedure start.
   * The Flight Deck evaluates deterministic clinical rules against structured `SNOMED CT` and `LOINC` patient records: systemic allergies, drug-procedure contraindications, preoperative vitals, airway assessment, signed informed consent, and sterile implant/biomaterial availability (`UDI` / `QR` scan with `FEFO` expiration verification).

## Consequences

### Positive
* **Elimination of Onboarding Friction:** Clinicians and surgical teams can operate the cockpit intuitively on day one without multi-week EHR training manuals.
* **Active Patient Safety at the Point of Care:** Transforms the EHR from a passive billing repository into an active perioperative safety interlock.
* **Complete Biomaterial Traceability:** Every implanted device, bone graft, or injectable biologic is cryptographically linked to its lot, expiration date, and anatomical site in 1 scan.

### Trade-offs
* Enforcing a `<= 3 clicks` budget requires disciplined information hierarchy and progressive disclosure, rejecting feature-bloat requests that would clutter the primary clinical canvas.
