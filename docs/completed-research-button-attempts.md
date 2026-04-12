# Completed Research Button Replacement — Attempts & Findings

## Goal
When a research ability completes on an upgrade facility (Ghost Academy/Armory/Fusion Core), replace the research button with a greyed-out placeholder so the player can't click it again.

## Approach 1: UnitAbilityRemove + UnitAbilityAdd (Swap)
**What:** Remove the completed research ability from the unit, add a placeholder ability in its place.

**Results:**
- `UnitAbilityRemove` causes the SC2 command card to **repack/shift** all remaining dynamically-added ability buttons. Researching slot 2 caused slot 3 to visually disappear from the first Ghost Academy because buttons shifted positions.
- This is the fundamental blocker: dynamically-added abilities don't respect `DefaultButtonLayout` column positions. They fill command card slots sequentially in the order added, and removing one causes all later ones to shift left.

## Approach 2: Full Rebuild (Remove All → Modify List → Re-add All)
**What:** Remove ALL dynamic abilities from the unit, modify the ability list (swap completed ability for placeholder), then re-add all abilities from the updated list.

**Results:**
- All abilities disappeared. `UnitAbilityAdd` for an ability that was just `UnitAbilityRemove`d **in the same game frame** doesn't work. The engine apparently doesn't process re-adding abilities that were removed in the same trigger execution.

## Approach 3: Remove All → Wait → Re-add (Split into two phases)
**What:** Remove all abilities first, modify the list, then re-add in a separate step.

**Results:**
- Same issue — removing then re-adding in the same trigger execution fails. We didn't try inserting a `Wait(0, c_timeGame)` between phases (which might have worked but would require the function to support thread yielding).

## Approach 4: Replacement Map (Don't modify list, swap per-unit)
**What:** Keep the ability list unmodified. Register a "replacement map" in DataTable. When `applyAbilityListToUnit` runs on new buildings, check the map and add the replacement instead of the original.

**Results:**
- Existing buildings: still used `UnitAbilityRemove` (swap), so the repack/shift issue persisted.
- New buildings: worked conceptually but the repack issue on existing buildings was still the blocker.

## Approach 5: UnitAbilityEnable
**What:** Instead of removing abilities, call `UnitAbilityEnable(u, abilId, false)` to grey out the button without removing it. No add/remove = no repack.

**Results:**
- **No effect.** `UnitAbilityEnable` does not work on dynamically-added abilities (added via `UnitAbilityAdd`), same as `UnitAbilityShow`.

## Approach 6: CatalogFieldValueSet (Modify ability data at runtime)
**What:** Use `CatalogFieldValueSet` to change the research ability's cost/requirements per-player at runtime. No add/remove needed.
 
**Why rejected (before testing):**
- Research abilities are **shared across facility types**. "Blink1" can appear on both Ghost Academy and Fusion Core. Modifying "Blink1" via CatalogFieldValueSet would affect ALL facilities for that player, not just the one that completed the research.

## Approach 7: Marker Behaviors + XML Requirements on InfoArray
**What:**
- Create hidden marker behaviors (`CompletedSlot1Marker` through `4`)
- Create requirements that check for these behaviors at the unit (per-unit scoping via `CompleteOnlyAtUnit`)
- Add `Requirements="HideIfSlotNComplete"` to every research ability's `InfoArray` in AbilData.xml
- When research completes, grant the marker behavior to all facilities of that type
- The requirement evaluates per-unit: Ghost Academy (has marker) → button hidden/greyed. Fusion Core (no marker) → unaffected.

**Results:**
- Debug messages confirmed the Galaxy code ran and the behavior name was correct.
- **No effect on the buttons.** The `Requirements` attribute on `CAbilResearch.InfoArray` appears to be **silently ignored** by the SC2 engine. This is likely not a valid field location — the SC2 data schema may not support `Requirements` at the InfoArray level for `CAbilResearch`, or it may only work for statically-defined abilities.
- We tried both `Show` nodes (hide button) and `Use` nodes (grey out button) — neither had any effect.

## What We Know Doesn't Work on Dynamically-Added Abilities
| Function/Mechanism | Effect |
|---|---|
| `UnitAbilityRemove` | Causes command card repack/shift |
| `UnitAbilityShow` | No effect |
| `UnitAbilityEnable` | No effect |
| `InfoArray.Requirements` | Silently ignored |
| `CatalogFieldValueSet` | Works but can't scope per-facility (shared abilities) |

## Untested Ideas
1. **`CmdButtonArray` with Requirements** — Instead of `InfoArray.Requirements`, add `<CmdButtonArray index="Research1" Requirements="..."/>` to each `CAbilResearch`. This is how `CAbilEffectTarget` abilities define requirements. Might be the correct XML location.

2. **`Wait(0, c_timeGame)` between remove-all and re-add-all** — Inserting a game-frame delay might allow the engine to process removals before re-adding. Requires the trigger context to support thread yielding.

3. **Per-facility CatalogFieldValueSet + unit-type requirement** — Use CatalogFieldValueSet to set a requirement that combines a per-facility-per-slot upgrade check WITH a unit-type-at-location check. E.g., "grey out if CompletedGASlot1 AND unit is GhostAcademy". The upgrade is global per-player but the unit-type check scopes it to the facility. Complex requirement chain (12 upgrades, AND/OR/NOT nodes).

4. **Cancel-on-start trigger** — Don't modify the button at all. Instead, add a trigger that fires when research starts, checks if that slot is already completed, and immediately cancels + refunds. Button stays visible but research is wasted. Poor UX.

5. **Pre-create facility-specific ability copies** — Create "Blink1_GA", "Blink1_Armory", "Blink1_FC" variants so CatalogFieldValueSet can target per-facility. Massive XML duplication.
