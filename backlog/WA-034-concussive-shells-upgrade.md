---
id: WA-034
status: in-progress
size: M
phase: 1-game-readiness
priority: 40
---
# Slow-on-attack (Concussive Shells) as per-unit upgrades — pool diversity

## 🔨 Slice 1 done 2026-07-16 (PR) — Marauder
Marauder can now roll **Concussive Shells** (stock `PunisherGrenades`). Count-upgrade recipe: `addUpgradeToUpgrade("PunisherGrenades","PunisherGrenades")` + `AnyOf Marauder`; `CAbilResearch PunisherGrenades3` (Barracks slot 3 → Ghost Academy col 2); `CButton PunisherGrenades3` (stock concussive icon); GameStrings. Locally testable (count/stat upgrade). No XML comments.

## 🔨 Slice 2 done 2026-07-16 (PR) — VoidRay (custom pattern proven)
VoidRay concussive wired by hand (no stock chain): marker `CUpgrade ConcussiveVoidRay` → `CountUpgradeConcussiveVoidRayCompleteOnly` node → `UseConcussiveVoidRay` requirement → `VoidRayConcussiveResearched` validator; `CEffectApplyBehavior VoidRayConcussiveSlow` (Behavior=Slow, gated by that validator + NotStructure/NotFrenzied) injected into `VoidRayWeaponPeriodicSet` at **index 3** (additive — keeps the existing beam effects); research UI `ConcussiveVoidRay2` (Starport slot 2, col 1), name **"Concussive Beam"**. **Awaiting in-game test** (does the VoidRay still attack + does the slow apply post-research) before replicating.

## 🔨 Slice 3 built 2026-07-19 — all 10 remaining units wired (VoidRay tested "just spicy enough")
Full concussive chain built for: **Vulture, Hellion, Zealot, Zergling, Diamondback, Archon, Colossus, Mutalisk, Ultralisk, Wraith.** Each = marker `CUpgrade Concussive<Unit>` → CountUpgrade node → `UseConcussive<Unit>` req → `<Unit>ConcussiveResearched` validator → `<Unit>ConcussiveSlow` apply-behavior injected into the weapon effect; research UI `Concussive<Unit><slot>` (unit's slot → column); GameStrings; `addUpgradeToUpgrade` + `AnyOf` unit tag. Shared 2s / 70% slow.

Injection safety property: the original damage effect is kept inside a new `CEffectSet`, so damage distribution is unchanged — only the slow rides along the same targets.
- Single/melee (Vulture, Zealot, Zergling, Wraith[both weapons], Diamondback[already a Set]): wrap/append slow into the weapon's damage set.
- Splash/line/cleave (Hellion, Colossus, Ultralisk): slow injected into the AREA/SEARCH effect → ALL targets slow.
- Archon: no stock search existed → authored `ArchonConcussiveSlowSearch` (r0.8) beside the splash damage.
- Mutalisk bounce: slow on primary + both bounces (GlaiveWurmS1/S2/S3).

**Verify in-game (riskier — flagged by the research pass):** Archon (custom search), Ultralisk (Liberty note hints its cleave search may be inactive; if only the primary slows, treat like Archon), Mutalisk (3-hop bounce). Everything else is the proven VoidRay-style wrap.

**Massive:** massive units are IMMUNE (2026-07-19) — added a shared `NotMassive` unit-filter validator (`-;Massive`) to all 11 custom apply-Slow effects (VoidRay + 10). Marauder's stock concussive already excludes massive natively. Tooltips say "non-massive"; splash/line/bounce/cleave tooltips call out the mechanic.

**Test convenience (devMode):** `getConcussiveUpgradeForUnit` auto-rolls each concussive unit's concussive upgrade, so walking `testCaseNumber` 1→7 shows Concussive pre-selected on every concussive unit's upgrade facility — one sweep tests all.

## 📋 Locked expansion plan (2026-07-19) — build AFTER the VoidRay test confirms the pattern
**Slow tuning — shared `Slow` behavior, ONE place, applies to every concussive unit at once:**
- Duration: **2s** (done, `BehaviorData.xml` Slow override; supersedes the earlier 1.5s).
- Strength: **70%** (`MoveSpeedMultiplier 0.3`, done) — target moves at 30% speed. Deliberately strong (user chose 70% over the ≤65% caution). Easy to dial back if it plays oppressive.

**Unit list (12):**
- **Done:** Marauder ✅ (stock), VoidRay ✅ (custom).
- **Straightforward single-weapon** (copy the VoidRay pattern verbatim): Vulture, Hellion, Zealot, Zergling, Diamondback.
- **Splash / line / bounce / cleave:** Archon, Colossus, Mutalisk, Ultralisk. **Decision (2026-07-19): the slow applies to ALL affected targets** — put the ApplySlow in the AREA/search effect (not just the primary impact) so the whole clump / line / bounce-chain / cleave slows.
- **Multi-weapon:** Wraith — wire the slow into BOTH weapons (air `WraithA` + ground `WraithG`).

**Notes:** Zealot / Zergling / Ultralisk are melee → "sticky melee" (target can't kite away) — intended, stronger flavor. Each variant also needs a WA-050 family entry → family `"ConcussiveShells"` so the roll-cap ([[WA-049]]) counts them all as one. See [[WA-050]].

**Build order:** the VoidRay test validates the custom pattern before replicating ×10 (~10 units × ~9 data pieces each — the mechanical fan-out batch).

## Why (updated)
Goal is a **more diverse pool for basic attackers** so players stop rolling blank. Marauder-only "doesn't move the needle" — the slow needs to be available on **multiple units**. Per decision: accept **per-unit count upgrades** (no behavior shortcut exists — see below).

## 🔍 Mechanism findings

### Behavior route is ruled out (searched, confirmed)
There is **no** SC2 mechanism for a behavior to inject an on-hit effect into a unit's attacks — searched every reference `behaviordata.xml`: no weapon-effect-append / on-attack-effect fields, and `DamageResponse` only fires on damage **taken**, not dealt. So `addBehaviorToUpgrade` can't do this (it'd silently fail). It must be **per-unit weapon-effect data gated by a count upgrade**.

### Marauder = the free special case (ship this first)
The Marauder already has the entire chain built in stock: weapon → `PunisherGrenadesSet` → `PunisherGrenadesSlow` (applies stock `Slow`) gated by validator `PunisherGrenadesResearched` (reads the `PunisherGrenades` upgrade). So Marauder concussive is just:
- `addUpgradeToUpgrade("PunisherGrenades", "PunisherGrenades")` + `addUpgradeRequirementTag(... AnyOf unitTag Marauder)`
- one research entry `PunisherGrenades3` (Barracks slot 3, column 2), copying `HighCapacityBarrels1` (`AbilData.xml:12`, `ButtonData.xml:344`) + GameStrings.
**This is a clean quick win — do it as slice 1.** (Full detail was the original WA-034 plan; it still holds.)

### Every other unit = custom per-unit wiring
For a non-Marauder unit **U** to slow on attack:
- U's weapon impact effect must *also* fire an apply-Slow. If U's weapon top effect is a bare `CEffectDamage`, wrap it in a `CEffectSet` {Damage, ApplySlow}; if it's already a Set, add ApplySlow to it.
- ApplySlow = a `CEffectApplyBehavior` (Behavior = stock `Slow`) gated by a validator reading U's own count upgrade — mirror `PunisherGrenadesSlow` + `PunisherGrenadesResearched`.
- **Multi-weapon units modify ALL weapons** (Goliath / Thor / Viking / Battlecruiser / Wraith have ground+air). Single-weapon units (Marine / Zergling / Hydralisk / Firebat / Vulture / Hellion) are one edit.
- Then the usual per-unit count-upgrade wiring: `CUpgrade` + requirement/validator + research UI (CAbilResearch / CButton / GameStrings) for U's slot.

## Suggested sequencing
1. **Slice 1:** Marauder via stock `PunisherGrenades` — quick win, proves the pool add.
2. **Slice 2:** prove the custom apply-Slow pattern on ONE single-weapon unit (e.g. Hydralisk or Firebat).
3. **Slice 3+:** mechanically replicate to the chosen list.

## Open decisions (grooming)
- **Which units** get slow-on-attack, and their slots? (drives # of research buttons)
- Reuse stock `Slow` (−50% move, 1.5s, excludes massive/structure) or a custom slow behavior?
- Any per-unit balance concerns (e.g. slow on a Thor is much stronger than on a Firebat)?

## Acceptance criteria
- [ ] Marauder can roll Concussive Shells (stock path).
- [ ] Each chosen custom unit can roll its version; after research, **all of that unit's weapons** slow non-massive ground targets.
- [ ] A unit's version is rollable only by that unit.

## Notes
Marauder slice = CLAUDE.md count-upgrade verbatim. Generalization = per-unit weapon surgery — heavier but mechanical once the pattern's proven. Sibling: [[WA-035]] (identical per-unit/multi-weapon structure; behavior route ruled out for both).
