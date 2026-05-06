# UMDO — Universal Media Delivery Ontology

UMDO is an open, machine-readable schema for broadcast, OTT, cinema, and ad-delivery technical specifications.

It lets a broadcaster, studio, post house, or QC vendor encode "what does this delivery need to look like" — codecs, containers, color, loudness, timecode, packaging, compliance — as a single JSON document that any tool can read.

**Schema version:** `0.7.0`
**Spec licence:** [CC BY 4.0](LICENSE) · **Tooling licence:** [MIT](LICENSE-MIT)

---

## Why UMDO

Every broadcaster and platform writes its own delivery spec — usually as a PDF. Vendors and post houses re-implement the same fields by hand for every spec they support, and updates are easy to miss.

UMDO replaces the PDF with structured data:

- **One shape** — every spec describes codecs, audio loudness, timecode, etc. using the same field names.
- **Validatable** — drop a profile in a CI job and reject typos.
- **Diffable** — version-control your spec; reviewers can read a PR diff.
- **Tool-agnostic** — UMDO is just JSON; any QC tool, ingest pipeline, or asset manager can consume it.

UMDO is intentionally vendor-neutral. It is governed in the open and welcomes contributions from broadcasters, studios, distributors, post houses, and QC vendors.

---

## Repository layout

```
schema/umdo.schema.json   JSON Schema (Draft 2020-12) — the contract
schema/enums/             Recommended controlled vocabularies (codecs, containers, colour)
profiles/                 Real-world delivery specs encoded as UMDO documents
examples/                 Minimal annotated examples
docs/                     Field reference and contribution guidance
tools/validate.py         CLI validator — runs in CI on every PR
```

---

## Quickstart

```bash
# Install validator dependencies
pip install jsonschema

# Validate every profile in this repo
python tools/validate.py

# Validate a single profile
python tools/validate.py profiles/svt_hd.json
```

To use UMDO in your own pipeline, fetch `schema/umdo.schema.json` and validate against it with any Draft 2020-12 JSON Schema library — Python (`jsonschema`), Node (`ajv`), Go (`gojsonschema`), Java (`networknt/json-schema-validator`), etc.

---

## Profile naming convention

```
brand_subbrand_region_format.json
```

| Component  | Required | Example                             |
|------------|----------|-------------------------------------|
| `brand`    | Yes      | `svt`, `rte`, `tg4`                 |
| `subbrand` | No       | `iplayer`, `commercials`            |
| `region`   | No       | `se`, `ie`, `uk`                    |
| `format`   | Yes      | `hd`, `sd`, `uhd`, `mez`            |

The filename (without `.json`) is the spec's stable `id` and matches the document's `id` field.

---

## Contributing

UMDO grows by **forks and pull requests**. The most useful contributions are:

1. **A new profile** — encode a public delivery spec your organisation supports. Open a PR with the new file under `profiles/`.
2. **A schema field** — propose a new field with rationale: which spec needs it, what it represents, why existing fields can't carry it.
3. **A correction** — fix an inaccuracy in an existing profile against its source document. Cite the source.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full workflow and [GOVERNANCE.md](GOVERNANCE.md) for how schema changes are accepted and versioned.

CI runs `tools/validate.py` on every PR. Profiles must validate against the published schema before merge.

---

## Versioning

UMDO follows semantic versioning of the schema document. The current version is **0.7.0**.

- **Patch** — clarifications, doc fixes, new profiles. Always backwards-compatible.
- **Minor** — new optional fields, new enum values, relaxed constraints.
- **Major** — breaking changes (renamed/removed fields, tightened types). Major versions ship with a migration note.

A profile's own `governance.spec_version` is the version of the **delivery spec it represents** (e.g. an internal v5.1 of a broadcaster's PDF) — not the UMDO schema version.

---

## Status

UMDO 0.7.0 covers the fields needed by every public-broadcaster, OTT-mezzanine, and ad-clearance spec we have profiled so far. It is `additionalProperties: true` throughout — vendors can add their own extension fields and propose them upstream as the ecosystem stabilises.

See [docs/gaps.md](docs/gaps.md) for known limitations and the proposal queue.

---

## Licence

The schema and profiles are licensed under [CC BY 4.0](LICENSE) — use them anywhere, attribute UMDO.
The validator and tooling code is licensed under [MIT](LICENSE-MIT).
