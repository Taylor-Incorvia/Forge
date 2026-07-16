---
id: WA-044
status: todo
size: S
phase: 1-game-readiness
priority: 22
---
# Let Transfuse heal (just about) any unit — kill the Biological target gate

Split from WA-025 (which decided *eligibility* — Transfuse now rolls on Sentry/Medic only). This ticket is the **targeting** half: the user wants Transfuse to be castable on almost any friendly unit (mechanical included), not just Biological ones.

## What I found (static trace, 2026-07-16)
- **Ability layer is already clean.** `F_Transfusion` (`AbilData.xml`) `TargetFilters = "Visible;Self,Neutral,Enemy,Missile,Stasis,UnderConstruction,Dead,Hidden,Invulnerable"`. Format is `required;excluded` → the **only requirement is `Visible`** (plus allies-only). **No `Biological` requirement.** Stock SC2 gates Transfuse to Biological right here; this mod already removed it. There is also no `ValidatorArray` on the ability.
- **Effect chain is inherited / not in the repo.** `F_Transfusion` → effect `TransfusionImpactSet` → heal. That effect is in neither the mod's `EffectData.xml` nor `reference/`, so it comes from a base dependency. Any bio gate at the *effect-validator* level can't be seen or edited from the working tree — only the editor's merged view shows it.
- **Conclusion:** the moddable data has no remaining bio gate. The old in-game "Vulture/Corsair = can't transfuse" test (WA-025) was almost certainly on a build from **before** the TargetFilters change. A rebuild should already allow mechanical allies.

## Next steps
1. **Rebuild + retest first.** Roll Transfuse onto a Sentry/Medic, try casting on a mechanical ally (Vulture, Hellion, a Protoss unit). It very likely just works now.
2. **If it still bio-gates:** the restriction is a validator on the inherited effect. In the editor, open `F_Transfusion`'s effect (`TransfusionImpactSet` and its child heal effect) → find the Biological validator in the `ValidatorArray` and remove it (the editor's merged view is source of truth here — trust it over the raw XML). Alternatively, override the heal effect in the mod's `EffectData.xml` to clear the offending `ValidatorArray` entry.
3. **Optional — make it useful on shield units.** Transfuse only restores `Life` (+75). On Protoss/shield-tankers Life is rarely the low bar, so the heal feels dead even when targetable. Consider also restoring `Shields` (add a shield-heal to the effect set) so "any unit" is meaningful, not just legal.

## Acceptance criteria
- [ ] Confirm in-game Transfuse can target mechanical/non-bio allies.
- [ ] If not, remove the Biological validator from the effect chain (editor) and re-confirm.
- [ ] Decide the Shields question (add shield heal, or accept Life-only) and note the call.

## Notes
Can't be validated locally (same editor-test limitation as the rest of the caster work) — pairs with any build you're already deploying. Low risk: worst case is a one-toggle editor change on the effect validator.
