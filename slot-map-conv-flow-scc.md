## SCC Conversational Intake: Script + Slot Map

### 1. Mandatory Slots

| Slot ID | Description | Example Values | Source |
| --- | --- | --- | --- |
| contract_type | Type of contract | "SCC" | fixed |
| exporter | Data exporter (usually Controller in UK/EU) | "Lattice AI Ltd", "UK" | user |
| importer | Data importer (outside UK/EU) | "Acme Cloud Inc.", "US" | user |
| transfer_type | Which SCC module applies | "Controller → Processor", "Controller → Controller", "Processor → Processor", "Processor → Controller" | user |
| services_description | Services requiring transfer | "SaaS hosting in US" | user |
| data_categories | Types of data transferred | "customer names", "emails", "payment data" | user |
| data_subjects | Whose data is transferred | "employees", "customers", "suppliers" | user |
| purpose | Purpose of processing | "to provide SaaS services" | user |
| duration | Duration of transfer arrangement | "contract term + 6 months" | user |

---

### 2. Conditional Slots

| Slot ID | Condition | Example Values |
| --- | --- | --- |
| onward_transfers | If importer uses further recipients | "yes_with_consent", "no" |
| subprocessors | If importer is processor | "yes_listed", "yes_with_notice", "no" |
| security_measures | Always required | "encryption at rest", "pseudonymisation", "access controls" |
| governing_law | Law of EU member state permitting SCC | "Ireland", "Germany", "France" |
| supervisory_authority | Supervisory authority | "Irish DPC", "CNIL", "ICO (UK Addendum)" |

---

### 3. Optional Slots

| Slot ID | Example Values |
| --- | --- |
| technical_org_measures | Detailed TOMs ("firewalls, SOC monitoring, backups") |
| importer_contact | DPO contact at importer |
| exporter_contact | DPO contact at exporter |

---

### 4. Conversation Script (example)

- Type — confirm SCCs for data transfers.
- Parties — capture exporter and importer details.
- Transfer Module — capture selected module.
- Services — capture `services_description`.
- Data & Subjects — capture categories and subjects.
- Purpose & Duration — capture.
- Onward Transfers & Subprocessors — capture preferences.
- Security — capture TOMs.
- Governing Law & Supervisory Authority — capture selections.
- Confirm & summarize; ask to generate.

---

### 5. Final JSON Output

```json
{
  "contract_type": "SCC",
  "exporter": {"name": "Lattice AI Ltd", "jurisdiction": "UK"},
  "importer": {"name": "Acme Cloud Inc.", "jurisdiction": "US"},
  "transfer_type": "Controller_to_Processor",
  "services_description": "SaaS hosting in US",
  "data_categories": ["customer names", "emails", "payment data"],
  "data_subjects": ["customers"],
  "purpose": "to provide SaaS services",
  "duration": "contract term + 6 months",
  "parameters": {
    "onward_transfers": "no",
    "subprocessors": "yes_listed",
    "security_measures": ["encryption", "pseudonymisation", "access controls"],
    "governing_law": "Ireland",
    "supervisory_authority": "Irish DPC"
  }
}
```

---

### 6. Law Firm Deliverables (SCCs)
1. Clause Corpus: all four modules (C→C, C→P, P→P, P→C); Annex I–III templates; UK Addendum alongside EU SCC text.
2. Parameters & Defaults: security measures baseline; supervisory authority mapping; default duration = contract term.
3. Playbook: when to use SCCs vs UK Addendum; mandatory TOMs; policy on uncapped liability for data breaches (if required).
4. Validation: signed SCCs + UK Addendum samples; annotated module selections.
