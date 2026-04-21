# Architecture: deepbiomat-features

> Two architectural shapes depending on scope decision at v0.1.0. Both share the foundation; they diverge at the package layer.

## 1. Shared Foundation

```
deepbiomat-features/
├── README.md
├── ARCHITECTURE.md
├── TODO.md
├── LICENSE                  ← Apache-2.0
├── CHANGELOG.md
├── pyproject.toml           ← uv + hatchling, latest stable Python
├── uv.lock
├── .python-version
├── Makefile
├── .pre-commit-config.yaml
├── .github/
│   └── workflows/
│       ├── ci.yml
│       ├── release.yml      ← PyPI (A) or Zenodo (B) on tag
│       └── docs.yml
├── docs/
│   ├── scope-decision.md    ← A vs B, rationale
│   ├── api/                 ← mkdocs material
│   └── examples/
├── src/
│   └── deepbiomat_features/
│       └── __init__.py      ← __version__, public API
├── tests/
└── examples/
    └── quickstart.ipynb
```

## 2. Scope A — Featurizer Package

```
src/deepbiomat_features/
├── __init__.py
├── featurizers/
│   ├── __init__.py
│   ├── base.py              ← thin extension of matminer.BaseFeaturizer
│   ├── degradation.py       ← e.g., hydrolytic-rate proxies
│   ├── surface.py           ← e.g., surface-energy descriptors
│   └── porosity.py          ← e.g., scaffold morphology features
├── data/
│   └── lookup_tables/       ← packaged reference values, CSV
└── _typing.py               ← Pydantic models for config
```

### Public API contract

```python
from deepbiomat_features.featurizers import DegradationProxyFeaturizer

f = DegradationProxyFeaturizer()
df = f.featurize_dataframe(df, col_id="composition")
```

### Featurizer rules

- Inherit `matminer.featurizers.base.BaseFeaturizer`.
- Implement: `featurize`, `feature_labels`, `citations`, `implementors`.
- `feature_labels` returns stable, snake_case names with units in brackets.
- `citations` returns BibTeX entries for every prior-work descriptor used.
- No silent NaN — return explicit sentinel + log warning, or raise.
- Pure functions where possible; no global state.

## 3. Scope B — Dataset Package

```
src/deepbiomat_features/
├── __init__.py
├── loader.py                ← single load() returning canonical DataFrame
├── schema.py                ← Pandera or Pydantic schema
├── manifest/
│   ├── croissant.json       ← MLCommons Croissant
│   └── frictionless.json    ← Frictionless Data
└── _provenance.py           ← per-row source DOI + license
data/
├── biomat_dataset.parquet   ← canonical artifact
├── biomat_dataset.csv       ← human-readable mirror
└── sources.csv              ← source DOIs + licenses
```

### Curation pipeline (`scripts/`)

```
scripts/
├── 01_extract.py            ← scrape/parse from sources
├── 02_normalize.py          ← unit conversions, deduplication
├── 03_validate.py           ← Pandera schema check
├── 04_license_audit.py      ← drop rows without redistributable license
└── 05_build_manifest.py     ← generate Croissant + Frictionless
```

### Per-row provenance

Every row carries: `source_doi`, `source_license`, `extraction_date`, `extractor_version`. Rows without all four are rejected.

## 4. Quality Gates (Both Scopes)

| Stage | Tooling |
|---|---|
| Format | `ruff format` |
| Lint | `ruff check` |
| Type | `pyright` strict on `src/` |
| Test | `pytest` + `pytest-cov` ≥ 90% |
| Doctest | `pytest --doctest-modules` |
| Schema (B) | `pandera` validators in CI |
| Build | `hatchling` |
| Security | `pip-audit` weekly |
| Docs | `mkdocs-material` + `mkdocstrings` |

## 5. Release Engineering

### Scope A — PyPI

- Trusted Publishing via OIDC (no PyPI tokens in repo).
- TestPyPI dry-run on `vX.Y.Z-rc.N` tags.
- Production publish on `vX.Y.Z` tags.
- `__version__` single-sourced from `pyproject.toml` via `importlib.metadata`.

### Scope B — Zenodo

- GitHub-Zenodo integration enabled.
- Release on tag → automatic DOI mint.
- DOI for each version + concept DOI for the dataset as a whole.
- README badge with concept DOI.

## 6. Dependency Policy

- `matminer`, `pymatgen`, `pandas` as primary deps (Scope A).
- `pandera`/`pydantic`, `pyarrow`, `requests` (Scope B).
- All deps at latest stable on initial release.
- Renovate auto-merges patch versions; minor/major are manual.
- No deps with restrictive licenses (GPL family) without explicit decision recorded.

## 7. Anti-Patterns to Refuse

- **Reimplementing matminer descriptors** instead of contributing upstream.
- **Untyped `**kwargs`** at public API boundaries.
- **Hidden network calls** at import time.
- **License laundering** (Scope B) — original license must permit redistribution.
- **Framework smell** — if the package needs a 50-line tutorial to use, the API is wrong.
