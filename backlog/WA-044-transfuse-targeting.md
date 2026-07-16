---
id: WA-044
status: todo
size: S
phase: 1-game-readiness
priority: 22
---
# Let Transfuse heal (just about) any unit — kill the Biological target gate

Split from WA-025 (which decided *eligibility* — Transfuse now rolls on Sentry/Medic only). This ticket is the **targeting** half: the user wants Transfuse to be castable on almost any friendly unit (mechanical included), not just Biological ones.

## ❌ Attempt 1 (merged PR #6) — did NOT fix it
**The Queen uses the stock native `Transfusion` ability** (not the rolled `F_Transfusion`, which was already clean). The mod's `Transfusion` override only cleared its `CmdButtonArray` Requirements and inherited the stock bio-gated `TargetFilters`. **Attempt:** added a clean `TargetFilters` override to `Transfusion` (`Visible;Self,Neutral,Enemy,Missile,Stasis,UnderConstruction,Dead,Hidden,Invulnerable`, no Biological) — merged in **PR #6**.

**Result (in-game, 2026-07-16): still errors "Must target biological units" on a Siege Tank.** So the bio gate is **not** (only) in the ability's `TargetFilters`. Two remaining suspects, in order:
1. **A `Biological` validator on the inherited heal effect chain** (`TransfusionImpactSet` → its heal effect). That effect isn't in the mod's `EffectData.xml` or `reference/`, so it can only be seen/edited via the **SC2 Editor's merged view** — open the `Transfusion` effect, find the `ValidatorArray` entry that requires Biological, and remove it (or override the heal effect in the mod's `EffectData.xml` to clear it).
2. **Editor command/ability cache** (WA-045) — less likely for an ability filter, but the `TargetFilters` override may not have taken effect locally. Rule out with a clean rebuild before assuming #1.

**Decision 2026-07-16 (user):** since the gate makes Transfuse useful on only a small subset of units, **pull the rolled `F_Transfusion` from the upgrade pool for V1** so it can't be rolled as a dead-ish upgrade. Done in `upgradeInitializers.galaxy` (registration commented out; Sentry/Medic no longer roll it — doc updated). Also **reverted** the dead native-`Transfusion` TargetFilters override (the Queen keeps its stock bio-only Transfusion — fine for V1). Re-enable the roll once the bio validator is actually removed.

This ticket now needs the **editor merged-view validator hunt** to truly kill the bio gate — a hands-on-editor task. Parked in `todo`.

## What I found (static trace, 2026-07-16)
- **Ability layer is already clean.** `F_Transfusion` (`AbilData.xml`) `TargetFilters = "Visible;Self,Neutral,Enemy,Missile,Stasis,UnderConstruction,Dead,Hidden,Invulnerable"`. Format is `required;excluded` → the **only requirement is `Visible`** (plus allies-only). **No `Biological` requirement.** Stock SC2 gates Transfuse to Biological right here; this mod already removed it. There is also no `ValidatorArray` on the ability.
- **Effect chain is inherited / not in the repo.** `F_Transfusion` → effect `TransfusionImpactSet` → heal. That effect is in neither the mod's `EffectData.xml` nor `reference/`, so it comes from a base dependency. Any bio gate at the *effect-validator* level can't be seen or edited from the working tree — only the editor's merged view shows it.
- **Conclusion:** the moddable data has no remaining bio gate. The old in-game "Vulture/Corsair = can't transfuse" test (WA-025) was almost certainly on a build from **before** the TargetFilters change. A rebuild should already allow mechanical allies.

## Next steps
1. **Rebuild + retest first.** Roll Transfuse onto a Sentry/Medic, try casting on a mechanical ally (Vulture, Hellion, a Protoss unit). It very likely just works now.
2. **If it still bio-gates:** the restriction is a validator on the inherited effect. In the editor, open `F_Transfusion`'s effect (`TransfusionImpactSet` and its child heal effect) → find the Biological validator in the `ValidatorArray` and remove it (the editor's merged view is source of truth here — trust it over the raw XML). Alternatively, override the heal effect in the mod's `EffectData.xml` to clear the offending `ValidatorArray` entry.
3. **Optional — make it useful on shield units.** Transfuse only restores `Life` (+75). On Protoss/shield-tankers Life is rarely the low bar, so the heal feels dead even when targetable. Consider also restoring `Shields` (add a shield-heal to the effect set) so "any unit" is meaningful, not just legal.

## Acceptance criteria
- [x] Confirm in-game Transfuse can target mechanical/non-bio allies. → **fix applied (native `Transfusion` TargetFilters); needs re-test.**
- [ ] If it targets but doesn't heal, remove the Biological validator from the inherited heal effect and re-confirm.
- [ ] Decide the Shields question (add shield heal, or accept Life-only) and note the call.

## Notes
Can't be validated locally (same editor-test limitation as the rest of the caster work) — pairs with any build you're already deploying. Low risk: worst case is a one-toggle editor change on the effect validator.
