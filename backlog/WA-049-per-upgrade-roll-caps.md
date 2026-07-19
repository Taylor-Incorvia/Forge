---
id: WA-049
status: done
size: M
phase: 1-game-readiness
priority: 15
---
# Per-upgrade roll caps — stop the same upgrade landing on too many units

## ✅ Implemented 2026-07-19 (branch wa-049-upgrade-caps) — awaiting playtest
Replaced the per-slot independent roll with a global cap-aware pass (`assignUpgradesWithCapsForPlayer` in `upgradeInitializers.galaxy`):
- **Caps** (`getUpgradeCap`): **1** for Blink, RavagerCorrosiveBile, any `AllOf caster` upgrade (auto-detected from the tag registry — 15 spells, zero maintenance), and the **ConcussiveShells family** (user chose 1, not 2); **2** for everything else (Speed, Range family, Stimpack, Lifesteal, the 38 specifics).
- **Per-family counting** (WA-050) via `getFamilyUsedCount`, derived live from the selected-upgrade state (no counter to reset between games).
- **Assignment**: build all 10 eligible pools, assign **smallest-pool-first**; each pick = random candidate that is under-cap AND doesn't break the same-slot-number rule; graceful fallback (drop the cap, then last-resort) so **no slot is ever blank**.
- **Same-slot-number uniqueness rule preserved** via `isUpgradeAlreadyAssignedToSlot` (the `<upgrade><slot>` research id is shared across facilities — user flagged this must not regress; it doesn't).
- Concussive test hack removed. Both galaxy files typecheck clean.

**Playtest checklist:** Blink ≤1, each caster ≤1, concussive ≤1 across your army; Speed/Range/Stimpack ≤2; overall variety up; no slot rolls blank; Archon/VoidRay/Sentry never stranded.

## Why
Player feedback (PiG, others): it feels repetitive when the same upgrade rolls onto several units in one game (e.g. Blink on 3 different slots). But "how many is too many" genuinely differs per upgrade — 2 units with Range is fun, 2 with Corrosive Bile is oppressive, 2 casters with the same spell is pointless. So a **per-upgrade cap** (max number of units that can roll a given upgrade in one game), not a single blanket rule.

## Design: default cap 2, with a short override list
| Cap | Upgrades |
|-----|----------|
| **1** | `Blink`, `RavagerCorrosiveBile`, **every caster-ability upgrade** (the ones gated by `AllOf caster`) |
| **2 (default)** | `Speed`, `Range`, `PunisherGrenades` (Concussive), `LifestealMarine` (Lifesteal), and everything else |

- **Caster abilities → 1:** two separate casters that can cast the same spell is wasted — you'd just A-move both. Capping each caster ability at 1 maximizes caster variety.
- **Concussive / Lifesteal → 2** to start (a "lifesteal army" theme across 2 units is fun; 3 feels samey). Tune from playtests.
- New upgrades added later inherit the default 2 automatically.

**Caps are GLOBAL (across all 3 facilities / all 10 slots), not per-facility** — "no more than 1 unit rolls Blink" means in the whole game.

## Detecting the caster-ability set (pick the cleaner one during impl)
- **(a) Reuse the requirement registry:** caster abilities are exactly the upgrades registered with `addUpgradeRequirementTag(x, logicType_AllOf, "unitTag", "caster")`. If that registry is queryable, cap any `AllOf caster` upgrade at 1 — zero maintenance.
- **(b) Explicit cap registry:** a `setUpgradeCap(upgradeId, n)` called at registration time (default 2; set 1 next to each caster registration + Blink + Bile). More verbose but dead simple and self-documenting.

## Implementation — the actual work is the assignment, not the caps
The roll currently assigns each slot's upgrade **independently** (`assignRandomUpgradeFromPoolToPlayerSlot` per slot, via `setUpgradeSlotsForFacility`). Caps require a **global assignment pass across all 10 slots**:

1. Roll all units first (already happens), then build each slot's eligible upgrade pool.
2. **Sort slots by eligible-pool-size ascending** (most-constrained first) and assign in that order. For each slot, pick a random eligible upgrade whose current used-count `< getUpgradeCap(upgrade)`, then increment a per-game `upgradeUsedCount[upgrade]`.
3. **Graceful fallback, never blank:** if a slot has no under-cap option left, let it **exceed a cap** (pick its least-used eligible upgrade) rather than roll empty.

### Why the ordering matters (the canary)
**Archon** (Factory s2) and **VoidRay** (Starport s2) have pools of exactly `{Blink, Range, Speed}` — **no Stimpack fallback**. With Blink=1 / Range=2 / Speed=2, if those get used up by other slots *before* the Archon is assigned, it has nothing legal → the fallback fires (a 3rd Range/Speed). Assigning smallest-pool-first fixes it (Archon gets picked while options remain). Sentry (caster, 3 options) is the same story on the caster side. **Assigning independently / largest-first will occasionally strand these units.**

## Feasibility (checked against the pools 2026-07-16)
- **42 distinct upgrades**; per-facility distinct availability Barracks 27 / Factory 18 / Starport 26.
- Frequency is top-heavy: Blink is in 31 unit-pools, Stimpack 29, Speed 25, Range 23; everything else ≤15 (most 1–5). The generic four are the whole reason repeats happen — caps push rolls toward the 38 flavorful specifics (desired).
- Every unit has ≥3 eligible upgrades, so with smallest-pool-first + the fallback, **no slot ever ends up empty.**

## Acceptance criteria
- [ ] A per-game cap is enforced: no upgrade is rolled onto more units than its cap (default 2; 1 for Blink / RavagerCorrosiveBile / caster abilities).
- [ ] Caps are global across all facilities.
- [ ] Assignment is smallest-eligible-pool-first (verify Archon/VoidRay/Sentry never strand).
- [ ] Graceful fallback: a slot never rolls a blank/empty upgrade even when caps are tight.
- [ ] Concussive/Lifesteal capped at 2 (adjustable).
- [ ] Playtest: Blink no longer lands on 3 units; the pool feels more varied.

## Depends on WA-050 (upgrade families)
Per-unit upgrades (Lifesteal, Concussive) are implemented as a **different count-upgrade id per unit** (`LifestealMarine`, `LifestealQueen`, `PunisherGrenades`, …). The cap must count all variants of a concept as one, so it counts on **`getUpgradeFamily(upgrade)`** (see [[WA-050]]), not the raw id. Shared upgrades (Speed/Range/Blink/casters) are each their own single-member family, so they need nothing extra. **Build WA-050 first (or together).**

## Notes
Sibling context: this restructures the same code path the dev `testCaseNumber` sweep and the roll-force hacks touch (`assignRandomUpgradeFromPoolToPlayerSlot` / `setUpgradeSlotsForFacility`). Consider whether the Archon/VoidRay `{Blink,Range,Speed}`-only pools should be widened by one upgrade so they're less deterministic under caps — small optional follow-up, not a blocker.
