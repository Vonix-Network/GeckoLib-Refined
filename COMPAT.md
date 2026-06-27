# Compatibility Contract

GeckoLib-Refined exists to keep the `geckolib3` ecosystem alive on Minecraft 1.18.2 and 1.19.2. Hundreds of mods are already compiled against the `software.bernie.geckolib3` API. This document defines what we **will not break**, and what is fair game.

This contract is binding on every commit on every Refined branch. Subagents, contributors, and the maintainer all follow it. PRs that violate the contract are rejected even if they fix a bug — fix the bug a different way.

---

## Frozen surface (NEVER change)

1. **Mod ID.** `geckolib3` on every branch. Never `geckolib`, never `geckolib-refined`, never `geckolib3_refined`.
2. **Top-level package.** `software.bernie.geckolib3.*` on every Refined branch. The `geckolib3` segment stays, on every public, package-private, and internal class.
3. **Public class names.** No renames. `AnimationController`, `GeoEntityRenderer`, `AnimatedGeoModel`, `BoneSnapshot`, `MolangParser`, etc. all stay at their existing fully-qualified names. Third-party mods (including ones that mix in via `iobvariantloader.mixins.json` style addons) target these names directly.
4. **Public method signatures.** No renames, no parameter-order changes, no return-type widening/narrowing, no `throws` clause changes on public/protected methods. Adding new overloads is fine; changing existing ones is not.
5. **Public field shapes.** No type changes, no visibility narrowing, no `final` additions on fields that weren't final. Adding `final` to a field that was already effectively-final-in-practice is still a contract break: reflection-using consumers fail.
6. **Refmap structure.** `geckolib.refmap.json` must continue to map the same target Minecraft methods/fields it currently does. Changes to mixin targets are a contract break.
7. **`mods.toml` / `fabric.mod.json` / `quilt.mod.json` declared API.** `provides`, `dependencies`, version range syntax — frozen.

## Fair game (changes allowed)

1. **Method bodies** — fix bugs, add guards, harden lifecycle.
2. **Private/package-private new classes and methods** — internal helpers welcome.
3. **New overloads** of existing public methods (additive).
4. **Logging** — adding, demoting (WARN→DEBUG), or removing redundant warnings. Use SLF4J markers.
5. **Internal data structures** behind a private field — switching `HashMap` to `ConcurrentHashMap` is fine if the field is private and the visible behaviour matches.
6. **Build configuration, CI, repository hygiene.**
7. **Resource files** (`pack.mcmeta`, lang files) — additive only.

## Borderline (require maintainer sign-off in PR)

1. **`protected` methods.** Subclasses in third-party mods may override them. Changes go through PR review with explicit "is anything overriding this?" audit.
2. **Demoting a `RuntimeException` throw to a logged warning.** Sometimes the right fix, but consumers may have been catching the exception. Document in PR.
3. **New dependencies bundled into the jar.** Each new shaded library must be relocated under `software.bernie.geckolib3.shaded.*` and called out in the release notes.

## Test before tagging

For every Refined release on every branch:

1. **Bytecode contract test.** Run `javap -public -classpath build/libs/<refined>.jar software.bernie.geckolib3.core.controller.AnimationController` and diff against the same command on the upstream jar. The diff must be additive only (new methods OK, removed/changed methods are a contract break).
2. **Drop-in smoke test.** Install Refined into a Forge 1.18.2 + Isle of Berk + Alex's Mobs test instance. Server must start; one entity of each mod must render without exception. Documented in the per-release notes.
3. **Refmap presence.** `unzip -p build/libs/<refined>.jar geckolib.refmap.json` must contain non-empty mappings.

## When this contract makes a fix impossible

Some bugs may genuinely require a public API change to fix correctly. When that happens:

1. Document the case in an issue tagged `compat-break-required`.
2. Implement the **partial fix that the contract allows** (defensive guard, even if it papers over the root cause).
3. PR the **proper fix to upstream's actively-maintained branch** (1.20.1 or 1.21.1) so the architecture moves forward where it can.
4. Do not ship the contract-breaking version in Refined.

The contract is more valuable than any single bug fix.
