---
id: WA-029
status: done
size: S
phase: 1-game-readiness
priority: 25
---
# Update Archon price (~225/150)

## ✅ DONE (2026-07-13)
`UnitData.xml` `<CUnit id="Archon">` set to Minerals 225 / Vespene 150 (was 150/250). No test needed per user.

Re-price the Archon to roughly **225 minerals / 150 gas**.

## Why
Current cost is **150 / 250** (mineral-light, very gas-heavy). Shifting to ~225/150 makes it more mineral-weighted and less gas-gated — tune to taste from there.

## Acceptance criteria
- [ ] `UnitData.xml` `<CUnit id="Archon">`: set `CostResource Minerals` = 225, `CostResource Vespene` = 150.
- [ ] Verify in game (unit cost is locally testable).

## Notes
Archon is Factory slot 2 (`initialize.galaxy`), produced directly (not merged from templar). Simple two-field change; current values are Minerals 150 / Vespene 250.
