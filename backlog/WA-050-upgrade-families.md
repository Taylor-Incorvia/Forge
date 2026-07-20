---
id: WA-050
status: done
size: S
phase: 1-game-readiness
priority: 14
---
# Upgrade families — treat per-unit variants of one concept as the "same upgrade"

## ✅ Implemented 2026-07-19 (branch wa-049-upgrade-caps)
`setUpgradeFamily`/`getUpgradeFamily` added to `upgradeHelpers.galaxy` (DataTable string map; an unregistered id defaults to its own id, so shared upgrades need no setup). Registered in `initializeUpgrades()`: all 12 concussive ids (`PunisherGrenades` + the 11 `Concussive*`) → `"ConcussiveShells"`; `LifestealMarine` → `"Lifesteal"`; `Range`/`SiegeTankRange`/`TempestRange` → `"Range"`. Consumed by WA-049's per-family cap counter. Typechecks clean.

## Why
Prerequisite for **[[WA-049]]** (per-upgrade roll caps). The mod has two shapes of upgrade, and the cap counter must treat them correctly:

1. **Shared upgrades** — ONE data id that lives in many unit pools (`Speed`, `Range`, `Blink`, the caster spells). Counting by id already works.
2. **Per-unit upgrades** — the SAME concept implemented as a DIFFERENT count-upgrade id **per unit**, because each modifies that unit's own weapon effect. E.g. Lifesteal = `LifestealMarine`, `LifestealQueen`, `LifestealVoidRay`, …; Concussive = `PunisherGrenades` (Marauder, stock), plus future `ConcussiveVoidRay`, `ConcussiveArchon`, … .

For WA-049, "no more than 2 units roll Lifesteal" must count **`LifestealMarine` + `LifestealQueen` + … as one family**. Without this, each per-unit variant is counted separately and the cap does nothing for them.

## What to build
A galaxy-side **upgrade family** mapping:
- `setUpgradeFamily(upgradeId, familyName)` — register a variant under a shared family (called in `initializeUpgrades`, next to each per-unit upgrade registration).
- `getUpgradeFamily(upgradeId)` — returns the registered family, or **the id itself** if none registered (so shared upgrades like `Speed`/`Blink` are each their own single-member family with zero setup).

Then WA-049 counts and caps on `getUpgradeFamily(upgrade)` instead of the raw id.

### Registrations (grow as per-unit upgrades are added)
- All `Lifesteal*` → family `"Lifesteal"`.
- Concussive variants (`PunisherGrenades` + any custom `Concussive*`) → family `"ConcussiveShells"`.
- **Range** — `Range` (the generic behavior upgrade) **+ `SiegeTankRange` + `TempestRange`** → family `"Range"`. The Tempest and Siege Tank got their own catalog range upgrades because a behavior couldn't update the range *visual* (WA-031), but conceptually all three are "+range," so they share one cap.
- Genuinely single-id shared upgrades (`Speed`, `Blink`, caster spells, etc.) → **no registration** (family defaults to their own id).

Note the family concept covers **two** reasons one concept becomes multiple data ids: (a) **per-unit** upgrades (Lifesteal, Concussive — each unit needs its own weapon-effect edit), and (b) **technical splits** (Range — Tempest/SiegeTank needed catalog upgrades for the visual). Both must be grouped.

## Acceptance criteria
- [ ] `getUpgradeFamily(upgradeId)` returns a canonical family for every rollable upgrade (defaulting to the id).
- [ ] Per-unit variants of one concept (all Lifesteals; all Concussives) share a family.
- [ ] WA-049's cap counting uses the family, so e.g. `LifestealMarine` + `LifestealQueen` count as 2 toward the Lifesteal cap.

## Notes
Small, self-contained (a registry + a getter). Do this **before / with** WA-049. The shared-upgrade shape needs nothing; only the per-unit stat/count upgrades (Lifesteal, Concussive, and any future per-unit upgrades) need a family registered. Sibling: [[WA-049]], [[WA-034]] (concussive per-unit expansion), [[WA-035]] (lifesteal per-unit expansion).
