# deepbiomat-features

> **P2 of 4.** A `matminer`-compatible featurizer package OR a curated open dataset for biomaterial-relevant prediction tasks. Fills one concrete, narrowly-scoped gap.

**Status:** `v0.0.3` (pre-scaffold; repo renamed to `deepbiomat-features` for trademark hygiene — see [`CHANGELOG.md`](./CHANGELOG.md))
**Repo path:** [`deepbiomat/deepbiomat-features`](https://github.com/deepbiomat/deepbiomat-features) (when created)
**License:** `Apache-2.0` (locked 2026-04-21 — explicit patent grant valuable for novel featurizers; upstream contributions to `matminer`/`pymatgen`/`DeepChem` flow through dedicated forks, not this repo).
**Stack:** Python (latest stable), `uv`, `matminer` + `pymatgen` + `pandas`, distributed via PyPI, archived via Zenodo.

---

## Goal

Pick **one** of the two scopes at v0.1.0:

- **Scope A — Featurizer:** implement a small set of biomaterial-relevant descriptors (e.g., degradation-rate proxies for biopolymers, surface-energy descriptors for implant alloys, scaffold-porosity features) following the `matminer.featurizers.base.BaseFeaturizer` contract. Publish to PyPI.
- **Scope B — Dataset:** curate, normalize, and document a small (≤ 10k rows) open dataset on a biomaterials property of clinical interest, scraped or compiled from published supplementary information with explicit license tracking. Publish to Zenodo with a Croissant or Frictionless Data manifest.

Lock the choice at `v0.1.0` based on what P1 revealed as the most useful gap.

## Why Both Scopes Qualify

Either output is genuinely useful to the field and citable. Both demonstrate the same core competence — translating domain knowledge into a reusable artifact for the open-source ecosystem.

## Non-Goals

- **Not** a kitchen-sink library — one focused contribution, not ten weak ones.
- **Not** a fork of `matminer` — extend via the public extension points; if those don't exist, file an upstream issue first.
- **Not** a benchmark (that's P3).
- **Not** a research paper (also P3).

## Quality Bar (Both Scopes)

- API docs with executable examples (doctest-verified).
- Test coverage ≥ 90% on `src/`.
- Type-checked strict.
- One end-to-end notebook that takes a user from install to first useful output in < 10 minutes.
- Reviewable diff size on initial PyPI release: ideally < 2k LOC.
- Validated against at least one public dataset; results documented.

## Scope A: Featurizer Specifics

- Subclass `matminer.featurizers.base.BaseFeaturizer`.
- Implement `featurize`, `feature_labels`, `citations`, `implementors`.
- Accept `pymatgen.core.Structure` or `Composition` as appropriate.
- Vectorize via `featurize_dataframe` interoperability.
- Cite original method in `citations()` when descriptor is from prior literature.

## Scope B: Dataset Specifics

- Single canonical CSV/Parquet + JSON schema.
- Provenance: every row traceable to a source DOI.
- License audit: only redistributable rows included (CC-BY, CC0, public domain, or licensed permissively in the original publication).
- Croissant manifest (https://github.com/mlcommons/croissant) for ML-readiness.
- One Hugging Face Datasets-compatible loader script.

## Versioning

- `v0.1.0` — scaffold + scope locked
- `v0.2.0` — API/schema designed and frozen
- `v0.3.0` — implementation passes tests
- `v0.4.0` — validated against external dataset/use case
- `v0.5.0` — published to PyPI (Scope A) or Zenodo (Scope B)
- `v1.0.0` — stable API, downstream user (P3 counts) successfully integrates

## Prerequisites (from curriculum repo)

- L2 ≥ `v0.3.0` (biomaterials taxonomy)
- L3 ≥ `v0.4.0` (matminer fluency)

## How to Cite

Will be added at `v0.5.0` with PyPI metadata + Zenodo DOI.
