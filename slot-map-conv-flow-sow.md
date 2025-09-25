# SOW Conversational Intake: Script + Slot Map

## 1. Mandatory Slots

| Slot ID | Description | Example Values | Source |
| --- | --- | --- | --- |
| contract_type | Type of contract | "SOW" | fixed |
| party_a | Customer name & jurisdiction | "Lattice AI Ltd", "UK" | user |
| party_b | Supplier name & jurisdiction | "Acme Logistics GmbH", "DE" | user |
| services_description | Scope of services/work | "Develop and deploy AI-powered logistics dashboard" | user |
| deliverables | List of deliverables | "Dashboard prototype", "Final production system", "Training materials" | user |
| milestones | Phases & target dates | "Phase 1: Prototype – 30 June 2025" | user |
| acceptance_criteria | How deliverables are accepted | "Written acceptance by Customer within 10 business days" | law firm variants |
| fees | Pricing model | "Fixed fee £50,000" / "Time & Materials £600/day" | user |
| payment_schedule | When payments are due | "50% on signing, 50% on delivery" | user |
| governing_law | Jurisdiction | "England and Wales" | user or default |
| venue | Venue for disputes | "London Courts" | user or default |

---

## 2. Conditional Slots

| Slot ID | Condition | Example Values |
| --- | --- | --- |
| expenses_reimbursable | if travel/expenses expected | "yes, at cost with receipts" |
| subcontracting_allowed | if supplier may delegate work | "yes_with_consent", "no" |
| warranty_period | if product deliverables | "90 days bug fix warranty" |
| service_levels | if ongoing support included | "Support response within 24h" |
| termination_rights | if project is long/multi-phase | "Either party may terminate with 30 days’ notice" |

---

## 3. Optional Slots

| Slot ID | Example Values |
| --- | --- |
| change_control | "Formal written process" |
| key_personnel | "John Doe, Project Manager must remain assigned" |
| reporting_requirements | "Weekly status report by email" |

---

## 4. Conversation Script (example)

- Type — confirm SOW and set `contract_type`.
- Parties — capture names and jurisdictions.
- Services & Deliverables — capture `services_description` and list deliverables.
- Milestones — capture milestone names and dates.
- Acceptance — capture `acceptance_criteria`.
- Fees & Payment — capture fee model and `payment_schedule`.
- Extras — ask expenses, warranty, and service levels if relevant.
- Law & Venue — confirm English law and London courts.
- Confirm & summarize; ask to generate.

---

## 5. Final JSON Output

```json
{
  "contract_type": "SOW",
  "parties": [
    {"name": "Lattice AI Ltd", "jurisdiction": "UK"},
    {"name": "Acme Logistics GmbH", "jurisdiction": "Germany"}
  ],
  "services_description": "Develop and deploy AI logistics dashboard",
  "deliverables": [
    "Prototype",
    "Final production system",
    "Training materials"
  ],
  "milestones": [
    {"name": "Prototype", "due_date": "2025-06-30"},
    {"name": "Final system", "due_date": "2025-09-30"},
    {"name": "Training", "due_date": "2025-10-15"}
  ],
  "acceptance_criteria": "Written acceptance by Customer within 10 business days",
  "fees": "Fixed fee £50,000",
  "payment_schedule": "50% on signing, 50% on delivery",
  "parameters": {
    "expenses_reimbursable": "yes_at_cost_with_receipts",
    "warranty_period": "90 days",
    "service_levels": "24h response",
    "governing_law": "England and Wales",
    "venue": "London Courts"
  }
}
```

---

## 6. Law Firm Deliverables (SOW)
1. Clause Corpus: scope/deliverables, milestones & acceptance variants, fee structures, expenses, warranty, SLA, change control & reporting, governing law & jurisdiction.
2. Parameters & Defaults: payment schedules (e.g., 30/70), acceptance windows (5/10/15 days), warranty terms (30–180 days), SLA response times (24h, 48h).
3. Playbook: allowed fee models, mandatory SLA minimums, default acceptance process.
4. Validation: 10–20 SOWs linked to MSAs; examples of accepted vs rejected redlines.
