# Backport Log

This file is the **single source of truth** for every change in a Refined branch that originated upstream. Every Refined branch (`1.18`, `1.19`) maintains its own section. Every entry records: upstream SHA, upstream author, upstream branch the fix came from, the Refined commit that applied it, and the rationale for why it was portable to the `geckolib3` architecture.

If a change is **not** in this file, it must appear in `CHANGELOG.md` tagged `[OPTIMIZED]` instead — those are Refined-original changes with no upstream provenance.

---

## Entry schema

```
### <upstream-short-sha> — <upstream-commit-subject>
- **Upstream author:** @<github-handle>
- **Upstream branch:** `<branch>` (e.g. `1.20.1`)
- **Upstream URL:** https://github.com/bernie-g/geckolib/commit/<full-sha>
- **Refined commit:** <refined-short-sha> on `<refined-branch>`
- **Files touched:** `path/to/File.java`, ...
- **Portability rationale:** Why this commit applies cleanly to geckolib3 (1.18.x architecture). If the upstream commit touches geckolib4-only classes, what was translated.
- **Behaviour delta:** What changes for end users. "None — pure NPE guard" is a valid answer.
- **Compat impact:** None | API-additive only | (must be None for inclusion)
```

---

## Refined branch: `1.18` (MC 1.18.2)

### a01f17b — Fix `math.pi` evaluating to 0
- **Upstream author:** @Tslat
- **Upstream branch:** `1.20.1`
- **Upstream URL:** https://github.com/bernie-g/geckolib/commit/a01f17bd2d6
- **Refined commit:** `1f42531` on `1.18`
- **Files touched:** `Forge/core/src/main/java/software/bernie/geckolib3/core/molang/MolangParser.java`, `Fabric/core/.../MolangParser.java`, `Quilt/core/.../MolangParser.java`
- **Portability rationale:** `MolangParser` constructor shape is identical between geckolib3 (1.18.2) and geckolib4 (1.20.1+). The `register(new Variable(...))` call and `remap(...)` mechanism are unchanged across the architectural rewrite. One-line addition, zero risk.
- **Behaviour delta:** Animations referencing `math.pi` (and the `pi` alias that remaps to `math.pi`) now evaluate to `Math.PI` instead of `0.0`. Affected animations: any using `math.pi` for rotation/scale math. Previously broken — now correct.
- **Compat impact:** None.

### a05782f — Crash with forced animation before transition completes
- **Upstream author:** @Tslat
- **Upstream branch:** `1.20.1`
- **Upstream URL:** https://github.com/bernie-g/geckolib/commit/a05782f
- **Refined commit:** _populated at tag time_ on `1.18`
- **Files touched:** `Forge/core/.../AnimationController.java`, `Fabric/core/.../AnimationController.java`, `Quilt/core/.../AnimationController.java`
- **Portability rationale:** The `justStartedTransition` + `shouldResetTick` + `justStopped` state-machine block at line 419 is byte-identical between geckolib3 (1.18.2) and the geckolib4 1.20.1 base at the time of this commit (the 4.x rewrite came later). Same pattern, same fix shape. Three lines added, no API change.
- **Behaviour delta:** When an animation is forced mid-transition (typical for blocks/items with state-driven animations), the controller no longer crashes on the next tick. Instead it re-asserts `AnimationState.Transitioning`, allowing the new animation to take over cleanly.
- **Compat impact:** None.

---

## Refined branch: `1.19` (MC 1.19.2)

### a01f17b — Fix `math.pi` evaluating to 0
- **Upstream author:** @Tslat
- **Upstream branch:** `1.20.1`
- **Upstream URL:** https://github.com/bernie-g/geckolib/commit/a01f17bd2d6
- **Refined commit:** _populated at tag time_ on `1.19`
- **Files touched:** `Forge/core/.../MolangParser.java`, `Fabric/core/.../MolangParser.java`, `Quilt/core/.../MolangParser.java`
- **Portability rationale:** `MolangParser` constructor shape is identical between 1.19 (geckolib3) and 1.20.1 (geckolib4). The `register(new Variable(...))` call and `remap(...)` mechanism are unchanged across the architectural rewrite. One-line addition, zero risk.
- **Behaviour delta:** Animations referencing `math.pi` (and the `pi` alias that remaps to `math.pi`) now evaluate to `Math.PI` instead of `0.0`. Previously broken — now correct.
- **Compat impact:** None.

### a05782f — Crash with forced animation before transition completes
- **Upstream author:** @Tslat
- **Upstream branch:** `1.20.1`
- **Upstream URL:** https://github.com/bernie-g/geckolib/commit/a05782f
- **Refined commit:** _populated at tag time_ on `1.19`
- **Files touched:** `Forge/core/.../AnimationController.java`, `Fabric/core/.../AnimationController.java`, `Quilt/core/.../AnimationController.java`
- **Portability rationale:** State machine block at line 419 is byte-identical between 1.19 (geckolib3) and the geckolib4 1.20.1 base at commit time. Same pattern, same fix shape.
- **Behaviour delta:** Forced animation switch mid-transition no longer crashes on the next tick.
- **Compat impact:** None.
