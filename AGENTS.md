# Repository Guidelines

## Project Structure & Module Organization
- Current: `README.md` (overview) and `project-spec.md` (PRD/requirements).
- Proposed layout as code lands:
  - `src/` backend/orchestrator code (e.g., planner, composer, verifier, exporter)
  - `corpus/` JSONL clause library (versioned)
  - `recipes/` YAML assembly recipes
  - `addons/word/` Office.js add‑in
  - `tests/` unit/integration tests
  - `scripts/` maintenance/validation utilities
  - `docs/` additional design notes

Example:
```
corpus/ nda/ ... .jsonl
recipes/ nda.yaml  msa.yaml
src/ composer/ verifier/ exporter/
```

## Build, Test, and Development Commands
- Docs preview: open `project-spec.md` in your editor or viewer.
- Python (if backend present): `python -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt`
- Run tests (Python): `pytest -q`
- Node (Word add‑in): `npm install && npm run dev` (local), `npm test`
- Optional Make targets (if `Makefile` exists): `make lint`, `make test`, `make validate-corpus`.

## Coding Style & Naming Conventions
- Python: Black (line length 100), Ruff, isort. snake_case functions, PascalCase classes, module names snake_case.
- JS/TS (add‑in): Prettier + ESLint. camelCase variables, PascalCase components.
- JSON/YAML: 2‑space indent. Filenames: kebab-case. Recipes end with `.yaml`; corpus files end with `.jsonl`.

## Testing Guidelines
- Frameworks: pytest for Python; Jest/Vitest for JS/TS.
- Locations: `tests/` mirrors `src/` package structure.
- Names: Python `test_*.py`; JS `*.test.ts` or `*.spec.ts`.
- Coverage: target ≥80%. Commands: `pytest --maxfail=1 -q`, `npm test -- --coverage`.

## Commit & Pull Request Guidelines
- Conventional Commits: `feat:`, `fix:`, `docs:`, `test:`, `chore:`, `refactor:`.
- Scope by area: `feat(composer): support supplier_friendly variant`.
- PRs: clear description, linked issue, rationale, before/after (screenshots for add‑in), tests added/updated, and notes on data residency/privacy where relevant.

## Security & Configuration
- Respect PRD constraints: UK/EU data residency; no external data exfiltration.
- Never commit secrets. Use `.env` (gitignored) and provide `.env.example`.
- Validate corpus/recipes on CI: schema checks, disallow missing parameters and broken cross‑refs.

