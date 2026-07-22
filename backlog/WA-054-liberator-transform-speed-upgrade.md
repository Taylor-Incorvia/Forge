---
id: WA-054
status: done
size: M
phase: 1-game-readiness
priority: 22
---
# Liberator transform-speed upgrade (Digging Claws equivalent) — replaces Speed

## ✅ Done 2026-07-20 (PR)
`LiberatorSiegeSpeed` ("Smart Servos") — mirrors Digging Claws' magnitudes: move speed ×1.5, and both morph durations (`LiberatorMorphtoAG` / `LiberatorMorphtoAA`, Actor + Stats) ×0.4. Replaces `Speed` for the Liberator (`Speed` → `NoneOf Liberator`); rolls at Starport slot 2 → Fusion Core col 1 (`LiberatorSiegeSpeed2`, reuses the Digging Claws icon). **Verify in-editor/in-game:** the morph-duration references use the **Unit-keyed** `InfoArray[LiberatorAG]` / `InfoArray[Liberator]` path (confirmed from the ability data, but the catalog-reference syntax for a unit-keyed InfoArray is the one thing to eyeball — worst case only the transform-speed half fails, move speed still applies).

## What
Give the **Liberator** (Starport slot 2) a rollable upgrade like `DiggingClaws` (which Siege Tanks / Lurkers roll): faster siege/unsiege transform + a move-speed bump. Since `DiggingClaws` also grants speed, this **replaces `Speed`** in the Liberator's pool (Liberator currently rolls `Speed`, not `Range` — Range is already `NoneOf Liberator`).

Suggested id `LiberatorSiegeSpeed`, display **"Smart Servos"** (the stock Terran mech transform-speed flavor).

**Decided:** match Digging Claws' magnitudes exactly — transform durations **×0.4** (i.e. 60% faster morph, both directions) and move speed **×1.5**, plus `RandomDelayMax` set to 0.125.

## Data (from research)
Model it on `<CUpgrade id="DiggingClaws">` (MOD `UpgradeData.xml`). Liberator specifics:
- Move speed: `Unit,Liberator,Speed` (base 3.375) — ×1.5 like DiggingClaws. (`LiberatorAG` sieged Speed=1 is stationary; skip or include harmlessly.)
- Transform abilities (void `abildata.xml`, note lowercase "to"): **`LiberatorMorphtoAG`** (mobile→Defender, base Duration 3.5417) and **`LiberatorMorphtoAA`** (Defender→mobile, base Duration 1.5417). Multiply the `SectionArray[Actor].DurationArray[Duration]` and `SectionArray[Stats].DurationArray[Duration]` ×0.4, mirroring how DiggingClaws does `SiegeMode`/`Unsiege`.

⚠️ **Verify in-editor:** the Liberator morph `InfoArray` element is keyed `Unit="LiberatorAG"`/`Unit="Liberator"`, not a plain `index="0"` like SiegeMode. The reference path may need `InfoArray[LiberatorAG]…` not `InfoArray[0]…`.

## Wiring
- `addUpgradeToUpgrade("LiberatorSiegeSpeed","LiberatorSiegeSpeed")` + `addUpgradeRequirementTag(AnyOf Liberator)`.
- `addUpgradeRequirementTag("Speed", logicType_NoneOf, "unitTag", "Liberator")` so Speed no longer rolls on it.
- `CAbilResearch`/`CButton` `LiberatorSiegeSpeed2` (Starport slot 2 → Fusion Core col 1), GameStrings, icon.

## Notes
- Like DiggingClaws, `AffectedUnitArray` hardcodes the unit — leave a comment (DiggingClaws already warns reslotting breaks it).
