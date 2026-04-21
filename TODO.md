# TODO: deepbiomat-features

> Atomic tasks. Open this repo only after curriculum L2 ≥ v0.3.0 and L3 ≥ v0.4.0, and ideally after P1 has surfaced a concrete gap to fill.

**Cursor:** `v0.0.3` (gated; repo renamed to `deepbiomat-features` for trademark hygiene — see [`CHANGELOG.md`](./CHANGELOG.md))

---

## v0.1.0 — Scaffold + Scope Locked

- [ ] **0.1** Add LICENSE *(locked in v0.0.1: `Apache-2.0`)*
  - [ ] 0.1.1 Add `LICENSE` file with canonical Apache-2.0 text
  - [ ] 0.1.2 Add `NOTICE` file (Apache-2.0 requires `NOTICE` when attribution to preserve exists; start empty, grow as contributors/derivations accrue)
  - [ ] 0.1.3 Add SPDX header `# SPDX-License-Identifier: Apache-2.0` to source files
  - [ ] 0.1.4 Add `CONTRIBUTING.md` note: upstream contributions to `matminer`/`pymatgen`/`DeepChem` must be made directly to those projects under their respective licenses via personal fork; do not mirror through this repo
- [ ] **0.2** Decide scope (A or B)
  - [ ] 0.2.1 Review P1 outputs and `docs/gap-analysis.md` from P1
  - [ ] 0.2.2 Survey what `matminer` already covers in the candidate area
  - [ ] 0.2.3 Score Scope A vs Scope B against: gap size, effort, downstream usefulness, your moat
  - [ ] 0.2.4 Lock decision in `docs/scope-decision.md`
- [ ] **0.3** Repo scaffold per `ARCHITECTURE.md` (shared foundation only)
  - [ ] 0.3.1 `pyproject.toml` (`uv` + `hatchling`)
  - [ ] 0.3.2 `Makefile` with `setup`, `lint`, `test`, `docs`, `build`, `publish`
  - [ ] 0.3.3 `.pre-commit-config.yaml`
  - [ ] 0.3.4 `.gitignore`, `.editorconfig`
- [ ] **0.4** CI baseline
  - [ ] 0.4.1 `ci.yml` — lint + type + test + doctest
  - [ ] 0.4.2 `docs.yml` — build mkdocs site
  - [ ] 0.4.3 Coverage gate ≥ 90%
- [ ] **0.5** Security baseline
  - [ ] 0.5.1 Dependabot config
  - [ ] 0.5.2 `pip-audit` in CI
  - [ ] 0.5.3 `CODEOWNERS`, `SECURITY.md`
- [ ] **0.6** Release plumbing (skeleton, not active yet)
  - [ ] 0.6.1 Scope A: PyPI Trusted Publishing config
  - [ ] 0.6.2 Scope B: Zenodo GitHub integration enabled
- [ ] **0.7** Tag `v0.1.0`

## v0.2.0 — API / Schema Frozen

### If Scope A (Featurizer)

- [ ] **1A.1** Public API design
  - [ ] 1A.1.1 List target featurizers (≤ 5; small is good)
  - [ ] 1A.1.2 Define `feature_labels` for each (units in brackets)
  - [ ] 1A.1.3 Define accepted input types (`Composition`, `Structure`, both)
  - [ ] 1A.1.4 Document API in `docs/api/`
- [ ] **1A.2** Stub all featurizer classes with NotImplementedError
- [ ] **1A.3** Tests pinning the API surface (signatures, types) — currently failing
- [ ] **1A.4** Tag `v0.2.0`

### If Scope B (Dataset)

- [ ] **1B.1** Schema design
  - [ ] 1B.1.1 List columns + types + units
  - [ ] 1B.1.2 `Pandera` schema in `src/deepbiomat_features/schema.py`
  - [ ] 1B.1.3 Provenance schema (source_doi, license, dates)
- [ ] **1B.2** Source identification
  - [ ] 1B.2.1 List candidate sources with DOIs and licenses
  - [ ] 1B.2.2 Reject sources without redistributable license
- [ ] **1B.3** Empty manifest skeletons (Croissant + Frictionless)
- [ ] **1B.4** Tag `v0.2.0`

## v0.3.0 — Implementation Passes Tests

### If Scope A

- [ ] **2A.1** Implement each featurizer
  - [ ] 2A.1.1 `featurize` correct on hand-computed examples
  - [ ] 2A.1.2 `feature_labels`, `citations`, `implementors` complete
  - [ ] 2A.1.3 Vectorized via `featurize_dataframe`
- [ ] **2A.2** Tests
  - [ ] 2A.2.1 Unit test per featurizer with ≥ 3 hand-verified inputs
  - [ ] 2A.2.2 Property tests (Hypothesis) for invariants
  - [ ] 2A.2.3 Integration test against a small public dataset
  - [ ] 2A.2.4 Doctest examples that compile + run
- [ ] **2A.3** Docs site builds, all examples pass
- [ ] **2A.4** Tag `v0.3.0`

### If Scope B

- [ ] **2B.1** Extraction pipeline (`scripts/01_extract.py`)
- [ ] **2B.2** Normalization (`scripts/02_normalize.py`) — units, dedup
- [ ] **2B.3** Validation (`scripts/03_validate.py`) — schema passes
- [ ] **2B.4** License audit (`scripts/04_license_audit.py`)
- [ ] **2B.5** Manifest build (`scripts/05_build_manifest.py`)
- [ ] **2B.6** Loader API: `deepbiomat_features.load() -> pd.DataFrame`
- [ ] **2B.7** Tests on schema + provenance completeness
- [ ] **2B.8** Tag `v0.3.0`

## v0.4.0 — External Validation

- [ ] **3.1** End-to-end notebook `examples/quickstart.ipynb`
  - [ ] 3.1.1 Install → first useful output in < 10 min
  - [ ] 3.1.2 Notebook runs in CI via `nbmake` or `papermill`
- [ ] **3.2** Validation
  - [ ] 3.2.1 Scope A: train a baseline model using your features on a public task; report metric
  - [ ] 3.2.2 Scope B: train a baseline on your dataset; report metric + comparison to source publications
- [ ] **3.3** External user test
  - [ ] 3.3.1 Have one other person (or your future-self in a clean env) follow the quickstart
  - [ ] 3.3.2 Log every friction point; fix or document
- [ ] **3.4** Tag `v0.4.0`

## v0.5.0 — Published

- [ ] **4.1** Pre-publish hygiene
  - [ ] 4.1.1 `CHANGELOG.md` finalized
  - [ ] 4.1.2 Version bumped via `pyproject.toml`
  - [ ] 4.1.3 README install instructions verified on fresh env
- [ ] **4.2** Publish
  - [ ] 4.2.1 Scope A: TestPyPI dry-run → PyPI release via Trusted Publishing
  - [ ] 4.2.2 Scope B: GitHub Release → Zenodo DOI minted
- [ ] **4.3** Discoverability
  - [ ] 4.3.1 Add concept DOI badge to README
  - [ ] 4.3.2 Add to `awesome-materials-informatics` (PR upstream)
  - [ ] 4.3.3 Open a discussion in matminer to announce (Scope A)
- [ ] **4.4** Tag `v0.5.0`

## v1.0.0 — Stable + Used Downstream

- [ ] **5.1** P3 (`deepbiomat-bench`) integrates this package and ships
- [ ] **5.2** API freeze: any breaking change requires `v2.0.0`
- [ ] **5.3** Final docs polish
  - [ ] 5.3.1 Architecture decision records in `docs/adr/`
  - [ ] 5.3.2 Tutorial covering all primary use cases
- [ ] **5.4** Tag `v1.0.0`

---

## Backlog (Unscheduled)

- [ ] Scope A: contribute the most-downloaded featurizer back to `matminer` core if maintainers are interested
- [ ] Scope B: dataset card on Hugging Face Hub
- [ ] Both: blog post / writeup linking to project

## Open Questions

- _populated during scope decision_
