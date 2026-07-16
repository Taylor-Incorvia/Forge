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

**Slice 2+ (follow-up, needs your unit-list decision):** every other unit needs custom per-weapon surgery (wrap weapon damage effect in a Set + ApplySlow gated by that unit's count upgrade; multi-weapon units modify all weapons). Blocked on: **which units get slow-on-attack + their slots?** (see Open decisions). Marauder-only "doesn't move the needle," so this stays open until the unit list is picked.

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
