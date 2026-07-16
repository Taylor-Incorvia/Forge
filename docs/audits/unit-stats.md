# Wildcard Arena — Per-Unit Merged Stats

Snapshot **2026-07-16** — regenerate after unit cost/stat/weapon/build-time changes. Companion to [`upgrade-pools-by-unit.md`](./upgrade-pools-by-unit.md) (who can roll what) and [`unit-costs.md`](./unit-costs.md).

> **Reading these numbers:** all values are merged from two layers — the **ForgeMod** override files (authoritative when present) and the **base game** (voidmulti = LotV multiplayer, authoritative; Wraith/Firebat/Medic/Vulture/Diamondback/Goliath/DuskWing from the Liberty campaign; CorsairMP/WarHound from Void). **`(MOD)`** marks a value set in ForgeMod's own GameData; everything else is base.
>
> **Speed of these numbers — read this:** **every time and DPS below is a standard-speed catalog value** (what the editor shows). Multiplayer runs on **"Faster" (≈ ×1.4 game speed)**, so in the actual game: **build times are shorter — roughly `listed ÷ 1.4`** (a listed 30.8s ≈ 22s real), and **weapon DPS is higher — roughly `listed × 1.4`** (weapon Periods are catalog, so attacks come faster). Armor defaults to 0 when `LifeArmor` is unset. Shields listed only for Protoss.

---

## BARRACKS

### Slot 1

**Zergling**
- Cost: 25 min / 0 gas — Supply 0.5 (base)
- Build: **5.6s (MOD)**
- Life 35 | Armor 0 | Speed 2.9531 (×1.3 on creep)
- Weapon — Claws (ground, melee): 5 dmg ×1, period 0.696, range 0.1 → **DPS 7.2**

**Zealot**
- Cost: 100 min / 0 gas — Supply 2 (base)
- Build: **21s (MOD)**
- Life 100 | Shields 50 | Armor 1 | Speed 2.25
- Weapon — PsiBlades (ground, melee): 8 dmg ×2, period 1.2, range 0.1 → **DPS 13.3**

### Slot 2

**Hydralisk**
- Cost: 100 min / 50 gas — Supply 2 (base)
- Build: **26s (MOD)**
- Life 90 (base voidmulti; liberty was 80) | Armor 0 | Speed 2.25 (×1.5 on creep)
- Weapon — NeedleSpines (ground + air): 12 dmg ×1, period 0.825, range 5 → **DPS 14.5**

**Marine**
- Cost: 50 min / **25 gas (MOD)** — Supply 1  *(stock Marine is 50/0; mod adds 25 gas)*
- Build: **15s (MOD)**
- Life 45 | Armor 0 | Speed 2.25
- Weapon — GaussRifle (ground + air): 6 dmg ×1, period 0.861, range 5 → **DPS 7.0**

**Queen**
- Cost: 175 min (base) / **50 gas (MOD)** — **Supply 3 (MOD)** *(stock Queen is 150/0, supply 2)*
- Build: **32s (MOD)**
- Life 175 | Armor 1 | Speed **1.8984 (MOD)** (base chain 1.5; needs creep for full speed)
- Weapon — Talons (ground): 4 dmg ×2, period 1, range ~5 → **DPS 8**
- Weapon — AcidSpines (air): 9 dmg ×1, period 1, range 7 → **DPS 9**
- *Note: catalog Period is 1 here (external refs often cite 0.71).*

### Slot 3

**Firebat**
- Cost: 100 min (base) / **50 gas (MOD)** — Supply 2 *(campaign base gas is 25)*
- Build: **21s (MOD)**
- Life 100 | Armor 1 | Speed 2.25 | Attributes: Armored, Biological
- Weapon — Flamethrower (ground, cone splash): 8 dmg (+4 vs Light) ×1, period 1.4, range 2 → **DPS 5.7 (8.6 vs Light)**

**Marauder**
- Cost: 100 min / 25 gas — Supply 2 (base)
- Build: **28s (MOD)**
- Life 125 | Armor 1 | Speed 2.25 | Attributes: Armored, Biological
- Weapon — PunisherGrenades (ground): 10 dmg (+10 vs Armored) ×1, period 1.5, range 6 → **DPS 6.7 (13.3 vs Armored)**

**Sentry**
- Cost: 50 min / 100 gas — Supply 2 (base)
- Build: **21s (MOD)**
- Life 40 | Shields 40 | Armor 1 | Speed 2.5 (base voidmulti)
- Weapon — DisruptionBeam (ground + air): 6 dmg ×1, period 1, range ~5 → **DPS 6**

**Medic**
- Cost: 75 min / 50 gas — Supply 2 (base)
- Build: **26.6s (MOD)**
- Life 60 | Armor 1 | Speed 2.25 | Energy 50/200
- **No attack** (healer/caster)

### Slot 4

**Ghost**
- Cost: 150 min / 125 gas — Supply 3 (base; catalog Food -3)
- Build: **40s (base fallback — not mod-set)**
- Life 100 | Armor 0 | Speed 2.75 (base voidmulti) | Energy 75/200 | Attribute: Light
- Weapon — C10 Canister Rifle (ground + air): 15 dmg ×1 (no Light bonus), period 1.5, range 6 → **DPS 10.0**
- *Note: voidmulti sets a flat 15 (stock/liberty was 10 +10 vs Light) — same vs Light, +50% vs everything else.*

**Infestor**
- Cost: 100 min / 150 gas — Supply 2 (base)
- Build: **37.8s (MOD)**
- Life 90 | Armor 0 | Speed 2.25 | Energy 75/200
- Weapon — Acid Spores (ground only): 4 dmg ×1, period 1.754, range 6 → **DPS 2.3**
- *Note: this attack exists only because voidmulti (5.0.16) adds a WeaponArray; stock Infestor has no attack. Primarily a caster.*

**HighTemplar**
- Cost: 50 min / 150 gas — Supply 2 (base)
- Build: **42s (MOD)**
- Life 40 | Shields 40 | Armor 0 | Speed 2.0156 (base voidmulti) | Energy 50/200
- Weapon — Psi Blast (ground only): 4 dmg ×1, period 1.754, range 6 → **DPS 2.3** (primarily a caster: Feedback, Psi Storm)

---

## FACTORY

### Slot 1

**Vulture**
- Cost: **100 min (MOD)** / 0 gas — Supply 2 *(campaign base is 75 min)*
- Build: **21s (MOD)**
- Life 75 | Armor 1 | Speed 4.25 | Attributes: Light, Mechanical
- Weapon — Vulture (ground): 10 dmg **(+10 vs Light — MOD; campaign base is +15)** ×1, period 1.694, range 6 → **DPS 5.9 (11.8 vs Light)**
- *Has a native **KD8 Charge** — a thrown grenade (12 dmg, `KD8ChargeExplodeDamage` in mod `EffectData`; button on the card). **Not** a spider mine: the mod removes the mine — there are no traps anywhere in the mod.*

**Hellion**
- Cost: 100 min / 0 gas — Supply 2 (base)
- Build: **21s (MOD)**
- Life 90 | Armor 0 | Speed 4.25 | Attributes: Light, Mechanical
- Weapon — InfernalFlameThrower (ground, line splash): 8 dmg (+6 vs Light) ×1, period 2.5, range ~5 → **DPS 3.2 (5.6 vs Light)**

**Stalker**
- Cost: 125 min / 50 gas — Supply 2 (base)
- Build: **25s (MOD)** *(Factory slot; the Barracks Stalker roll is 25.2s)*
- Life 80 | Shields 80 | Armor 1 | Speed 2.9531 | Attributes: Armored, Mechanical | has Blink
- Weapon — ParticleDisruptors (ground + air): 13 dmg (+5 vs Armored) ×1, period 1.87, range 6 → **DPS 7.0 (9.6 vs Armored)**

### Slot 2

**Diamondback**
- Cost: 150 min / 150 gas — Supply 4 (base)
- Build: **38s (MOD)**
- Life 200 | Armor 1 | Speed 2.9531 | Attributes: Armored, Mechanical | fires while moving
- Weapon — Diamondback (ground): 20 dmg (+20 vs Armored) ×1, period 2, range 6 → **DPS 10 (20 vs Armored)**

**Immortal**
- Cost: 250 min / 100 gas — Supply 4 (base)
- Build: **72.8s (MOD)**
- Life 200 | Shields 100 | Armor 1 | Speed 2.25 | Attributes: Armored, Mechanical | Barrier
- Weapon — PhaseDisruptors (ground): 20 dmg ×1 (ArmorReduction 1), period 1.6, range 6 → **DPS 12.5**
- *Note: catalog shows flat 20 with no +Armored bonus anywhere in the chain (live SC2 commonly cites 20 +30 vs Armored) — 20 is the data value.*

**SiegeTank** *(multi-form)*
- Cost: 150 min / 125 gas — Supply 3 (base)
- Build: **45s (base fallback — not mod-set)**
- Life 175 (base voidmulti) | Armor 1 | Speed 2.25 (tank mode) | Attributes: Armored, Mechanical
- Weapon (tank mode) — 90mmCannons (ground): 15 dmg (+10 vs Armored) ×1, period 1.04, range 7 → **DPS 14.4 (24.0 vs Armored)**
- Weapon (**sieged** / combat form) — CrucioShockCannon (ground, splash): 40 dmg (+30 vs Armored) ×1, period 3, range 13 (min 2) → **DPS 13.3 (23.3 vs Armored)**

**WarHound**
- Cost: **175 min / 125 gas (MOD)** — Supply 3 *(base is 150/75)*
- Build: **72.8s (MOD)**
- Life 220 | Armor 1 | Speed 2.8125 | Attributes: Armored, Mechanical
- Weapon — WarHound (ground missile): 23 dmg ×1 (ArmorReduction 1), period 1.3, range 7 → **DPS 17.7** (has Haywire anti-mech missile)

**Archon**
- Cost: **225 min / 150 gas (MOD)** — Supply 4 *(base is 175/275)*
- Build: **70s (MOD)** *(Factory slot; the Barracks Archon roll is 45s)*
- Life 10 | Shields 350 | Armor 0 | Speed 2.8125 | Attributes: Psionic, Massive
- Weapon — PsionicShockwave (ground + air, splash): 25 dmg (+10 vs Biological) ×1, period 1.754, range 3 → **DPS 14.3 (20.0 vs Biological)**

**LurkerMP** *(attacks only while burrowed)*
- Cost: 150 min / 150 gas — Supply 3 (base)
- Build: **37.8s (MOD)**
- Life 190 (base voidmulti) | Armor 1 | Speed 3.375 | Attributes: Armored, Biological
- Weapon — LurkerMP (ground, burrowed, line splash): 20 dmg (+10 vs Armored) ×1, period 2, range 8 → **DPS 10 (15 vs Armored)**

**Goliath** *(air + ground)*
- Cost: 150 min / 50 gas — Supply 2 (base)
- Build: **43s (MOD)**
- Life 150 | Armor 1 | Speed 2.6875 | Attributes: Armored, Mechanical
- Weapon — GoliathG (ground): 18 dmg ×1, period 1.5, range 6 → **DPS 12**
- Weapon — GoliathA (air): 8 dmg (+8 vs Armored) ×2, period 1.5, range 6 → **DPS 10.7 (21.3 vs Armored)**

### Slot 3

**ThorAP** *(air + ground; anti-air precision variant)*
- Cost: 300 min / 200 gas — Supply 6 (base)
- Build: **67.2s (MOD)**
- Life 400 | Armor 1 | Speed 1.875 | Attributes: Armored, Mechanical, Massive
- Weapon — ThorsHammer (ground): 30 dmg ×2, period 1.28, range 7 → **DPS 46.9**
- Weapon — LanceMissileLaunchers (air, high-impact): 25 dmg (+10 vs Massive) ×1, period 1.28, range 11 → **DPS 19.5 (27.3 vs Massive)**

**Ultralisk**
- Cost: 275 min / 200 gas — Supply 6 (base)
- Build: **67.2s (MOD)**
- Life 500 | Armor 2 | Speed 2.9531 base → **3.375 (MOD: starts with AnabolicSynthesis)** | Attributes: Armored, Biological, Massive
- Weapon — KaiserBlades (ground, melee cleave): 35 dmg ×1, period 1, range ~1 → **DPS 35**
- *Note: mod grants AnabolicSynthesis at spawn (`Unit,Ultralisk,Speed +0.4219`, raising off-creep speed to the on-creep value); also has Frenzy.*

**Colossus**
- Cost: 300 min / 200 gas — Supply 6 (base)
- Build: **67.2s (MOD)**
- Life 250 | Shields 100 | Armor 1 (base voidmulti; liberty was 200/150) | Speed 2.25 | Attributes: Armored, Mechanical, Massive
- Weapon — ThermalLances (ground only, line splash): 10 dmg (+5 vs Light) ×2, period 1.5, range **9 (MOD)** → **DPS 13.3 (20.0 vs Light)**
- *Note: mod grants **Extended Thermal Lance** at spawn (+2 weapon range, base 7 → 9), like the Ultralisk's AnabolicSynthesis. No in-mod tooltip flags this yet → WA-048.*

---

## STARPORT

### Slot 1

**CorsairMP**
- Cost: 150 min / 100 gas — Supply 2 (base, Void)
- Build: **30.8s (MOD)**
- Life 120 | Shields 60 | Armor 1 | Speed 2.8125
- Weapon — NeutronFlare (air only, splash r0.5): 5 dmg ×1, period 0.472, range on launch effect → **DPS ~10.6** (anti-air only; has Disruption Web)

**Phoenix**
- Cost: 150 min / 100 gas — Supply 2 (base)
- Build: **30.8s (MOD)**
- Life 120 | Shields 60 | Armor 0 | Speed 4.25
- Weapon — IonCannons (air only): 5 dmg (+5 vs Light) ×2, period 1.1, range 5 → **DPS 9.1 (18.2 vs Light)** (Graviton Beam lifts ground units)

**Wraith**
- Cost: **100 min / 100 gas (MOD)** — Supply 2 *(campaign base is 150/150)*
- Build: **30.8s (MOD)**
- Life 140 | Armor 0 | Speed 3.75 | permanent cloak, Energy 50/200
- Weapon (air) — WraithA: 5 dmg (+5 vs Armored) ×2, period 1.25, range on launch effect → **DPS 8 (16 vs Armored)**
- Weapon (ground) — WraithG: 8 dmg ×1, period 1.694, range on launch effect → **DPS 4.7**

**VikingFighter** *(air fighter mode)*
- Cost: 125 min / 75 gas — Supply 2 (base)
- Build: **42s (base fallback — not mod-set)**
- Life 135 (base voidmulti) | Armor 0 | Speed 2.75 | Attributes: Armored, Mechanical
- Weapon — LanzerTorpedoes (air only): 10 dmg (+4 vs Armored) ×2, period 2.0, range 9 → **DPS 10 (14 vs Armored)**
- *AssaultMode transforms to a separate ground-attack walker (not covered here).*

### Slot 2

**Liberator** *(dual-mode)*
- Cost: 150 min / 125 gas — Supply 3 (base)
- Build: **49s (MOD)**
- Life 180 | Armor 0 | Speed 2.4 (accel to 3.375) | Attributes: Armored, Mechanical
- Weapon (default AA mode) — missile launchers (air only): 5 dmg ×2, period 1.8, range ~5 → **DPS ~5.6**
- Weapon (Defender/siege ground mode, `LiberatorAG`) — (ground only): 75 dmg ×1, period 1.6, range 10 → **DPS ~46.9**

**Mutalisk**
- Cost: 100 min / 100 gas — Supply 2 (base)
- Build: **20s (MOD)**
- Life 120 | Armor 0 | Speed 3.75
- Weapon — GlaiveWurm (air + ground, bounces 3×): 9 / 3 / 1 dmg, period 1.525, range 3 → **DPS ~5.9 single-target (~8.5 full bounce)**

**DuskWing** *(Banshee-derived merc)*
- Cost: **200 min / 150 gas (MOD)** — Supply 3 *(campaign base is 175/100)*
- Build: **39.2s (MOD)**
- Life 175 | Armor 0 | Speed 2.75
- Weapon (ground) — DuskWingBanshee: 18 dmg ×2, period 1.25, range 6 → **DPS ~28.8**
- Weapon (anti-air) — Hurricane Missile Pods (energy ability, Energy 75, range 7): **6 dmg (MOD) ×10 missiles = 60 flat vs air** *(campaign base was 4 +5 vs Light; mod flattens to 6 with Light bonus zeroed)*

**VoidRay**
- Cost: 250 min / 150 gas — Supply 4 (base)
- Build: **49s (MOD)**
- Life 150 | Shields 100 | Armor 0 | Speed 2.75 (base voidmulti)
- Weapon — VoidRaySwarm (air + ground): 6 dmg (+4 vs Armored) per hit, period 0.5, range 6 → **base DPS ~12 (+8 vs Armored)** before Prismatic escalation (damage ramps the longer it stays on one target)

### Slot 3

**Raven**
- Cost: 100 min / 150 gas — Supply 2 (base)
- Build: **48s (base fallback — voidmulti value)**
- Life 140 | Armor 1 | Speed 2.9492 (base voidmulti)
- **No attack** (detector/caster: Auto-Turret, Seeker/Shredder/Scrambler missiles)

**Tempest**
- Cost: 250 min / 175 gas — Supply 4 (base voidmulti)
- Build: **58.8s (MOD)**
- Life 200 | Shields 100 | Armor 2 | Speed 2.25 | Attributes include Massive
- Weapon — Tempest (air only): 30 dmg (+22 vs Massive) ×1, period 3.3, range 13 → **DPS 9.1 (13.1 vs Massive)**
- Weapon — TempestGround (ground only): 40 dmg ×1, period 3.3, range 10 → **DPS 12.1**

**Viper**
- Cost: 100 min / 200 gas — Supply 3 (base)
- Build: **39.2s (MOD)**
- Life 150 (base void) | Armor 1 | Speed 2.9531
- **No attack** (spellcaster: Abduct, Blinding Cloud, Consume)

**Battlecruiser**
- Cost: 400 min / 300 gas — Supply 6 (base)
- Build: **70s (MOD)**
- Life 550 | Armor 3 | Speed 1.875 | Attributes: Armored, Mechanical, Massive
- Weapon — ATA Laser Battery (air only): 5 dmg ×1, period 0.225, range 6 → **DPS 22.2**
- Weapon — ATS Laser Battery (ground only): 8 dmg ×1, period 0.225, range 6 → **DPS 35.6**

---

## Mod-overridden values (summary)

**Costs / supply (ForgeMod `UnitData.xml`):**
- **Marine** — +25 gas (50 / **25**), stock is 50 / 0
- **Firebat** — gas → **50** (100 / 50), campaign base 100 / 25
- **Queen** — gas → **50** and **supply 2 → 3** (175 / 50); minerals 175 is base
- **Vulture** — minerals **75 → 100** (100 / 0)
- **WarHound** — **150 / 75 → 175 / 125**
- **Archon** — **175 / 275 → 225 / 150**
- **DuskWing** — **175 / 100 → 200 / 150**
- **Wraith** — **150 / 150 → 100 / 100**

**Unit stats / behaviors (ForgeMod):**
- **Queen** — Speed → **1.8984** (`UnitData.xml`)
- **Ultralisk** — spawns with **AnabolicSynthesis** (speed 2.9531 → ~3.375), granted via upgrade/trigger init
- **Colossus** — spawns with **Extended Thermal Lance** (weapon range 7 → 9), granted via trigger init
- **Vulture** — spider mine removed; gains a native **KD8 Charge** thrown grenade (`AbilArray Link="KD8Charge"` + card button)

**Weapon-effect overrides (ForgeMod `EffectData.xml`):**
- **Vulture** (`VultureU`) — Light bonus **+15 → +10** (10 base dmg unchanged)
- **DuskWing** (`HurricaneMissileDamage`) — anti-air missile → **6 dmg**, Light bonus zeroed (6 × 10 missiles = 60 flat vs air)
- **Vulture** native **KD8 Charge** thrown grenade (`KD8ChargeExplodeDamage` = 12) — replaces the removed spider mine (mod has no traps).

**Build times (ForgeMod `AbilData.xml` train InfoArrays) — MOD unless noted:**
- Barracks: Zergling 5.6, Zealot 21, Hydralisk 26, Marine 15, Queen 32, Firebat 21, Marauder 28, Sentry 21, Medic 26.6, Infestor 37.8, HighTemplar 42; **Ghost 40 = base fallback**
- Factory: Vulture 21, Hellion 21, Stalker 25, Diamondback 38, Immortal 72.8, WarHound 72.8, Archon 70, LurkerMP 37.8, Goliath 43, ThorAP 67.2, Ultralisk 67.2, Colossus 67.2; **SiegeTank 45 = base fallback**
- Starport: CorsairMP 30.8, Phoenix 30.8, Wraith 30.8, Liberator 49, Mutalisk 20, DuskWing 39.2, VoidRay 49, Tempest 58.8, Viper 39.2, Battlecruiser 70; **VikingFighter 42 & Raven 48 = base fallback**

---

## Data caveats worth knowing

1. **Immortal** — the catalog carries a flat **20 damage with no `+Armored` bonus** in any dependency layer. If the mod expects the live "20 +30 vs Armored," it isn't in the data — 20 flat is what's there.
2. A handful of legacy/missile weapons (**CorsairMP** NeutronFlare, **Wraith** WraithA/WraithG, **Liberator** AA) store their **range on the launch effect** rather than on the `CWeapon`, so the weapon-line range reads "not set" rather than a true 0 — the effective range still applies in game.
