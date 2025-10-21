# Repository Guidelines

## Project Structure & Module Organization
SkillAegis orchestrates three pieces. Root `app.py` serves the welcome screen and proxies to the active URLs defined in `config.json` (copy from `config.json.sample`). Scenario JSON lives under `scenarios/` and must respect the CEXF schema. `SkillAegis-Editor/` and `SkillAegis-Dashboard/` are git submodules; each keeps its Vue client in `src/`, and the Dashboard includes a Python backend in `backend/`.

## Build, Test, and Development Commands
After cloning, run `git submodule update --init --recursive`. Create virtualenvs per component; the Dashboard backend expects `python3 -m venv venv && pip install -r backend/requirements.txt`. Both front-ends require `npm install`. `bash SkillAegis.sh --scenario_folder ./scenarios` boots the trio via GNU screen, while `npm run dev` inside either submodule and `python3 app.py --dashboard_url ... --editor_url ...` support targeted development.

## Coding Style & Naming Conventions
Follow PEP 8 in Python code: 4-space indents, snake_case helpers, and explicit imports. Keep defaults in the `*.sample` files and load via config objects rather than literals. Vue files stay in PascalCase under `src/`; composables and stores should be camelCase exports. Enforce ESLint and Tailwind conventions with `npm run lint`, and format staged Vue/JS changes with `npm run format` before committing.

## Testing Guidelines
Automated coverage is light, so treat linting and builds as required gates: run `npm run lint` and `npm run build` in both submodules, and rerun `pip install -r backend/requirements.txt && python -m compileall backend` when the Dashboard server changes. Validate scenario files with `python -m jsonschema --schema SkillAegis-Editor/schema_cexf.json --instance scenarios/<file>.json` (install `jsonschema` in your active venv first). Complete a smoke round via `bash SkillAegis.sh` using a sample exercise.

## Commit & Pull Request Guidelines
Commits in this repo prefer short prefixes (`chg:`, `add:`, `fix:`) plus a scoped tag in brackets when helpful, e.g. `chg: [doc] clarify setup`. Mirror that style and keep subjects under 72 characters with optional bullet bodies. Pull requests should summarise impact, flag which apps changed, link issues, and include screenshots or CLI output for UI/API work. Highlight config or schema adjustments so reviewers can retest locally.
