# Changelog

All notable changes to the UMDO schema are recorded here. The format follows [Keep a Changelog](https://keepachangelog.com/) and the project adheres to [Semantic Versioning](https://semver.org/).

## [0.7.1] — Provenance fields

### Added

- `governance.sourceSpec` — direct URL to the source delivery specification the profile was encoded from. Optional in 0.7.1; intended to become required at 1.0 once the back-catalogue is populated.
- `governance.sourceAccess` — access category of `sourceSpec`. Enum: `public` (openly published), `portal-login` (behind a free login), `contracted` (available only under a production agreement), `unknown` (legacy profile encoded before this rule existed). Optional in 0.7.1.
- Example profiles updated with the new fields where known: `clearcast_commercials` (`portal-login`), `rte_hd` (`contracted`), `rtl_smallitems_hd` (`public` + URL).

### Notes

- Backwards compatible — existing profiles without the new fields continue to validate.
- See the UMDO legal & contribution policy for the rationale behind the new fields.

## [0.7.0] — Initial public release

First public release of UMDO as a standalone project.

### Added

- `assets.video.gop` — GOP structure constraints (`closed_required`, `max_length`). Promoted from a v7 schema gap to a first-class field.
- `assets.video.timecode` — Embedded timecode track requirements (`ltc_required`, `vitc_required`). Promoted from a v7 schema gap to a first-class field.
- `structure.timeline.timecode_start` — Required start timecode (e.g. `10:00:00:00`). Was present in the v7 type stub but documented as missing from most encoded specs; now first-class with a SMPTE timecode pattern.
- Draft 2020-12 JSON Schema replacing the v7 type-stub document.
- `tools/validate.py` — CLI validator for profiles.
- CI workflow validating every profile on every PR.
- Three seed profiles: `svt_hd` (Sweden), `rte_hd` (Ireland), `tg4_hd` (Ireland).

### Changed

- Numeric "constraint" fields where `null` means "not enforced" (`loudness.true_peak_max`, `safe_area.*_percent`, `preclearance.lead_time_days`, signal-limit min/max, loudness target/tolerance) now formally accept `null` in addition to a number.
- `delivery_paradigm` is an open-string field with documented examples rather than a closed enum, to admit `imf` and `dcp` without breaking existing profiles.

### Notes

- Every UMDO object is `additionalProperties: true`. Vendor extensions are valid; widely-used extensions are candidates for promotion in 0.8.x.
