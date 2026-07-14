---
id: WA-018
status: done
size: L
phase: 1-game-readiness
priority: 12
parent: WA-002
---
# Let hybrid units roll BOTH caster and non-caster upgrades

Introduce a "hybrid" path in the upgrade-pool system so units that have **both an energy bar and an attack** (Queen, DuskWing) can draw from both the caster and non-caster upgrade pools.

## Why
Today the pool splits cleanly in two and the halves are mutually exclusive: caster upgrades require the `caster` tag (`logicType_AllOf`), non-caster upgrades forbid it (`logicType_NoneOf`). A unit is either/or. Units that both cast and fight can't currently draw from both, which is exactly what the Queen and DuskWing need.

## ⭐ Do before WA-019 and WA-020 — it unblocks both.

## Acceptance criteria
- [x] Document how caster vs non-caster eligibility is currently decided (see below).
- [x] Design a mechanism decoupling "can cast" from "excluded from fighter buffs".
- [x] Pure casters and pure fighters are **unchanged** (behavior-preserving).
- [x] No broken rolls — hybrid gets caster gate + fighter eligibility, nothing it can't use.
- [x] Mechanism is general (any unit → `tagHybridCaster()`), Queen + DuskWing as first targets.
- [ ] **YOU:** in-game sanity check — current rolls unchanged (no behavior diff), and a temp `tagHybridCaster("DuskWing")` confirms it can roll from both pools.

## ✅ Implemented (2026-07-12) — behavior-preserving
The `caster` tag did two opposite jobs (gate for caster upgrades via `AllOf`, exclusion for fighter upgrades via `NoneOf`), making hybrids impossible. Split it:
- `caster` = eligible for caster/energy-ability upgrades (unchanged `AllOf` gate; ~18 caster upgrades untouched).
- `pureCaster` = a caster excluded from fighter buffs (new `NoneOf` exclusion).
- New helpers in `forgeUnitHelpers.galaxy`: `tagPureCaster()` (both tags) and `tagHybridCaster()` (caster only).
- `unitInitializers.galaxy`: the 12 pure casters now use `tagPureCaster()`.
- `upgradeInitializers.galaxy`: fighter upgrades' `NoneOf caster` → `NoneOf pureCaster` (Blink/Range/Speed/Stim/Yamato/RavagerCorrosiveBile + the commented ones).

Result — identical current behavior (every caster has both tags); hybrids get BOTH pools by carrying `caster` without `pureCaster`. To make a unit hybrid: `tagHybridCaster("X")`.

## Resolved: Ghost is now a hybrid
Ghost's dead `fighter-caster` tag → `tagHybridCaster("Ghost")` (the original intent). This is the ONE intentional behavior change in this ticket — Ghost now also rolls caster spells (previously fighter-only). Everything else is behavior-preserving.

Bonus: Ghost is already in the pool (Barracks slot 4), so it's a **live test case** for the hybrid mechanism — roll a Ghost and confirm it can pull both a caster spell and a fighter buff.
