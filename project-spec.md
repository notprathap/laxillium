# PRD: Commercial Contract Generator (UK/EU, v1)

---

## 1. Objective

Develop a private system that generates commercial contracts (NDA, MSA, SOW, DPA, SCC) for UK/EU jurisdictions.

### Key Goals:
- Generate contracts from a validated clause corpus (supplied/approved by partner law firm)
- Intake contract requirements via a conversational AI agent
- Output legally enforceable drafts in .docx with embedded JSON manifest for audit
- Provide a Word add-in for variant toggling and regeneration

---

## 2. Users & Use Cases

- **Business user (non-lawyer):** Converses with AI agent to generate NDA or SOW quickly
- **In-house counsel:** Reviews generated draft, toggles clause variants, edits parameters
- **Compliance lead:** Audits which clauses, variants, and rules were used

---

## 3. Functional Requirements

### 3.1 Clause Corpus (Law Firm Deliverables)

The law firm must provide an annotated, machine-readable clause library, structured as JSONL.

**Each clause must include:**
- `id` (unique identifier, versioned)
- `category` (e.g., Liability, Confidentiality, Termination, IP, Governing Law, Data Protection)
- `jurisdiction` (UK / EU / both)
- `variants` (e.g., "standard", "supplier-friendly", "customer-friendly")
- `text_by_variant` (clause text with placeholders for parameters)
- `parameters` (name, type, allowed values/ranges, defaults)
- `instrument_refs` (statutory references, e.g., "GDPR Art. 28", "UCTA 1977")
- `notes` (law firm commentary on when/how to use the clause)

**Example:**

```json
{
  "id": "limitation_liability_uk_v1",
  "category": "Limitation of Liability",
  "jurisdiction": "UK",
  "variants": ["standard", "supplier_friendly", "customer_friendly"],
  "text_by_variant": {
    "standard": "Subject to Clause ${exclusions_ref}, each party's liability is capped at ${cap_percent}% of the Fees paid in the preceding ${lookback_months} months..."
  },
  "parameters": {
    "cap_percent": {"type": "number", "default": 100, "min": 50, "max": 300},
    "lookback_months": {"type": "integer", "default": 12, "enum": [12, 24]}
  },
  "instrument_refs": ["Unfair Contract Terms Act 1977"],
  "notes": "Use supplier_friendly only if counterparty is SME with low leverage"
}
```

**Categories for v1 corpus (minimum 10):**
- Confidentiality
- Limitation of Liability
- Intellectual Property
- Termination
- Governing Law & Jurisdiction
- Payment Terms
- Data Protection (GDPR)
- Subcontracting & Assignment
- Anti-bribery / Modern Slavery
- Service Levels & SLAs

---

### 3.2 Assembly Recipes

- JSON/YAML definitions that specify which clause categories + variants form a complete contract
- Each recipe supports conditional modules (e.g., include SCC module if cross-border transfers flagged)

**Example (NDA recipe):**

```yaml
contract_type: NDA
jurisdiction: UK
clauses:
  - {id: "definitions_general_v1"}
  - {id: "confidentiality_obligations_v3", variant: "standard", params: {"term_years": 3}}
  - {id: "exceptions_confidentiality_v2"}
  - {id: "return_destruction_v1"}
  - {id: "governing_law_ew_v2"}
  - {id: "venue_london_v1"}
```

---

### 3.3 Conversational Intake Agent

- **Mode:** Conversational web/Word add-in chat
- **Backend:** Slot-filling + orchestration with SaulLM-7B / Llama-3.1
- **Flow:**
  - **Start:** ask contract type
  - **Branch:** ask only relevant slots for that type
  - **Confirm:** repeat back to user before generating draft
  - **Output:** JSON payload with structured fields

**Example Output:**

```json
{
  "contract_type": "MSA",
  "parties": [
    {"name": "Lattice AI Ltd", "jurisdiction": "UK"}, 
    {"name": "Beta Ventures GmbH", "jurisdiction": "DE"}
  ],
  "jurisdiction": {"law": "England and Wales", "venue": "London Courts"},
  "parameters": {"liability_cap_percent": 100, "notice_days": 30, "sla_uptime": "99.9"}
}
```

---

### 3.4 Contract Generation Pipeline

1. **Planner:** Map intake JSON → assembly recipe (clause IDs, variants, params)
2. **Composer:** Retrieve text from clause corpus; substitute parameters; stitch into contract skeleton
3. **Verifier:** Check coverage, parameter substitution, cross-refs, and compliance rules
4. **Exporter:** Generate .docx (with numbering/cross-refs) and embed JSON manifest for audit

---

### 3.5 Word Add-in

**Side pane (Office.js) with:**
- Chat-based intake (same conversational agent)
- Clause metadata (jurisdiction, rationale, statute)
- Buttons: switch variant, change parameter, regenerate clause

---

## 4. Non-Functional Requirements

- **Data residency:** UK/EU only
- **Privacy:** no contract data sent to external APIs
- **Audit:** manifest with clause IDs + variants stored in docx metadata
- **Versioning:** clause corpus under Git, each clause versioned

---

## 5. Inputs Required from Partner Law Firm

1. **Clause Corpus:** annotated JSONL, min. 10 categories, 2–3 variants each
2. **Contract Skeletons:** base Word/Markdown templates with clause slots
3. **Playbook Rules:** fallback positions, deal-leverage conditions, parameter ranges
4. **Statute Mapping:** clause ↔ relevant UK/EU law references
5. **Validation Corpus:** sample intake responses + approved final drafts to test system

---

## 6. Evaluation Metrics

- **Coverage:** % required categories present
- **Slot Completion Rate:** % mandatory intake fields filled correctly
- **Parameter Accuracy:** match between user intake and contract text
- **Reviewer Accept Rate:** % of clauses accepted without edits
- **Draft turnaround time:** NDA <20s, MSA <60s

---

## 7. Roadmap

### Phase 1 (4–6 weeks):
- Build clause corpus + assembly recipes with law firm
- Implement conversational intake → JSON
- Generate NDA/MSA drafts → docx export

### Phase 2 (6–8 weeks):
- Word add-in for interactive toggling/regeneration
- Governance dashboards (coverage, usage)

### Phase 3:
- LoRA/SFT for tone/style
- Live grounding (EUR-Lex, legislation.gov.uk)
- Negotiation mode (counterparty redlines)
