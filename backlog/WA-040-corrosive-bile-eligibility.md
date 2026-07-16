---
id: WA-040
status: done
size: S
phase: 1-game-readiness
priority: 30
---
# Restrict who can roll RavagerCorrosiveBile (currently ~everyone)

## ✅ Done 2026-07-15
Went **subtractive**: added `NoneOf` tags in `upgradeInitializers.galaxy` for eight units.
- **Too cheap / too strong:** Marine, Hellion, Vulture.
- **Useless / awkward on it:** Immortal, Siege Tank, Lurker.
- **Too expensive to bother:** Archon, VoidRay.

After exclusions, the eligible span is **Marauder (100/25, cheapest)** → **DuskWing (200/150, most expensive)**. Marauder deliberately kept as the cheap floor (tankier than the cut units). Regenerated `docs/audits/upgrade-pools-by-unit.md`.

## Problem
Corrosive Bile is a strong ranged AoE, but it's rollable by **~23 units** — nearly every non-caster. Current eligibility (`upgradeInitializers.galaxy`): `NoneOf pureCaster, rax1, factory3, starport3, rax4, Ravager`. That leaves it available to Barracks s2/s3-fighters, all of Factory s1+s2, and Starport s1+s2.

- **Cheapest eligible unit: the Marine (~50/25).** Corrosive Bile on a 50/25 Marine is the broken case — insanely strong for the cost. Same issue on Hellion and Vulture.
- **User calls (2026-07-15):** remove **Marine, Hellion, Vulture** (too strong for their cost); **Immortal** should be excluded too (bile is useless on it).

## Decide: who *should* roll it?
Options:
- **Cost floor** — only units above some mineral/gas threshold (kills the cheap-unit abuse in one rule).
- **Curated list** — hand-pick the units where it's balanced *and* fun.
- **Subtractive** — keep broad, just `NoneOf` the offenders (Marine/Hellion/Vulture) + the duds (Immortal).

## Acceptance criteria
- [x] Decide the eligibility rule and apply it (NoneOf tags or a curated AnyOf list).
- [x] Marine / Hellion / Vulture can no longer roll Corrosive Bile; Immortal excluded.
- [x] Regenerate `docs/audits/upgrade-pools-by-unit.md` after.

## Notes
Found while reviewing the per-unit upgrade pools (`docs/audits/upgrade-pools-by-unit.md`). Sibling quick-fixes done same day: Medic no longer rolls Hyperjump; Ghost no longer rolls Recall.
