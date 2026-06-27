# Changelog

All notable changes to GeckoLib-Refined are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and [SemVer](https://semver.org/) where applicable.

Versioning convention: `<upstream-version>+refined.<N>` — e.g. `3.0.57+refined.1` is upstream `3.0.57` plus Refined patch revision 1.

Every entry under `### Backported` includes the upstream commit SHA. Every entry under `### Optimised` is a Refined-original change with rationale.

---

## [Unreleased]

### Branches in flight
- `1.18` — initial Refined cut against upstream `bernie-g/geckolib@1.18` HEAD (`4533673`)
- `1.19` — initial Refined cut against upstream `bernie-g/geckolib@1.19` HEAD (`6b17247`)

---

<!--
Template for future releases:

## [3.0.57+refined.1] - YYYY-MM-DD — MC 1.18.2 Forge

### Fixed
- One-line fix description, referencing #issue or crash signature.

### Backported
- `<upstream-sha>` — `<upstream-subject>` (by @upstream-author). Pulled from `bernie-g/geckolib@1.20.1`. Rationale: applies cleanly to geckolib3 because [reason].

### Optimised
- `[OPTIMIZED]` <subject>. Rationale: [why this is safe / measured impact / what it replaces].

### Build / CI
- Tooling changes that don't ship in the jar.
-->
