---
id: WA-004
status: todo
size: M
phase: 1-game-readiness
priority: 4
---
# Verify upgrade pools

Audit `initializeUpgrades()` so every unit's possible upgrade rolls are intentional — no dead upgrades, no accidental broken combos, no "already has this ability" leaks.

## Why
The upgrade pool is the wildcard. A single wrong tag makes something unfun (Tempest speed was "unbeatable, no counterplay" — already excluded) or broken. Several upgrades are commented as not-actually-working.

## Acceptance criteria
- [ ] For each registered upgrade, confirm its `logicType_AllOf/AnyOf/NoneOf` tags produce the intended eligible-unit set.
- [ ] Flag upgrades marked in comments as broken/not-working (e.g. `stalkerblinkrange`, `TempestDisruptionBlast`, `ImmortalRevive`, `PsionicShockwave`) — decide: fix, or delete so they can't roll. See WA-014.
- [ ] Confirm every `addAbilityToUpgrade` has a matching `NoneOf` for units that already have that ability (missing one = a wasted roll).
- [ ] Produce a unit → possible-upgrades matrix for the docs (feeds WA-008 and the Arsenal modal WA-001).

## Notes
The eligibility math is in `initializeUpgradePoolForPlayerSlot()` — a unit gets an upgrade only if it satisfies NoneOf AND AllOf AND AnyOf. Commented-out `addAbilityToUpgrade` calls are disabled and won't roll.
