# Rollable upgrade descriptions (mod-accurate)

Website-ready descriptions for the 28 core rollable upgrades, derived from the mod DATA (UpgradeData / AbilData / EffectData / BehaviorData / upgradeInitializers.galaxy) and the patch notes, NOT from stock-SC2 memory. Snapshot 2026-08-06. Regenerate after balance changes.

Format: `UpgradeId | changed-from-stock | description | source`

## Global notes (apply to the whole caster column)
- **+25% energy regen, permanently:** every caster-spell upgrade also attaches the `CasterEnergyRegen` behavior (`VitalRegenArray[Energy] +0.140625/s`, i.e. +25%) to the unit that researches it. This is a mod addition layered on top of the stock spell — it is NOT reflected in "changed-from-stock" below, which judges the spell's own mechanics. Source: `BehaviorData.xml` (CasterEnergyRegen), `upgradeInitializers.galaxy` (attachEnergyRegenToCasterUpgrades).
- **Energy costs = standard as of 3.0 (patch 2026-07-27):** Force Field / Graviton Beam / Auto-Turret 50, Guardian Shield / Fungal Growth 75, Neural Parasite 100, Parasitic Bomb 125, Abduct 75, Irradiate 50. These now MATCH stock LotV, so those spells read "NO" even though they differ from *pre-3.0 mod* values.

## Mobility
- `Speed` | **YES (custom)** | Grants a permanent +50% movement speed (`CBehaviorBuff Speed`, MoveSpeedMultiplier 1.5); a mod-generic buff with no single stock equivalent. Excludes pure casters, melee, Tempest, and units with their own speed upgrade. | BehaviorData.xml (Speed), upgradeInitializers.galaxy
- `Charge` | **YES** | Grants the Charge ability (leap-intercept nearby enemies) PLUS a permanent +50% move speed (Speed behavior); melee-only, excludes Zergling. Stock Charge is Zealot-only and has no flat speed buff. | AbilData F_Charge, BehaviorData.xml (Speed), upgradeInitializers.galaxy
- `HotSRaptorCharge2` | **YES (custom)** | Grants the HotS "Raptor" leap: the unit pounces onto its attack target (2s cooldown) and gains a CliffJumper mover so it can jump up and down cliffs. Rolls on Zergling/Zealot (slot 1) and Firebat/Ultralisk (slot 3). Not a stock rollable. | AbilData (CAbilAugment HotSRaptorCharge2 -> HotSRaptorLeap2), BehaviorData (ReaperJump), ButtonData (btn cliffjump-zergling), upgradeInitializers.galaxy
- `Hyperjump` | **YES (application)** | Grants Tactical Jump: after ~6s the unit warps to any target location (no vision required) and is invulnerable while warping. Stock is the Battlecruiser's ability; here it is granted to many fliers (Tempest, Mutalisk, DuskWing, Viking, Wraith, Corsair, Colossus, Phoenix). | GameStrings F_Hyperjump/ResearchHyperjump, upgradeInitializers.galaxy
- `zerglingmovementspeed` | **NO ⚠️** | Stock Metabolic Boost: increases the Zergling's movement speed by 1.746 (~60%, to ~4.7). Not overridden by the mod (inherits the stock CUpgrade). Rolls on Zergling only (slot 1). **⚠️ VERIFY IN-GAME:** the mod doesn't override the Zergling's base speed and recently "gave zerglings default zergling speed" — confirm this upgrade still *increases* speed rather than being redundant before putting it on the site. | reference liberty.sc2mod upgradedata (Unit,Zergling,Speed +1.746), mod UpgradeData.xml (absent = inherits stock)
- `Blink` | **NO** | Stock Blink: teleports the unit a short distance to a target location. Granted to non-casters; excludes the Stalker (starts with Blink). | AbilData F_Blink, upgradeInitializers.galaxy

## Range
- `Range` | **YES (custom)** | Grants +2.5 weapon range (`CBehaviorBuff Range`, WeaponRange 2.5); a mod-generic buff with no single stock equivalent. Excludes units that use a bespoke range upgrade (Tempest, Siege Tank, Goliath, Phoenix) and a few others. | BehaviorData.xml (Range), upgradeInitializers.galaxy
- `LurkerRange` | **NO** | Stock Seismic Spines: +2 Lurker attack range (8 -> 10) and +2 spine hits on the attack line. Not overridden by the mod. Rolls on Lurker only (slot 2). | reference voidmulti upgradedata (Weapon,LurkerMP,Range +2; Effect,LurkerMP,PeriodCount +2), mod UpgradeData.xml (absent = pure stock)

## Combat
- `Stimpack` | **NO** | Stock Stimpack: temporarily boosts move and attack speed at the cost of health. Granted broadly; excludes pure casters, Void Ray, Archon, Zergling. | AbilData F_Stimpack, upgradeInitializers.galaxy
- `zerglingattackspeed` | **NO ⚠️** | Stock Adrenal Glands: +40% Zergling attack speed. The mod's override adds a Zealot PsiBlades effect (dormant — Zealot removed from the pool 2026-07-23); the stock Zergling Claws speedup should survive via catalog array-merge, but that merge is the uncertain part. Rolls on Zergling only (slot 1). **⚠️ VERIFY IN-GAME** that a rolled Zergling actually attacks faster before publishing. | reference void.sc2mod/liberty.sc2mod upgradedata (Claws RateMultiplier), mod UpgradeData.xml (zerglingattackspeed override adds Zealot/PsiBlades)
- `RavagerCorrosiveBile` | **NO** | Stock Ravager Corrosive Bile: launches a bile that deals 60 damage to all units in the area on impact and destroys Force Fields. Eligibility broadened to many mid-cost units. | GameStrings ResearchRavagerCorrosiveBile, patch 2026-07-22
- `Yamato` | **NO** | Stock Yamato Cannon: 240 single-target damage; capped at 1 roll, slot-3 Factory/Starport units. **The current in-game `ResearchYamato` tooltip wrongly says 260 (see fix below).** | reference liberty EffectData (Yamato -> YamatoU Amount 240), mod EffectData/AbilData (not overridden)
- `D8Charge` | **YES (custom)** | Campaign G-4 / D-8 cluster charge: explodes after a short delay for 30 splash damage (+100 vs structures) in a small radius (full to 1.1, falloff to 2.5), 60s cooldown, range 5. **The current `ResearchD8Charge` tooltip wrongly says 155 (see fix below).** | reference liberty.sc2campaign EffectData (D8ChargeExplodeDamage 30, +100 Structure), mod AbilData F_D8Charge, upgradeInitializers.galaxy

## Caster spells (all also carry +25% energy regen — see global note)
- `ForceField` | **NO** | Stock Force Field (50 energy): raises a barrier for 15s that blocks ground movement; massive units shatter it. Low-tier only (off Barracks s4 / Starport s3). | GameStrings F_ForceField/ResearchForceField, patch 2026-07-27 (energy 40->50)
- `GuardianShield` | **NO** | Stock Guardian Shield (75 energy): radius-4.5 aura that reduces incoming ranged damage to nearby allies by 2. Low-tier only. | GameStrings ResearchGuardianShield, patch 2026-07-27 (50->75)
- `GravitonBeam` | **NO** | Stock Graviton Beam (50 energy): lifts a target unit into the air for up to 10s, disabling it while the caster channels; massive units immune. | GameStrings ResearchGravitonBeam, patch 2026-07-27 (40->50)
- `MissilePods` | **YES** | Reworked into a low-tier anti-air burst: flat 60 damage to all air units in the target area (Light bonus REMOVED), 75 energy. | mod EffectData (HurricaneMissileDamage Amount 6, AttributeBonus[Light] 0; x PeriodCount 10 = 60), patch 2026-07-22
- `BuildAutoTurret` | **YES** | Stock Auto-Turret (50 energy) but cast range increased 2 -> 5 so a ground unit can actually place it; deploys a timed defensive turret. | GameStrings ResearchBuildAutoTurret, patch 2026-07-22 (cast range 2->5)
- `SeekerMissile` | **NO** | Stock Hunter Seeker Missile: charges 4s then chases the target for 100 damage plus splash; fizzles if the target escapes 13 range. | GameStrings ResearchSeekerMissile
- `RavenScramblerMissile` | **NO** | Stock Interference Matrix: disables a Mechanical or Psionic unit for 11s (cannot attack or use abilities) and reveals cloaked units. | GameStrings ResearchRavenScramblerMissile
- `FungalGrowth` | **NO** | Stock Fungal Growth (75 energy): roots and slows enemies in the area by 75% and deals ~25 damage over its duration; reveals cloaked/burrowed units and blocks Blink/transport. | GameStrings F_FungalGrowth/ResearchFungalGrowth, patch 2026-07-27 (50->75)
- `NeuralParasite` | **NO** | Stock Neural Parasite (100 energy): mind-controls a target enemy unit while the caster channels; heroic units immune. | GameStrings F_NeuralParasite, patch 2026-07-27 (75->100)
- `ParasiticBomb` | **NO** | Stock Parasitic Bomb (125 energy): attaches a cloud dealing 120 damage over 7s to the target and nearby enemy air units; cannot target ground. | GameStrings ResearchParasiticBomb, patch 2026-07-27 (100->125)
- `BlindingCloud` | **NO** | Stock Blinding Cloud: creates a cloud (~5.7s) that reduces the attack range of ground units and structures beneath it to melee. | GameStrings F_BlindingCloud/ResearchBlindingCloud
- `Irradiate` | **NO** | Stock Irradiate (50 energy): 30s damage-over-time centered on a target, damaging nearby biological units; does not damage mechanical units directly. | GameStrings F_Irradiate, patch 2026-07-27 (40->50)
- `Yoink` (displayed **Abduct**) | **NO** | Stock Abduct (75 energy): pulls a target unit to the caster's location. | GameStrings ResearchYoink, patch 2026-07-27 (50->75)
- `ArbiterMPRecall` (displayed **Recall**) | **YES (custom)** | Campaign Arbiter Recall: teleports all friendly units in the target area to the caster. Not present in stock multiplayer; a mod-added rollable. | GameStrings F_ArbiterMPRecall/ResearchArbiterMPRecall, upgradeInitializers.galaxy
- `CorsairMPDisruptionWeb` (displayed **Disruption Web**) | **YES (custom)** | Campaign Corsair Disruption Web: every ground unit and structure under it — friendly AND enemy — cannot attack (area denial). Low-tier only (Queen/Sentry/Medic/Phoenix/Wraith). | GameStrings ResearchCorsairMPDisruptionWeb, patch 2026-07-27

## Naming questions (resolved from data)
- **HotSRaptorCharge2** — the icon is `btn-ability-zerg-cliffjump-zergling` and the ability sets `Mover=CliffJumper` + attaches `ReaperJump`. It is NOT a damage charge; it is a leap that (a) pounces onto the attack target every 2s and (b) lets the unit jump up/down cliffs. Displayed as "Raptor Charge" on the unit's card.
- **LurkerRange** — yes, this IS stock Seismic Spines (+2 range to 10, +2 spines); Button/Name already reads "Seismic Spines".
- **zerglingattackspeed / zerglingmovementspeed** — yes, these are the stock ids for Adrenal Glands (+40% attack speed) and Metabolic Boost (+1.746 speed). Both currently roll on the Zergling ONLY (slot 1); the "rolls on units other than the Zergling" note is stale — the Zealot was removed from both pools on 2026-07-23.

## Data-confirmed errors found in EXISTING in-game tooltips
1. **ResearchMissilePods1-4** say "40 splash damage (+50 vs light)" — actual is flat **60** vs air, no light bonus. (fixed in this pass)
2. **ResearchYamato1-4** say "260 damage" — actual is **240** (stock, not overridden). (fixed in this pass)
3. **ResearchD8Charge1-4** say "155 splash damage in a radius of 3" — actual is **30** splash (+100 vs structures) in a ~2.5 radius. (fixed in this pass)

## Publish-verification / rendering notes
- All added tooltips are for **research buttons on the upgrade facilities** (Ghost Academy / Armory / Fusion Core), whose CButtons already carry icons and correct column layouts in ButtonData.xml — they render like the existing count/stat research buttons and should be visible in the local Test Document.
- The unit-card ability buttons that these upgrades GRANT (e.g. the "Raptor Charge" button on the unit) are added via UnitAbilityAdd and per CLAUDE.md only render on PUBLISHED builds — but those are separate from the research-button tooltips added here.
