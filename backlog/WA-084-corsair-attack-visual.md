---
id: WA-084
status: done
size: M
phase: 2-polish
priority: 70
---
# Corsair attack visual — add a punchy laser (Neutron Flare has no beam)

## ✅ DONE 2026-08-20 (PR #42, merged to main `8caa107`)
Shipped a `GenericAttackBeam` laser (Corsair weapon → target, per shot) + a softened impact flash (`ColossusAttackBeamImpact`, scale 1.0, tint {80,140,255} @1.5). Both are additive cosmetic actors in `ActorData.xml` — confirmed rendering in the Test Document (attack visuals are NOT publish-gated; that caveat is only for command-card buttons of added abilities). Also widened the splash **0.5 → 0.82** for feel/readability (per playtest; a modest area buff, not per-hit damage — the DPS verdict to not buff damage still holds). Corsair build-button tooltip now states the air splash (shipped separately, `5d45bd2`). Icon fix also shipped (`6747186`). No new shockwave-ring model existed in base data, so the toned-down energy burst stands; revisit only if a truer ring is wanted later.

## What
The Corsair's attack reads as "does nothing" — it fires with a launch **sound** but no visible beam or projectile. Add a punchy laser beam **and a visible impact splash flash** so the attack lands with weight, and **update the tooltip to say it deals air splash**. Pure visual + text, no gameplay change.

## Balance verdict — DO NOT buff damage (confirmed 2026-08-20)
DPS across the Starport slot-1 air units (single-target):
- **Corsair** (NeutronFlare): 5 / 0.47s = **~10.6 air DPS**, flat, **+ real splash (`AreaArray` r=0.5)**.
- Phoenix 9.1 base / 18.2 vs Light · Viking(air) 10.0 / 14.0 vs Armored · Wraith(air) 8.0 / 16.0 vs Armored.
- The Corsair has the **highest *flat* air DPS** of the four **and is the ONLY one with a real `AreaArray`** — Phoenix/Viking/Wraith are tagged `Kind="Splash"` but carry no AreaArray, so they don't actually splash. Against a clumped air ball the Corsair is the best in the slot by a mile.
- **Conclusion:** the Corsair is already strong; it just doesn't *look* it. This ticket is **purely readability** — do NOT raise damage or add an attribute bonus. Buffing a real-splash air unit is how it becomes oppressive vs air.

## Why it looks broken (root cause)
`CActorAction id="CorsairMPAttack"` (parent `GenericAttack`, `effectAttack="NeutronFlare"`) specifies only `LaunchAssets Sound="CorsairMP_AttackLaunch"` + an impact sound — **no launch/beam model.** So you hear it, see almost nothing. (Base Void data ships it this way.)

## Approach (beam)
Add a `CActorBeamSimple` keyed to the Neutron Flare attack — created when the weapon fires, drawn from the Corsair's weapon attach point to the target, **one beam per shot**.
- Model it on the **Colossus attack beam** (`ColossusAttackBeamAir`, actordata ~2318) — a *per-shot* beam, the right fit for the Corsair's discrete attacks (the Oracle's `OracleWeaponAttackBeam` ~2737 is a *continuous* channel — wrong shape). `VoidRaySwarmAttackBeam` ~2985 is another template.
- Reuse a base Protoss beam **model** (e.g. `RipFieldAttackBeam`, the Colossus lance, or the Void Ray beam) — **no new art**; those assets resolve from the shared game files regardless of dependencies. Pick a bright one for punch.
- Optional extra punch: a bright impact flash model on hit + a muzzle glow at the launch attach point.
- Weapon is `NeutronFlare` (CWeaponLegacy); it attacks **air**, so use an air-capable beam (Colossus/Void Ray beams handle air).

## Cheaper alternative (if a literal beam isn't required)
Add a **projectile `LaunchModel` + impact flash** to the existing `CActorAction` instead of a beam. Less iteration; gives a visible bolt that *lands* with weight, just not a laser line.

## Effort / risk
Moderate. The actor is small + self-contained (~15–30 lines of ActorData) and **pure-visual → zero gameplay risk**. Cost is iteration: attack visuals must be seen live, and tuning the beam's timing / attach point / duration (so it doesn't linger) is trial-and-error — same publish-and-eyeball loop as the Firebat flames. Budget a couple of test-publish cycles.

## Acceptance
- [ ] Corsair's attack shows a visible, punchy beam (or bolt+flash) from unit to target, per shot.
- [ ] A visible **impact splash flash** sells the AoE — you can see it hit multiple clumped air units.
- [ ] **Corsair tooltip states it deals area/splash damage to air.**
- [ ] Beam appears/disappears cleanly with each attack — no lingering/stuck beams.
- [ ] **No damage or attribute-bonus change** — readability only. Verify on a published build.

## Notes
Post-launch polish. Corsair is `CorsairMP` (restored BW unit, not WoL) — stays regardless of the WoL-dependency decision. Icon was already fixed (real `btn-unit-protoss-corsair.dds`); this ticket is the remaining "weapon looks weak" half.
