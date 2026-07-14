---
id: WA-031
status: todo
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

## Likely cause (to confirm during investigation)
The mod's Range upgrade grants a **behavior** (`addBehaviorToUpgrade("Range", "Range")`) that modifies weapon range at runtime. The range-indicator visual is probably bound to a **static/base range value** (the weapon's or unit's display range) that the behavior modifier doesn't touch — so the actual range grows but the indicator doesn't.

## Acceptance criteria
- [ ] Find what drives the range indicator (likely a unit/weapon "range indicator" actor or display-range field) and why it doesn't reflect the `Range` behavior modifier.
- [ ] Make the indicator reflect the post-upgrade range for units that roll Range (esp. Tempest + sieged Siege Tank).
- [ ] Verify in game: roll Range on a Tempest / Siege Tank and confirm the triangle ring matches the real (increased) range.

## Notes
Clarity feature, same family as the stim indicator (WA-026) and ability telegraph (WA-030). `Range` upgrade registration is in `upgradeInitializers.galaxy`; `reference/` has the stock Tempest / SiegeTank / weapon + range-indicator actor wiring to inspect.
