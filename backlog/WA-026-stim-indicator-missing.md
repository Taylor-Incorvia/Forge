---
id: WA-026
status: todo
size: M
phase: 1-game-readiness
priority: 22
---
# Stim indicator doesn't show on units that don't normally stim

When a rolled **Stimpack** upgrade is applied to a unit that doesn't natively use stim (e.g. a Queen, Void Ray, etc. via `F_Stimpack`), the visual "stimmed" indicator/overlay above the unit does not appear. The unit IS stimmed (gets the speed/attack boost), but there's no telegraph.

## Why
Legibility — both players should be able to see that a unit is stimmed. A hidden buff on a non-standard unit is confusing and hard to play around.

## Acceptance criteria
- [ ] Find how the stim indicator is driven — it's an actor event tied to the stim behavior/ability, wired on the Marine/Marauder actors that normally have it.
- [ ] Make the indicator show for ANY unit that receives the rolled `F_Stimpack` (likely a shared actor/actor-event, or attaching the indicator to the stim behavior itself rather than per-unit actors).
- [ ] Verify: stim a non-standard unit (e.g. Queen) and confirm the overlay appears.

## Notes
Discovered while reviewing the Queen's rollable pool (WA-019) — the Queen (and other hybrids) can roll Stimpack. `reference/` has the Marine/Marauder actor + stim behavior wiring to model against.
