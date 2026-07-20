---
id: WA-055
status: done
size: S
phase: 1-game-readiness
priority: 23
---
# Goliath-specific range upgrade (+3 air / +1 ground) — replaces Range for Goliath

## ✅ Done 2026-07-20 (PR)
Custom `GoliathRange` catalog upgrade (mirrors `SiegeTankRange`): +3 to `GoliathA`/`GoliathAUpgraded` range, +1 to `GoliathG`/`GoliathGUpgraded` (verified the Goliath's WeaponArray uses all four). Rolls at Factory slot 2 → Armory col 1 (`GoliathRange2`); generic `Range` now `NoneOf Goliath`. **Open decision resolved:** added to the **Range family** (shares the range cap with Range/SiegeTankRange/TempestRange) — consistent with the others; revisit if the shared cap-2 feels tight in playtests.

## What
Replace the generic `Range` (+2.5 all weapons) on the **Goliath** (Factory slot 2) with a Goliath-flavored upgrade matching the WoL campaign **Ares-Class Weapons System**: **+3 air-weapon range, +1 ground-weapon range**. New custom catalog upgrade `GoliathRange`.

## Data (from research)
Goliath weapons (campaign `weapondata.xml`): **`GoliathA`** (air, Range 6, `DisplayAttackCount 2`), **`GoliathG`** (ground, Range 6), plus hidden `GoliathAUpgraded`/`GoliathGUpgraded` (Range 6). Stock reference: `<CUpgrade id="AresClassWeaponsSystem">` (campaign `upgradedata.xml`) does exactly `GoliathA/GoliathAUpgraded Range +3`, `GoliathG/GoliathGUpgraded Range +1`.

Mirror `TempestRange`/`SiegeTankRange`:
```xml
<CUpgrade id="GoliathRange">
    <EditorCategories value="Race:Terran"/>
    <EffectArray Reference="Weapon,GoliathA,Range" Value="3"/>
    <EffectArray Reference="Weapon,GoliathAUpgraded,Range" Value="3"/>
    <EffectArray Reference="Weapon,GoliathG,Range" Value="1"/>
    <EffectArray Reference="Weapon,GoliathGUpgraded,Range" Value="1"/>
    <AffectedUnitArray value="Goliath"/>
</CUpgrade>
```

## Wiring
- `addUpgradeToUpgrade("GoliathRange","GoliathRange")` + `AnyOf Goliath`; `addUpgradeRequirementTag("Range", NoneOf, Goliath)`.
- `CAbilResearch`/`CButton` `GoliathRange2` (Factory slot 2 → Armory col 1), GameStrings, icon.

## Open decision
Add `GoliathRange` to the **Range family** (WA-050) so it shares the range cap of 2 with Range/SiegeTankRange/TempestRange/AnionPulseCrystals? Consistent, but the shared cap-2 across 5 range variants may feel tight — could give it its own family instead. **Decide during impl / ask.**
