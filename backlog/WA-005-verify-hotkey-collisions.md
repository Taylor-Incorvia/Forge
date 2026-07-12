---
id: WA-005
status: todo
size: M
phase: 1-game-readiness
priority: 5
---
# Verify hotkey collisions

Make sure no unit or ability you've added collides on a hotkey with something else reachable from the same production facility. This is about *collisions*, not missing hotkeys.

## Why
Because units are randomly slotted into the same building, two different units (or a unit's ability and a researchable upgrade) can end up sharing a key on the same command context. That silently breaks control for a new player who won't know why their key "doesn't work."

## The two collision classes to check
1. **Unit-vs-unit within a facility.** Units built from the same production facility but in different slots must not share a build hotkey.
   - Example: Vulture is Factory slot 1 on hotkey `v` by default. If a Factory slot-2 unit is also on `v`, that's a collision.
2. **Caster default ability vs. researchable-upgrade hotkey `G`.** Researchable upgrades all use hotkey `G` (Row 2 / Col 3). Any spellcaster whose *default* ability sits on `G` collides with a rolled research.
   - Example: Sentry's Guardian Shield defaults to `G` — it had to be moved to `E` to avoid conflicting with a researchable upgrade.

## Acceptance criteria
- [ ] For each production facility (Barracks, Factory, Starport), list every unit that can roll into it and its build hotkey; flag any key shared by two units.
- [ ] For every caster in the pool, list its default ability hotkeys; flag any default ability on `G`.
- [ ] Reassign the colliding hotkeys (move the caster ability off `G`, à la Sentry → `E`; or rekey the colliding unit) and update `GameHotkeys.txt` accordingly.
- [ ] Re-verify no facility has a duplicate key after fixes.

## Notes
Unit → slot assignments live in `initialize.galaxy` (which units roll into which facility/slot). Ability hotkeys live in `ButtonData.xml` + `GameHotkeys.txt`. The known-good precedent is Sentry Guardian Shield moved G → E.
