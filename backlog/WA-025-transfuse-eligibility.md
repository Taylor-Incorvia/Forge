---
id: WA-025
status: done
size: S
phase: 1-game-readiness
priority: 21
---
# Figure out who should roll the Transfuse upgrade

## ✅ Done 2026-07-16
**Eligibility decision:** keep Transfuse, but pull it off the Starport slot-1 fliers. Added `NoneOf starport1` in `upgradeInitializers.galaxy` — heal-another is too situational on lone aggressive flyers (Phoenix/Corsair/Wraith).

**Now rolls on: Sentry, Medic** (the two Barracks-s3 casters). Doc updated (`upgrade-pools-by-unit.md`).

**Targeting question → split to [WA-044](./WA-044-transfuse-targeting.md).** The Biological-target-gate / Life-vs-Shields question below was moved to its own ticket (user wants Transfuse castable on almost any unit). Static trace there found no bio gate left in the moddable data — likely already works post-rebuild.

Transfuse (`F_Transfusion`, target-heal an ally) has come up a lot in rolls and felt lame in some situations. Decide the intended set of units that should be able to roll it, then adjust the tags — or cut it.

## Why
A heal-another spell is unexciting on an aggressive unit, and can feel like a dead roll when you've got nothing worth healing. The existing code already half-acknowledges this (it's excluded from Starport slot 3 as "not exciting enough"). Time to make a deliberate call rather than leaving it broadly available.

## Current eligibility (upgradeInitializers.galaxy ~100-104)
- `AllOf caster` — any caster-tagged unit
- `NoneOf starport3` ("not exciting enough for slot 3")
- `NoneOf rax4`
- `NoneOf Queen` (Queen has it natively — not added yet)

So today it rolls on: **Sentry, Medic, Phoenix, CorsairMP, Wraith**.

## The decision to make
- On which units is Transfuse actually *fun/impactful* vs. a dead roll?
  - Medic already heals bio — Transfuse may be redundant there.
  - On lone aggressive flyers (Phoenix/Corsair/Wraith) heal-another is situational.
- Design lens: you previously cut DefensiveMatrix because you don't want casters spending energy on *surviving* instead of casting offense. Transfuse leans the same direction — is it worth keeping at all?

## Also: what Transfuse can TARGET (data change)
Goal: let it heal *anything* useful (it's not a strong ability, so no reason to restrict).

### Confirmed test matrix (in game)
| Target | Type | Transfusable? |
|--------|------|---------------|
| Marauder | Biological | ✅ yes |
| Hydralisk | Biological | ✅ yes |
| Vulture | Mechanical (no shields) | ❌ no |
| Corsair | Mechanical (Protoss) | ❌ no |

Clean split → it's a **Biological target gate** (the Vulture rules out any shields/Life-full explanation).

### The puzzle: the gate is NOT in the current `F_Transfusion` data
Re-confirmed against the working tree + reference:
- `F_Transfusion` `TargetFilters` = `Visible;Self,Neutral,Enemy,...` — **no `Biological`**, no `parent=`, no `ValidatorArray`.
- Effect chain (`TransfusionImpactSet` → heal `Life+75`); validators `LifeNotFull` (life-fraction check) + `NotDisintegrating`. No Biological anywhere.

So the current files *shouldn't* bio-gate. Most likely **the test was on a build from before `Biological` was removed** — the fix may already be in the files, just not rebuilt.

### To do
- [ ] **Rebuild + retest** — the Biological filter may already be gone; it might just work now.
- [ ] If it still bio-gates: in the SC2 editor open `F_Transfusion` → Stats → **Target: Filters**, set **Biological** to *not Required* (editor shows the true merged value; trust it over the raw XML). One toggle.
- [ ] Then decide the Life-only heal question: Transfuse only heals `Life` (+75), so it's weak on shield-tanking Protoss whose Life is rarely low. Optionally have it also restore `Shields`, or accept the limitation (it's a weak ability anyway → ties into the "keep or cut" decision above).

## Acceptance criteria
- [ ] **Targeting:** confirm in-game whether Transfuse already hits non-bio allies. If not, find + remove the Biological validator in the effect chain.
- [ ] **Eligibility:** decide keep-and-restrict (which units/slots?) or remove entirely.
- [ ] Adjust the `addUpgradeRequirementTag` lines for `Transfusion` to match, OR remove the `addAbilityToUpgrade("Transfusion", ...)` registration.
- [ ] If kept: confirm the remaining set feels good in a few rolls.

## Notes
Since it gates on `AllOf caster`, hybrids (Ghost/Phoenix/CorsairMP/Wraith) are eligible unless slot-excluded. If you want it only on dedicated support units, a `NoneOf`-per-hybrid or an `AllOf`-on-a-support-tag approach both work.
