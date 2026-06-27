# Changelog

All notable changes to GeckoLib-Refined are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and [SemVer](https://semver.org/) where applicable.

Versioning convention: `<upstream-version>+refined.<N>` — e.g. `3.1.40+refined.1` is upstream `3.1.40` plus Refined patch revision 1.

Every entry under `### Backported` includes the upstream commit SHA. Every entry under `### Optimised` is a Refined-original change with rationale.

---

## [1.19.2-3.1.40+refined.1] — 2026-06-27 — MC 1.19.2

First Refined release for the abandoned `1.19` upstream branch. Base: `bernie-g/geckolib@6b17247`. Forge `3.1.40`, Fabric `3.1.40`, Quilt `3.1.41`.

### Optimised
- **`[OPTIMIZED]` `AnimationController.process`: lazy-init `boneSnapshot` instead of relying on a production-disabled `assert`.** Upstream's `assert boneSnapshot != null` (line 467) is a no-op under any JVM not started with `-ea`. The downstream field reads NPE with `Cannot read field "rotationValueX" because "boneSnapshot" is null`. Refined initialises a fresh `BoneSnapshot` from the bone's `getInitialSnapshot()` and caches it in `this.boneSnapshots` so subsequent ticks reuse it. Preserves observed behaviour; only changes the null path. Applied to Forge, Fabric, Quilt. Same bug class as 1.18 — affects any geckolib3 entity whose first render races bone snapshot population.

### Backported
- **`a01f17b` — `Fix math.pi evaluating to 0` (by @Tslat).** Pulled from `bernie-g/geckolib@1.20.1`. `MolangParser` was remapping `pi` → `math.pi` but never registering it as a `Variable`, so animations using `math.pi` evaluated to 0 at runtime. Applied to Forge, Fabric, Quilt. Rationale: `MolangParser` registration shape identical between 1.19 (geckolib3) and 1.20.1 (geckolib4).
- **`a05782f` — `Hopefully fix crash with setting an animation forcefully before it transitions` (by @Tslat).** Pulled from `bernie-g/geckolib@1.20.1`. Forced animation switch mid-transition can leave `currentAnimation` null while the state machine thinks it's transitioning, crashing on the next tick. Re-asserts `AnimationState.Transitioning` if `currentAnimation == null` after the just-started-transition block. Applied to Forge, Fabric, Quilt at `AnimationController.process` line 419. Rationale: state machine is byte-identical between 1.19 (geckolib3) and 1.20.1 (geckolib4 base).

### Build / CI
- GitHub Actions matrix build added for Forge / Fabric / Quilt on PR; release-on-tag publishes artifacts.

---

## [Unreleased]

### In flight
- Deeper backport sweep from `bernie-g/geckolib@1.20.1` and `1.21.1` (target: `refined.2`)
- Industry-grade polish pass: executor lifecycle audit, SLF4J marker, concurrent-map review
