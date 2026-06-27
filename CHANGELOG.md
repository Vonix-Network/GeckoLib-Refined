# Changelog

All notable changes to GeckoLib-Refined are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and [SemVer](https://semver.org/) where applicable.

Versioning convention: `<upstream-version>+refined.<N>` — e.g. `3.0.57+refined.1` is upstream `3.0.57` plus Refined patch revision 1.

Every entry under `### Backported` includes the upstream commit SHA. Every entry under `### Optimised` is a Refined-original change with rationale.

---

## [1.18.2-3.0.57+refined.1] — 2026-06-27 — MC 1.18.2

First Refined release for the abandoned `1.18` upstream branch. Base: `bernie-g/geckolib@4533673` (upstream `1.18` HEAD).

### Optimised
- **`[OPTIMIZED]` `AnimationController.process`: lazy-init `boneSnapshot` instead of relying on a production-disabled `assert`.** Upstream line 467 was `assert boneSnapshot != null` which is a no-op under any JVM not started with `-ea`. The downstream field reads on lines 480/483/486/495/497/499/507/509/511 then NPE with `Cannot read field "rotationValueX" because "boneSnapshot" is null`. Refined initialises a fresh `BoneSnapshot` from the bone's known-good `getInitialSnapshot()` (already retrieved on the next line) and caches it so subsequent ticks reuse it. Preserves observed behaviour; only changes the null path. Applied to Forge, Fabric, Quilt. Fixes the dragon-render crash reported on Isle of Berk (`Skrill`, `Monstrous Nightmare`) and the broader bug class affecting any geckolib3 entity whose first render races bone snapshot population.

  *Cross-version note:* Upstream's GeckoLib4 rewrite (1.20.1+) already addresses this bug class with a `continue` short-circuit at `AnimationController.process:506`. Their approach skips the bone for one frame; ours lazy-inits the snapshot so the transition still renders. Both are valid; the lazy-init keeps animation continuity intact.

### Backported
- **`a01f17b` — `Fix math.pi evaluating to 0` (by @Tslat).** Pulled from `bernie-g/geckolib@1.20.1`. `MolangParser` was remapping `pi` → `math.pi` but never registering `math.pi` as a `Variable`, so animations using `math.pi` evaluated it to 0 at runtime (subtle: easy to miss in dev, animations look "almost right"). Applied to Forge, Fabric, Quilt. Rationale: the variable registration shape is identical between geckolib3 and geckolib4 `MolangParser`.

### Build / CI
- GitHub Actions matrix build added for Forge / Fabric / Quilt on PR; release-on-tag publishes artifacts.
- `META-INF/mods.toml`: Refined attribution added (`displayName` → "GeckoLib (Refined)", `credits`/`authors` extended with Refined fork maintainer, `issueTrackerURL` → Refined repo, `displayURL` → Refined repo).

---

## [Unreleased]

### In flight
- Deeper backport sweep from `bernie-g/geckolib@1.20.1` and `1.21.1` (target: `refined.2`)
- Industry-grade polish pass: executor lifecycle audit, SLF4J marker, concurrent-map review

---

<!--
Template for future releases:

## [3.0.57+refined.N] - YYYY-MM-DD — MC 1.18.2

### Fixed
- One-line fix description, referencing #issue or crash signature.

### Backported
- `<upstream-sha>` — `<upstream-subject>` (by @upstream-author). Pulled from `bernie-g/geckolib@1.20.1`. Rationale: applies cleanly to geckolib3 because [reason].

### Optimised
- `[OPTIMIZED]` <subject>. Rationale: [why this is safe / measured impact / what it replaces].

### Build / CI
- Tooling changes that don't ship in the jar.
-->
