# Upgrade eligibility — the tag system

Each random upgrade is gated to specific units by **tag requirements**. When a unit occupies a slot, an upgrade is eligible for it only if the unit's tags satisfy that upgrade's requirements. Requirements are declared in `initializeUpgrades()` (`upgradeInitializers.galaxy`); the logic lives in `upgradeHelpers.galaxy` (`satisfiesUnitTagUpgradePoolRequirement`) + `listHelpers.galaxy` (`evaluateListLogicType`).

## Declaring a requirement
```galaxy
addUpgradeRequirementTag(upgrade, logicType, "unitTag", tag);
```
`logicType` ∈ `logicType_NoneOf` / `logicType_AllOf` / `logicType_AnyOf`. Call it repeatedly to add more tags — **all tags of the same logicType for an upgrade pool into one list.**

## The three logic types
Let **U** = the unit's tag set, **R** = the requirement's tag set (for one logicType):

| logicType | Passes when | Meaning |
|---|---|---|
| `AnyOf` | U ∩ R ≠ ∅ | unit has **at least one** of R — an **OR** across the tags |
| `AllOf` | R ⊆ U | unit has **all** of R — an **AND** |
| `NoneOf` | U ∩ R = ∅ | unit has **none** of R — exclusion |

- **Absent = pass.** If an upgrade declares no tags for a given logicType, that check is skipped (returns `true`).
- **Overall eligibility = all three checks pass:** `NoneOf` AND `AllOf` AND `AnyOf`, evaluated independently and ANDed.
- Because repeated `AnyOf` calls pool into one list, `AnyOf` is how you say "slot 3 **OR** slot 4 **OR** …" — each call adds another acceptable tag.

## Where a unit's tags come from

### Auto-tags — `addUnitToSlotPool(facility, slot, unitId)` adds 4
| Tag | Example: Viper in Starport slot 3 |
|---|---|
| `<facility><slot>` | `Starport3` |
| `<facility>` | `Starport` |
| `slot<slot>` | `slot3` |
| `<unitId>` | `Viper` — for per-unit exceptions |

Facility constants: `rax`=`"Barracks"`, `factory`=`"Factory"`, `starport`=`"Starport"`. So `rax +"4"` = `Barracks4`, `starport +"3"` = `Starport3`.

### Semantic tags — set manually in `initializeTags()` (`unitInitializers.galaxy`)
- `tagPureCaster(unitId)` → adds `caster` + `pureCaster`. Rolls caster/energy-ability upgrades, **excluded** from fighter buffs (Speed / Stim / Range / Blink).
- `tagHybridCaster(unitId)` → adds `caster` only. Rolls **both** caster spells and fighter buffs (e.g. Queen, DuskWing — has an energy bar *and* a real attack).
- `addUnitTag(unitId, tag)` → any custom tag.

## Common patterns
- **Only this unit can roll it:** `AnyOf unitTag "<Unit>"` — only that unit carries the `<Unit>` auto-tag. (e.g. `TempestRange` → `AnyOf "Tempest"`.)
- **Unit already has the ability, exclude it:** `NoneOf unitTag "<Unit>"`. (e.g. BlindingCloud → `NoneOf "Viper"`.)
- **Casters only:** `AllOf unitTag "caster"` — most `F_` spell upgrades.
- **Keep fighter buffs off pure casters:** `NoneOf unitTag "pureCaster"` — Range / Speed / Stim.
- **Restrict to certain slots:** one `AnyOf` per slot tag — e.g. `AnyOf starport+"3"` + `AnyOf rax+"4"` = "slot-3 casters or slot-4 casters."

## Worked example — `ForceField`
Sentry-family spell, restricted to casters, not the deep slots, and excluding the unit that already has it:
```galaxy
addUpgradeRequirementTag("ForceField", logicType_AllOf, "unitTag", "caster");       // casters only
addUpgradeRequirementTag("ForceField", logicType_NoneOf, "unitTag", starport +"3"); // not Starport slot 3
addUpgradeRequirementTag("ForceField", logicType_NoneOf, "unitTag", rax +"4");      // not Barracks slot 4
addUpgradeRequirementTag("ForceField", logicType_NoneOf, "unitTag", "Sentry");      // Sentry has it natively
```
A Sentry (Barracks slot 3) passes: has `caster` (AllOf ✓), is not in Starport-3 or Barracks-4 (NoneOf ✓), but is excluded by `NoneOf "Sentry"` → **not eligible** (correct, it already has Force Field). A different Barracks-slot-3 caster with `caster` would be eligible.
