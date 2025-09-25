# NDA Conversational Intake: Script + Slot Map

## 1. Mandatory Slots

| Slot ID | Description | Example Values | Source |
| --- | --- | --- | --- |
| contract_type | Type of contract | "NDA" | fixed |
| party_a | Name & jurisdiction of first party | "Lattice AI Ltd", "UK" | user |
| party_b | Name & jurisdiction of second party | "Beta Ventures GmbH", "Germany" | user |
| purpose | Business purpose of NDA | "exploring a potential partnership in AI software" | user |
| governing_law | Law governing the agreement | "England and Wales" | user or default |
| venue | Court/venue for disputes | "London Courts" | user or default |
| term_years | Duration of confidentiality obligation | 3 | user |
| disclosure_type | Mutual or one-way NDA | "mutual", "one-way" | user |
| exceptions | Exceptions to confidentiality | "public information, already known, independently developed" | pre-set list |
| return_destruction | Obligation on return/destruction of materials | "yes" | default |

---

## 2. Optional Slots

| Slot ID | Description | Example Values |
| --- | --- | --- |
| notice_method | How notices are delivered | "email + registered mail" |
| survival_clauses | Which obligations survive termination | "confidentiality, governing law" |
| third_party_disclosure | Permitted disclosures to advisors | "yes, under same obligations" |

---

## 3. Conversation Script (example)

- Step 1: Greeting & Type
  - Agent: "Hi, I’m your contract assistant. What type of contract do you need today?"
  - User: "I need an NDA."
  - Agent sets `contract_type = NDA`.
- Step 2: Parties
  - Agent: "Who are the parties to this NDA?"
  - User: "Lattice AI Ltd in the UK and Beta Ventures GmbH in Germany."
  - Extracts:
    ```json
    {
      "party_a": {"name": "Lattice AI Ltd", "jurisdiction": "UK"},
      "party_b": {"name": "Beta Ventures GmbH", "jurisdiction": "Germany"}
    }
    ```
- Step 3: Purpose
  - Agent: "What’s the purpose of sharing confidential information?"
  - User: "We’re exploring a partnership in AI software."
  - Capture `purpose = "exploring a partnership in AI software"`.
- Step 4: Disclosure type
  - Agent: "Will both parties disclose confidential information, or just one side?"
  - User: "Both sides."
  - Capture `disclosure_type = "mutual"`.
- Step 5: Term
  - Agent: "How long should confidentiality last?"
  - User: "Three years."
  - Capture `term_years = 3`.
- Step 6: Governing law & venue
  - Agent: "Use English law and London courts?"
  - User: "Yes."
  - Capture `governing_law = "England and Wales"`, `venue = "London Courts"`.
- Step 7: Confirm & Summarize
  - Agent summarizes and asks to generate the draft.

---

## 4. Final JSON Output

```json
{
  "contract_type": "NDA",
  "parties": [
    {"name": "Lattice AI Ltd", "jurisdiction": "UK"},
    {"name": "Beta Ventures GmbH", "jurisdiction": "Germany"}
  ],
  "purpose": "exploring a partnership in AI software",
  "disclosure_type": "mutual",
  "parameters": {
    "term_years": 3,
    "governing_law": "England and Wales",
    "venue": "London Courts",
    "exceptions": ["public information", "already known", "independently developed"],
    "return_destruction": true
  }
}
```

---

## 5. Law Firm Deliverables (NDA)
1. Clause Corpus: confidentiality (mutual/one-way), exceptions, return/destruction, governing law & jurisdiction, termination/survival variants.
2. Parameter Ranges: default term (3 years), allowed jurisdictions/venues.
3. Playbook: when to allow one-way vs mutual; default exceptions; mandatory references (CRA 2015, UCTA 1977 if relevant).
4. Validation: 10–20 NDA examples (intake + approved draft).
