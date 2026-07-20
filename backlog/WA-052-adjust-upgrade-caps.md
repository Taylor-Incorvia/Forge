---
id: WA-052
status: todo
size: XS
phase: 1-game-readiness
priority: 20
---
# Adjust upgrade roll caps — Blink to 2

## What
Raise one per-upgrade roll cap to the default 2 (set in WA-049):
- **Blink**: 1 → 2
- **RavagerCorrosiveBile**: stays **1** (still one unit max — Corrosive Bile on two units is oppressive).
- **Speed / Range / Stimpack**: already 2 (the default) — no change.

## Why
Playtest feel: capping Blink at 1 was too restrictive; two units with Blink is fine. Corrosive Bile stays capped at 1.

## Implementation
`upgradeCapHelpers.galaxy` → `getUpgradeCap`: delete the single early-return line
```
if (upgradeId == "Blink") { return 1; }
```
It then falls through to the default `return 2`. **Keep** the `RavagerCorrosiveBile` line (cap 1), and the Concussive-family + caster-spell caps at 1.

## Acceptance criteria
- [ ] Blink can land on up to 2 of a player's units.
- [ ] Corrosive Bile, caster spells, and the Concussive family still capped at 1.
- [ ] Still no blank slots.
