# Meridian — Study Abroad Intelligence Platform

Meridian is a full-stack study-abroad decision platform that turns a student's
academic profile, goals, experience, documents, budget, and preferences into a
structured application dossier.

It combines a cinematic Reflex interface with a FastAPI analysis service and
seven connected specialist agents. The platform is designed around transparent
results: model-backed scores are labelled, rule-based recommendations are
identified, unavailable artifacts are reported explicitly, and no admission
probability is fabricated when a verified model export is missing.

## Why Meridian stands out

- **Multi-agent analysis pipeline** covering profile, universities, programs,
  finances, scholarships, careers, and extracurricular strength.
- **End-to-end dossier workflow** from registration and profile completion to
  document preview, analysis, saved results, and PDF reporting.
- **Evidence-first model boundaries** with artifact manifests, checksum
  validation, pinned worker environments, and explicit provenance notices.
- **Production-minded local architecture** with separate Reflex and FastAPI
  environments, SQLite-backed persistence, password hashing, session
  revocation, throttling, and protected analysis events.
- **Responsive editorial UI** featuring the Meridian dossier, accessible
  controls, keyboard focus states, reduced-motion support, and a visual
  landing experience.

## Agent architecture

| Agent | Responsibility | Output |
| --- | --- | --- |
| 1. Profile | Candidate-tier classification | Tier and confidence |
| 2. University | Verified catalog matching | Shortlist, fit scores, evidence |
| 3. Program | Program relevance ranking | Program fit and ranking components |
| 4. Financial | Tuition and living-cost analysis | Annual costs and data quality |
| 5. Scholarship | Retrieval and eligibility review | Awards, eligibility, review reasons |
| 6. Career | Career alignment | Career paths, skill gaps, earnings estimate |
| 7. Extracurricular | Structured achievement scoring | Strength tier, score, archetype |

Agent 2 is built from the included local training data through the controlled
`scripts/run_agent2_notebook.py` workflow. Agent 7 runs through its pinned
`.venv-agent7` inference environment. The original MS/MBA outcome exports are
not included; Meridian returns an explicit unavailable response rather than
substituting an incompatible model.

## Technology

- **Frontend:** Python, Reflex, React Router, responsive CSS
- **Backend:** FastAPI, Uvicorn, Pydantic-compatible JSON boundaries
- **Persistence:** SQLite with hashed credentials and opaque sessions
- **ML/runtime:** scikit-learn, XGBoost, SHAP, pandas, joblib
- **Documents/reports:** TXT, DOCX, PDF preview and ReportLab PDF output
- **Validation:** pytest, backend contract tests, integration tests, smoke flow

## Run locally on Windows

Use Python 3.13+, Node.js 22.22+, and PowerShell.

```powershell
cd Major_Project-2027-main

py -3.13 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt

py -3.13 -m venv .venv-backend
.\.venv-backend\Scripts\python.exe -m pip install -r backend\requirements-test.txt

py -3.13 -m venv .venv-agent7
.\.venv-agent7\Scripts\python.exe -m pip install -r backend\app\agents\extracurricular_agent\requirements.txt
```

Start the services in separate PowerShell terminals:

```powershell
.\scripts\start_meridian.ps1 -Service backend
.\scripts\start_meridian.ps1 -Service frontend
```

Open **http://localhost:3000**. The API documentation is available at
**http://127.0.0.1:8001/docs**.

## Verification

The current integration flow has been verified with:

```powershell
.\.venv-backend\Scripts\python.exe -m pytest backend\tests -q
.\.venv\Scripts\python.exe -m pytest tests\test_meridian_integration.py -q
.\.venv\Scripts\python.exe scripts\smoke_integration.py
```

The smoke flow covers real API calls, all seven agent stages, persistence,
report generation, and logout.

## Project structure

```text
backend/       FastAPI app, agent routes, models, and workers
meridian/      Reflex state, pages, services, and UI components
data/raw/      Versioned source datasets used by the agents
assets/        Brand, globe, scene, and visual assets
docs/          Architecture, integration, artifact, and verification notes
scripts/       Startup, model-bundle, audit, and smoke-test utilities
tests/         Frontend integration and agent tests
```

## Responsible-use notes

Meridian is a decision-support prototype, not an admissions guarantee.
Relevance scores are not admission probabilities. Agent 7 uses a synthetic
rubric and is not validated against admissions outcomes. Catalog tuition,
program requirements, scholarship eligibility, and career estimates should be
verified with official sources before making decisions.

## License

This project is released under the MIT License. See [LICENSE](LICENSE).
