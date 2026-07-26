---
id: WA-070
status: todo
size: M
phase: 1-game-readiness
priority: 30
---
# Scout enemy rolls — floating unit icons above production structures

## Why
Scouting should reveal *what an opponent can build*, the RTS-native way: fly an SCV over their base, look at a Barracks, and see the four units it rolls floating above it. This is the opponent-facing half of the roll-visibility question — deliberately **not** dumping the enemy's roll into the "Your Faction" modal (too much to parse at 0:00). You earn the info by scouting, and vision gates it for free. Sibling to the Faction epic (WA-001 / WA-039).

## Feasibility — confirmed native support (this is the good news)
The data is trivial and every hard display problem has a native:

**Data (per-player, already stored):**
- Rolls live in data tables keyed by facility *type* + slot + player. `getPlayerSlotUnitId(facilityType, slot, ownerPlayer)` returns the unit for each slot; `getSlotSelectedUpgrade(ownerPlayer, slot, facility)` returns each slot's rolled upgrade.
- **Rolls are per-player, not per-building** — every Barracks that player owns builds the same four units. Scouting one reveals the whole category.

**Display (`natives.galaxy`, via the already-included `NativeLib`):**
- `TextTagCreate(text, fontSize, point, heightOffset, show, useFogofWar, playergroup)` — creation already takes **who sees it** (playergroup) and **fog gating** (useFogofWar).
- `TextTagAttachToUnit(tag, unit, heightOffset)` — the tag follows the structure.
- `TextTagFogofWar(tag, bool)`, `TextTagShow(tag, playergroup, bool)`, `TextTagDestroy(tag)`, `TextTagSetColor`, `TextTagSetBackgroundImage` — full lifecycle + styling.
- Inline icons: SC2 text supports `<img path="…" width="16" height="16" alignment="absolutemiddle"/>` (confirmed in stock tooltips). Four `<img>` in one tag = four unit icons.

**Net-new:** the mod has **zero** existing UI/text-tag code today, so this is built from scratch — but on solid native ground.

## The mapping that makes it clean
- Show it to the **opponent(s)**, not the owner (owner already knows their roll): `playergroup` = enemies of the structure's owner.
- `useFogofWar = true` → each opponent sees it **only when they currently have vision** of the structure. That IS the "scout it to learn it" gate, for free. (Confirm the hide-on-vision-loss behavior in play — see risks.)
- Each building reveals **its own** roll: a Barracks shows its 4 units; a Ghost Academy could show the rolled *upgrades*. Keeps each tag small.
- **1v1 is the target** (the user's framing). FFA/team fog-per-player-in-a-group is a fuzzier case — note it, don't block on it.

## Staged prototype plan (de-risk in order)
1. **Data + trigger path (names only).** On an enemy production structure being seen/created, create a text tag attached to it, shown to the opponent, listing the rolled unit **names** as plain text. Proves the read + attach + per-player + fog-gate all work.
2. **Swap names → icons.** Resolve each unit's icon path (via `CatalogFieldValueGet(c_gameCatalogButton, <trainButton>, "Icon", player)` or a static unitId→icon map) and build the `<img/>` markup. Confirm inline `<img>` renders in a *TextTag* (it does in tooltips; verify for tags) and nail the dds path format.
3. **Lifecycle + vision.** Confirm fog gating hides the tag when vision is lost; handle structure death (`TextTagDestroy` on unit-removed). One tag per production building.
4. **Tune presentation.** Decide always-on-when-visible vs on-select; units-only vs units+upgrades; clutter in multi-Barracks bases.

## Design decisions to settle (in the prototype)
- **Always-on-when-visible vs on-select.** Always-on is the dream ("just see it"); four icons over every building can clutter a 6-Barracks base. Per-player rolls mean one representative building could suffice.
- **Units only, or + upgrades.** Units-only is tidy. Upgrades live on the tech building, so letting each building show its own roll keeps tags small.
- **Add-on-gated top slot.** Barracks s4 / Factory s3 / Starport s3 need an add-on to actually build. Show all rolled slots (full potential) or mark the gated one? Minor.

## Risks / to confirm in-editor
- `<img>` markup rendering in a **TextTag** specifically (confirmed in tooltips; very likely, verify).
- Exact dds path format inside `<img path="…"/>` (raw `Assets\Textures\*.dds` vs an `@`-alias).
- `useFogofWar` semantics — does the tag truly hide when the viewing player loses vision of the structure? (Core to the "scout to learn" gate.)
- Trigger wiring (CLAUDE.md split): the user wires editor triggers for "unit created / enters play" and "unit dies"; this ticket provides the galaxy functions they call (e.g. `onProductionStructureCreated()` / `onProductionStructureRemoved()` reading `EventUnit()`).
- **Publish-to-verify** likely (UI/vision behavior won't show in the local Test Document).

## Acceptance criteria
- [ ] When the local player has vision of an enemy production structure, its rolled units appear as floating **icons** above it.
- [ ] The tag is visible **only to the opponent** (not the structure's owner) and **only while that player has vision** (disappears in fog).
- [ ] Reads live per-player roll data (matches what that player's structures actually build).
- [ ] Cleans up on structure death (no orphan tags).
- [ ] Verified on a **published** build.

## Notes
Rejected sibling approach (Option C): forcing the enemy's greyed command card to show on selection — SC2 hides enemy command cards by design; no clean data toggle found. Option A (this) chosen. Option B (dialog panel on selection) is the fallback if always-on clutter proves bad. Part of the roll-visibility / "Your Faction" family (WA-001, WA-039).
