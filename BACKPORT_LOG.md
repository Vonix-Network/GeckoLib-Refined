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

_No backports applied yet — population begins with the first Refined release._

---

## Refined branch: `1.19` (MC 1.19.2)

_No backports applied yet — population begins with the first Refined release._
