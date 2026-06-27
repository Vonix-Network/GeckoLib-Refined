# Backport Log — 1.19 branch

This file tracks every change in the `1.19` Refined branch that originated upstream. See `main` branch `BACKPORT_LOG.md` for the schema and the `1.18` branch's entries.

---

## Refined branch: `1.19` (MC 1.19.2)

### a01f17b — Fix `math.pi` evaluating to 0
- **Upstream author:** @Tslat
- **Upstream branch:** `1.20.1`
- **Upstream URL:** https://github.com/bernie-g/geckolib/commit/a01f17bd2d6
- **Refined commit:** `6ca2986` on `1.19`
- **Files touched:** `Forge/core/.../MolangParser.java`, `Fabric/core/.../MolangParser.java`, `Quilt/core/.../MolangParser.java`
- **Portability rationale:** `MolangParser` constructor shape is identical between 1.19 (geckolib3) and 1.20.1 (geckolib4). The `register(new Variable(...))` call and `remap(...)` mechanism are unchanged. One-line addition, zero risk.
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
