---
id: WA-015
status: done
size: M
phase: 1-game-readiness
priority: 9
---
# Blink on burrowed/sieged units teleports, then changes form

## ✅ DONE (2026-07-13) — trigger wired; confirmed on both Lurker and Siege Tank; devMode hacks removed
(Note: the editor trigger's Unit param must be "Any Unit", not "Triggering Unit", or it throws a harmless init error.)
- Removed the broken `addAltFormBannedAbility` calls (`upgradeInitializers.galaxy`).
- Added `onBlinkUsed()` (`unitInitializers.galaxy`): on blink, if caster is `LurkerMPBurrowed` → issue `BurrowLurkerMPUp`; if `SiegeTankSieged` → issue `Unsiege` (queued `AddToEnd` so it runs after the teleport).
- **YOU:** wire an editor trigger — Event: *Any Unit uses Ability `F_Blink`* (Complete stage); Action: Custom Script `onBlinkUsed();`.
- **Test:** blink a burrowed Lurker / sieged Tank → teleports THEN transforms. If it transforms at the old spot, bump the trigger stage later. (Liberator left alone — already unsieges incidentally.)

Change the intended behavior for Blink on alternate-form units. The old approach (`addAltFormBannedAbility`) was meant to *prevent* blinking while sieged/burrowed — but it never actually worked. New direction: don't ban it, transform it.

## Desired behavior
Press Blink → **instantly blink to the destination** → then **instantly trigger the transformation**.
- A **burrowed Lurker** blinks, lands, then unburrows.
- A **sieged Siege Tank** blinks, lands, then unsieges.

## Reference: Liberator already does this
The Liberator was given Blink and it already works this way — blinking unsieges it after the teleport. The base Liberator code (not authored here) makes this happen. There appears to be a **distance validator**: blinking a short distance does *not* trigger the unsiege (reason unknown). Worth inspecting how the Liberator is wired as a model. Not required to mirror it exactly — if simply triggering a transformation right after blink is easy, do that instead.

## Acceptance criteria
- [ ] Remove the non-functional `addAltFormBannedAbility` calls for `LurkerBurrowed` / `SiegeTankSieged` (upgradeInitializers.galaxy ~116-117).
- [ ] Blinking a burrowed Lurker teleports it, then unburrows it at the landing spot.
- [ ] Blinking a sieged Siege Tank teleports it, then unsieges it at the landing spot.
- [ ] No stuck/limbo states (mid-transform blink, half-sieged, etc.).
- [ ] Decide whether to replicate the Liberator's short-distance "no transform" quirk or ignore it.

## Notes
- Start by inspecting how the Liberator's blink→unsiege is set up (likely an ability/validator combo in `AbilData.xml` / `ValidatorData.xml`), then decide: reuse that pattern, or just fire a transform trigger after the blink resolves.
- Alt-form mappings live in `unitInitializers.galaxy` (`initializeAlternateForms()`), mapping units to their burrowed/sieged forms.
- `addAltFormBannedAbility` is being abandoned as a mechanism — this ticket replaces it.
