# GeckoLib-Refined

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Minecraft 1.18.2](https://img.shields.io/badge/MC-1.18.2-green)](https://github.com/Vonix-Network/GeckoLib-Refined/tree/1.18)
[![Minecraft 1.19.2](https://img.shields.io/badge/MC-1.19.2-green)](https://github.com/Vonix-Network/GeckoLib-Refined/tree/1.19)
[![Upstream: bernie-g/geckolib](https://img.shields.io/badge/upstream-bernie--g%2Fgeckolib-blue)](https://github.com/bernie-g/geckolib)

> Community-maintained, drop-in compatible fork of [GeckoLib](https://github.com/bernie-g/geckolib) for the **1.18.2** and **1.19.2** branches that upstream stopped updating in October 2024.

GeckoLib-Refined is a maintenance fork. Upstream is alive and well on 1.20.1 / 1.21.1 — go there if you're on a current Minecraft version. This project exists because hundreds of older modpacks (Isle of Berk, Alex's Mobs, Citadel-based mods, dozens more) are stuck on `geckolib3` and have nowhere to file a crash report when GeckoLib itself NPEs.

## What "Refined" means

1. **Drop-in compatible.** Same `software.bernie.geckolib3.*` package, same `modid = geckolib3`, same public class shapes. Existing compiled mods work without recompilation. We do not break the API.
2. **Backports, not rewrites.** Bug fixes from upstream's actively-maintained 1.20.1 / 1.21.1 branches are carried back when they apply to the `geckolib3` architecture. Every backport carries upstream attribution (commit SHA + author) in `BACKPORT_LOG.md`.
3. **Honest optimisation.** Where we go beyond upstream — concurrency hardening, executor lifecycle, logging hygiene — it is labelled **`[OPTIMIZED]`** in the changelog and explained. We do not silently rewrite hot paths and call it a backport.
4. **No version-bump games.** We append `+refined.N` to the upstream version so you can always tell what you're running: `geckolib-forge-1.18-3.0.57+refined.1.jar` is upstream `3.0.57` plus Refined patch revision 1.

## Version matrix

| MC Version | Upstream base | Refined branch | Status | Latest jar |
|---|---|---|---|---|
| 1.18.2 | `3.0.57` (Forge) / `3.0.80` (Fabric) / `3.0.45` (Quilt) | [`1.18`](https://github.com/Vonix-Network/GeckoLib-Refined/tree/1.18) | active | _see [Releases](https://github.com/Vonix-Network/GeckoLib-Refined/releases)_ |
| 1.19.2 | TBD (matches upstream `1.19` branch HEAD) | [`1.19`](https://github.com/Vonix-Network/GeckoLib-Refined/tree/1.19) | active | _see [Releases](https://github.com/Vonix-Network/GeckoLib-Refined/releases)_ |
| 1.20.1+ | — | — | **Use [upstream](https://github.com/bernie-g/geckolib)** | — |

## Installation

1. Download the matching jar from [Releases](https://github.com/Vonix-Network/GeckoLib-Refined/releases) — pick the file for your MC version and loader (Forge / Fabric / Quilt).
2. **Remove the old `geckolib-*.jar`** from your `mods/` folder. The `modId` is the same; two cannot coexist.
3. Drop the Refined jar into `mods/`.
4. Start the server / client. No config migration required.

## Crash you should report

If your crash report names `software.bernie.geckolib3.*` and you're on MC 1.18.2 or 1.19.2, **open an issue here**, not on upstream — upstream doesn't ship for these versions any more. Include the full crash report, the mod that triggered the render, and your modlist.

## Repository layout

```
main                  ← this branch: docs, CHANGELOG, version matrix, CI workflows
1.18                  ← forked from bernie-g/geckolib@1.18, Refined patches on top
1.19                  ← forked from bernie-g/geckolib@1.19, Refined patches on top
```

Each version branch keeps upstream's tree shape (`Forge/`, `Fabric/`, `Quilt/`) so anyone can `git diff bernie-g/1.18..vonix/1.18` and see exactly what changed.

## Attribution & licensing

- **Upstream:** [bernie-g/geckolib](https://github.com/bernie-g/geckolib) by Bernie (`@bernie-g`) and contributors, primarily maintained by **Tslat** (`@Tslat`).
- **License:** MIT, preserved verbatim from upstream. See [`LICENSE`](LICENSE).
- **Refined fork maintainer:** [Vonix Network](https://vonix.network).
- **Backport audit log:** every commit cherry-picked from a newer upstream branch is logged in [`BACKPORT_LOG.md`](BACKPORT_LOG.md) with upstream SHA, author, and a one-line provenance summary.
- **Optimisation log:** changes that go beyond upstream (and could not be attributed to an upstream commit) are tagged `[OPTIMIZED]` in [`CHANGELOG.md`](CHANGELOG.md) with explicit rationale.

If you are the upstream maintainer and want this fork archived, renamed, or merged back: open an issue — happy to coordinate.

## Building from source

```bash
git checkout 1.18  # or 1.19
cd Forge   # or Fabric, Quilt
JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64 ./gradlew build
# Jar at: build/libs/geckolib-<loader>-<mcver>-<gecko>+refined.<N>.jar
```

Requires JDK 17. ForgeGradle 5 (Gradle 7.4) for 1.18.2. See the per-branch `README.md` for any version-specific quirks.

## Contributing

PRs welcome for: bug fixes, backports from upstream with provenance, build hygiene, CI improvements. Please **don't** PR API changes — Refined exists precisely because mods depend on the `geckolib3` API surface. New features go upstream, not here.

When backporting an upstream commit, your PR must:
1. Reference the upstream SHA in the commit message: `Backport bernie-g/geckolib@<sha>: <subject>`
2. Add an entry to `BACKPORT_LOG.md`
3. Add an entry to `CHANGELOG.md` under the next release's `### Backported` section

When proposing an optimisation that is **not** a backport:
1. Tag the commit subject `[OPTIMIZED]`
2. Add a `### Optimised` entry to `CHANGELOG.md` with rationale and measured/expected impact
3. Provide a before/after snippet in the PR description

---

_GeckoLib-Refined is not affiliated with or endorsed by the upstream GeckoLib project. It is an independent maintenance fork operating under the MIT license._
