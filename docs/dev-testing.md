# Dev testing reference

How to force specific game states for testing. All dev toggles live in `nativeHelpers.galaxy`. **Everything here must be reset/removed before committing.** (CLAUDE.md has the condensed version.)

## `devMode`
`bool devMode` (`nativeHelpers.galaxy`) gates all test hacks + `showMessage`. Toggle by swapping the comment so the alternative stays handy:
```galaxy
bool devMode = false;   // commit state
// bool devMode = true;
```

## `testCaseNumber` sweep — force every slot at once
`int testCaseNumber` (`nativeHelpers.galaxy`). `0` = normal random rolls. `1..7` forces **every** production slot to its Nth pool unit:

```
unit index in its pool = (testCaseNumber - 1) % poolSize
```

7 runs cover every unit (the largest pool is Factory slot 2 with 7 units). To find the number for a specific unit: `testCaseNumber = (its 0-based index in the slot pool) + 1`. The pools are defined in `initialize.galaxy` (`initialize<Facility>Slot<N>Pool`) — check there for current membership, it changes as units are added/removed.

Only active when `devMode` is also true. Reset to `0` for commit. The sweep code itself lives in `setRandomSlotUnitFromPoolForPlayer` and is inert when the flags are off — leave it.

## Force a single roll (unit or upgrade)
Two `devMode`-gated force points. Pattern: `if (devMode && facilityId == <fac> && slotIndex == N) { ... = "X"; }`. **Delete these lines before commit.**
- **Force a unit into a slot** — `setRandomSlotUnitFromPoolForPlayer` (`forgeProductionFacilityHeplers.galaxy`): set `selectedUnitId`.
- **Force an upgrade onto a slot** — `assignRandomUpgradeFromPoolToPlayerSlot` (`upgradeInitializers.galaxy`): set `selectedUpgrade`.

Combine with `testCaseNumber` to get a specific unit AND a specific upgrade in the same slot (e.g. `testCaseNumber=2` puts a Tempest in Starport s3, and a force there rolls `TempestRange` onto it).

## Production structure ↔ slots ↔ upgrade facility
Add-ons **gate the top slot** (they do NOT enable double production — see CLAUDE.md core mechanics):

| Prod facility | Slots | Add-on (unlocks top slot) | Rolled upgrades researched at |
|---|---|---|---|
| Barracks | 1–4 | Tech Lab → slot 4 | **Ghost Academy** |
| Factory | 1–3 | Reactor → slot 3 | **Armory** |
| Starport | 1–3 | Tech Reactor → slot 3 | **Fusion Core** |

Research-button column on the upgrade facility card = **slot − 1** (slot 1 → col 0, slot 2 → col 1, slot 3 → col 2, slot 4 → col 3).

## The local-editor render caveat (important)
Command-card buttons for **added / upgrade-unlocked abilities** (granted at runtime via `UnitAbilityAdd`) do **not** render in the local editor "Test Document" but **do** on published builds. "Publish to verify" applies to that class only. Train buttons, count/stat upgrades, and native abilities render fine locally.

Related gotcha: a card's `LayoutButtons` attribute that was *historically a different value* can render stale locally when omitted — set button `Row`/`Column` explicitly. (An attribute simply never set, with no prior value, defaults fine.) See `docs/static-prototype-attempt-1.md` and the `reference-sc2-editor-testing-constraint` memory for the full investigation.

## Cleanup checklist before commit
- [ ] `devMode = false`
- [ ] `testCaseNumber = 0`
- [ ] Removed any force-roll `if` blocks added to `setRandomSlotUnitFromPoolForPlayer` / `assignRandomUpgradeFromPoolToPlayerSlot`
- [ ] (The permanent `testCaseNumber` sweep block stays — it's inert with the flags off)
