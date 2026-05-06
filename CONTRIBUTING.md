# Contributing to UMDO

Thanks for your interest. UMDO grows by community contribution — broadcasters, studios, distributors, post houses, and QC vendors all have specs worth encoding and gaps worth filling.

## Three kinds of contribution

### 1. Add a profile

The easiest way to help. If your organisation publishes a delivery spec, encode it as a UMDO profile.

1. Fork this repo.
2. Create `profiles/<brand>_<subbrand>_<region>_<format>.json`. See the [naming convention](README.md#profile-naming-convention).
3. Encode against the schema. Use existing profiles in `profiles/` as templates.
4. Cite the source spec (PDF version, effective date, URL) in `governance.source`.
5. Run `python tools/validate.py profiles/your_profile.json` and confirm it passes.
6. Open a PR. CI will re-validate.

The profile must encode a **publicly available** spec — UMDO does not host confidential or NDA-only specs.

### 2. Propose a schema field

If your spec needs a field UMDO does not have:

1. Open an issue using the **Schema change proposal** template before opening a PR.
2. Explain: which delivery spec needs it, what concept it captures, why existing fields can't represent it, what type/shape you propose.
3. After discussion, open a PR that updates `schema/umdo.schema.json`, adds an example to `docs/`, and bumps `CHANGELOG.md`.

### 3. Correct an existing profile

Profiles drift as broadcasters publish updates. If you find an inaccuracy:

1. Open an issue or PR with the diff.
2. Cite the authoritative source document and section.
3. Update `governance.spec_version` and `governance.effective_date` if the change reflects a new published version of the spec.

## Pull request rules

- Every PR runs `tools/validate.py` in CI. PRs that fail validation cannot be merged.
- Every schema change must include a `CHANGELOG.md` entry and a worked example in `docs/` or `examples/`.
- Every new profile must include `governance.source` and `governance.spec_version`.
- Keep PRs small. One new profile per PR. One schema field per PR.

## Style

- JSON files: 2-space indent, sorted keys are not required but preferred.
- Field names: snake_case.
- Comments live in `notes` fields inside the JSON, not in code comments. JSON does not support comments.
- Use `null` (not `0` or `-1`) to mean "not enforced" for numeric fields.

## Review

Schema changes are reviewed by the maintainers per [GOVERNANCE.md](GOVERNANCE.md). Profile additions are typically merged within a few days if CI passes and the source is cited.
