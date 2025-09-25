## DPA Conversational Intake: Script + Slot Map

### 1. Mandatory Slots

| Slot ID | Description | Example Values | Source |
| --- | --- | --- | --- |
| contract_type | Contract type | "DPA" | fixed |
| party_controller | Controller (customer) | "Lattice AI Ltd", "UK" | user |
| party_processor | Processor (supplier) | "Acme Cloud Services GmbH", "DE" | user |
| services_description | Services requiring processing | "Hosting and managing SaaS platform" | user |
| data_categories | Categories of personal data | "employee data", "customer contact data", "payment info" | user |
| data_subjects | Whose data is processed | "employees", "customers", "suppliers" | user |
| processing_purposes | Why data is processed | "to provide SaaS services" | user |
| processing_duration | Duration of processing | "contract term + 6 months" | user |
| law | Governing law | "England and Wales" | user or default |
| venue | Venue for disputes | "London Courts" | user or default |

---

### 2. Conditional Slots

| Slot ID | Condition | Example Values |
| --- | --- | --- |
| subprocessors_allowed | Always asked | "yes_with_consent", "no", "yes_listed_only" |
| cross_border_transfers | If data leaves UK/EU | "yes_to_US", "no" |
| transfer_mechanism | If transfers = yes | "Standard Contractual Clauses", "UK Addendum", "Adequacy Decision" |
| security_measures | Always asked | "ISO 27001 compliance", "Encryption at rest + in transit", "Access controls" |
| breach_notification_hours | Always asked | "72 hours", "24 hours" |
| audit_rights | Always asked | "annual audit", "on request" |

---

### 3. Optional Slots

| Slot ID | Example Values |
| --- | --- |
| data_retention_policy | "delete after 12 months", "return or delete on termination" |
| liability_cap_data | "uncapped for data breaches", "aligned with MSA liability cap" |
| staff_training_required | "annual GDPR training" |

---

### 4. Conversation Script (example)

- Type — set `contract_type = DPA`.
- Parties — capture controller and processor details.
- Services — capture `services_description`.
- Data & Subjects — capture categories and subjects.
- Purpose & Duration — capture `processing_purposes` and `processing_duration`.
- Subprocessors — capture `subprocessors_allowed`.
- Transfers — capture `cross_border_transfers` and `transfer_mechanism`.
- Security & Breach — capture `security_measures` and `breach_notification_hours`.
- Audit — capture `audit_rights`.
- Law & Venue — confirm.
- Confirm & summarize; ask to generate.

---

### 5. Final JSON Output

```json
{
  "contract_type": "DPA",
  "party_controller": {"name": "Lattice AI Ltd", "jurisdiction": "UK"},
  "party_processor": {"name": "Acme Cloud Services GmbH", "jurisdiction": "DE"},
  "services_description": "Hosting and managing SaaS platform",
  "data_categories": ["customer contact data", "payment info"],
  "data_subjects": ["customers"],
  "processing_purposes": "to provide SaaS services",
  "processing_duration": "contract term + 6 months",
  "parameters": {
    "subprocessors_allowed": "yes_with_consent",
    "cross_border_transfers": "yes_to_US",
    "transfer_mechanism": "Standard Contractual Clauses",
    "security_measures": ["encryption at rest", "encryption in transit", "access controls"],
    "breach_notification_hours": 72,
    "audit_rights": "annual audit",
    "law": "England and Wales",
    "venue": "London Courts"
  }
}
```

---

### 6. Law Firm Deliverables (DPA)
1. Clause Corpus: GDPR Art. 28 obligations; subprocessor approval variants; transfer clauses (SCCs, UK Addendum, adequacy); security (ISO27001/NIST-aligned); breach notification; audit rights; return/delete; liability treatment.
2. Parameters & Defaults: breach notification 72h; baseline security; default subprocessor rule = with consent.
3. Playbook: SCCs vs UK Addendum; minimum security standards; deletion vs return preferences.
4. Validation: 10–20 signed DPAs; annotated preferred variants.
