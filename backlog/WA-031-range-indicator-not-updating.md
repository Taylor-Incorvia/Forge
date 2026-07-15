---
id: WA-031
status: done
size: M
phase: 1-game-readiness
priority: 26
---
# Range indicator doesn't update when the Range upgrade is rolled

When a unit rolls the **Range** upgrade (increased attack range), its **range indicator** — the ring of triangles at the edge of the unit's weapon range — keeps showing the OLD range instead of the new, larger one. Confusing: the unit outranges what the indicator claims.

## Where the indicator shows
- **Tempest** — always shows its range indicator.
- **Siege Tank** — shows the indicator when sieged.

So those two are the visible cases; the desync is most obvious there.

## 🔍 Findings (confirmed 2026-07-14)
- The indicator is drawn by two **stock `CActorRange`** actors — **`TempestRange`** (`<Weapon value="Tempest"/>`, swarm actordata) and **`SiegeTankSiegedRange`** (liberty/swarm). The mod does NOT override them. They're created on `SelectionLocalUpdate.<unit>.Start` and draw the bound **weapon's** range.
- The Range upgrade is `CBehaviorBuff id="Range"` with `<Modification WeaponRange="2.5"/>` (Permanent, Hidden) — a **per-unit effective-range modifier**, not a catalog change.
- **Root cause:** `CActorRange`/`<Weapon>` draws the weapon's **base catalog range**; the behavior's per-unit `WeaponRange` modifier isn't reflected → the ring stays at base while the real range grows.

## Fix options (none are a clean quick win — this is why it's size M)
1. **Small experiment (uncertain):** mod-override `TempestRange` + `SiegeTankSiegedRange` to also fire `Send="Create"` on `Behavior.Range.On`. Works ONLY if `CActorRange` re-queries the *effective* range on creation; if it reads catalog range (likely), no effect. Needs an in-game test to know.
2. **Rework Range from behavior → catalog weapon-range upgrade** (a `CUpgrade` modifying `Weapon,<id>,Range`). Then the indicator reflects it — but it becomes per-unit + player-wide, i.e. the same per-weapon grind as WA-034/WA-035. Bigger.
3. **Accept the cosmetic desync** — the unit works, only the triangle ring is off.

Verdict: not a brainless pickup. Option 1 is worth one quick in-game test; if it fails, it's option 2 (real work) or option 3 (wontfix).

## ✅ Done (2026-07-14) — verified in-game (ring now tracks the upgraded range)
Two new **count upgrades** replace the behavior Range for these units, so the *catalog* weapon Range changes and the `CActorRange` indicator follows it:
- **`TempestRange`** (Starport slot 3 → research `TempestRange3`): +2.5 to `Weapon,Tempest,Range` (air) + `Weapon,TempestGround,Range`.
- **`SiegeTankRange`** (Factory slot 2 → research `SiegeTankRange2`): +2.5 to `Weapon,90mmCannons,Range` (tank mode) + `Weapon,CrucioShockCannon,Range` (sieged).
- Behavior `Range` now has `NoneOf Tempest` + `NoneOf SiegeTank`, so those units roll the count version instead.
- Files touched: UpgradeData (2 `CUpgrade`), AbilData (2 `CAbilResearch`), ButtonData (2 `CButton`, reuse Grooved Spines icon), GameStrings (6 lines), `upgradeInitializers.galaxy` (registration + NoneOf). Follows the stock `ExtendedThermalLance` weapon-range pattern + the mod's `HighCapacityBarrels` count-upgrade wiring.

**Test:** `devMode=true` + `testCaseNumber=2` (Tempest) or `=3` (SiegeTank). A dev force in `assignRandomUpgradeFromPoolToPlayerSlot` auto-rolls the range upgrade onto those slots. Research at **Fusion Core** (Tempest) / **Armory** (SiegeTank), then confirm the triangle ring grows to the new range. Remove the force + reset devMode/testCaseNumber after.

Confidence: weapon-range via catalog is the proven ExtendedThermalLance path (unlike `stalkerblinkrange`, an *ability*-range upgrade that reportedly never worked). Main risk is a wrong weapon id — if one weapon doesn't grow, that `EffectArray` reference is off.

## Acceptance criteria
- [ ] Find what drives the range indicator (likely a unit/weapon "range indicator" actor or display-range field) and why it doesn't reflect the `Range` behavior modifier.
- [ ] Make the indicator reflect the post-upgrade range for units that roll Range (esp. Tempest + sieged Siege Tank).
- [ ] Verify in game: roll Range on a Tempest / Siege Tank and confirm the triangle ring matches the real (increased) range.

## Notes
Clarity feature, same family as the stim indicator (WA-026) and ability telegraph (WA-030). `Range` upgrade registration is in `upgradeInitializers.galaxy`; `reference/` has the stock Tempest / SiegeTank / weapon + range-indicator actor wiring to inspect.
