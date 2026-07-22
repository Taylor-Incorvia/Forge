---
id: WA-057
status: done
size: S
phase: 1-game-readiness
priority: 25
---
# Phoenix: replace generic Range with Anion Pulse-Crystals

## ✅ Done 2026-07-20 (PR)
Reused the stock `AnionPulseCrystals` id, overriding its range to **+2.5** (stock +2). Phoenix now rolls it instead of generic `Range` (`Range` → `NoneOf Phoenix`), at Starport slot 1 → Fusion Core col 0 (`AnionPulseCrystals1`, stock phoenix-range icon). Added to the **Range family**. **Verify in-game:** the beam should turn purple (that recolor rides on the stock upgrade's own actor — granting the real id is what triggers it).

## What
Swap the generic `Range` on the **Phoenix** (Starport slot 1) for the flavored **Anion Pulse-Crystals** upgrade: same +2.5 range, the proper Phoenix-range icon, and — per known MP behavior — the Phoenix beam turns **purple instead of blue**. Add it to the **Range family** (WA-050).

## Reuse the STOCK upgrade id (so the purple recolor rides along)
In standard multiplayer, Anion Pulse-Crystals recolors the Phoenix attack purple. That recolor is tied to the **stock `AnionPulseCrystals` upgrade** completing (research didn't surface a standalone color effect in the extracted data, but the recolor is real in MP — it comes with the stock upgrade). So **reuse the stock `AnionPulseCrystals` id** rather than inventing a new one, and just **override its range to +2.5** (stock is +2):
```xml
<CUpgrade id="AnionPulseCrystals">
    <EffectArray index="0" Reference="Weapon,IonCannons,Range" Value="2.5"/>
</CUpgrade>
```
Overriding the existing entry keeps whatever actor/model swap the stock upgrade triggers, so the purple projectiles should follow for free. (Stock already targets the Phoenix + carries icon `Assets\Textures\btn-upgrade-protoss-phoenixrange.dds`.)

## Wiring
- `addUpgradeToUpgrade("AnionPulseCrystals","AnionPulseCrystals")` + `AnyOf Phoenix`; `addUpgradeRequirementTag("Range", NoneOf, Phoenix)`.
- `setUpgradeFamily("AnionPulseCrystals", "Range")` in `initializeUpgradeFamilies` (WA-050).
- `CAbilResearch`/`CButton` `AnionPulseCrystals1` (Starport slot 1 → Fusion Core col 0), GameStrings.

## Acceptance criteria
- [ ] Phoenix rolls Anion Pulse-Crystals instead of Range; +2.5 range applies (range indicator updates).
- [ ] Phoenix projectiles turn purple after research — **verify in-game** (expected free from the stock upgrade; if it doesn't, the recolor becomes a small follow-up actor task).
- [ ] In the Range family (shares the range cap).
