---
id: WA-057
status: todo
size: S
phase: 1-game-readiness
priority: 25
---
# Phoenix: replace generic Range with Anion Pulse-Crystals

## What
Swap the generic `Range` on the **Phoenix** (Starport slot 1) for a flavored **Anion Pulse-Crystals** upgrade: same +2.5 range, but the proper Phoenix-range icon. Add it to the **Range family** (WA-050).

## Data (from research)
- Weapon `IonCannons` (Phoenix), Range 5 (LotV). Stock `<CUpgrade id="AnionPulseCrystals">` (void `upgradedata.xml`) does `Weapon,IonCannons,Range +2` only. We want **+2.5** to match the generic Range, so make our own catalog upgrade:
```xml
<CUpgrade id="AnionPulseCrystals">
    <EditorCategories value="Race:Protoss"/>
    <EffectArray Reference="Weapon,IonCannons,Range" Value="2.5"/>
    <AffectedUnitArray value="Phoenix"/>
</CUpgrade>
```
- **Icon** (reuse stock): `Assets\Textures\btn-upgrade-protoss-phoenixrange.dds` (`CButton id="AnionPulseCrystals"`, liberty `buttondata.xml`).

## Wiring
- `addUpgradeToUpgrade("AnionPulseCrystals","AnionPulseCrystals")` + `AnyOf Phoenix`; `addUpgradeRequirementTag("Range", NoneOf, Phoenix)`.
- `setUpgradeFamily("AnionPulseCrystals", "Range")` in `initializeUpgradeFamilies` (WA-050).
- `CAbilResearch`/`CButton` `AnionPulseCrystals1` (Starport slot 1 → Fusion Core col 0), GameStrings.

## ⚠️ Projectile color change — NOT free
You asked for the projectiles to change color. Research found **no color effect tied to `AnionPulseCrystals`** anywhere in the catalog (nothing in actordata/effectdata — the stock upgrade is range-only). So the color change is a **separate actor/model task**, not something the upgrade grants. Options: (a) ship range-only now, add color as a follow-up ticket; (b) find/author a projectile-tint actor keyed on the upgrade. Recommend (a) — decide before building the color piece.
