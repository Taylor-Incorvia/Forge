---
id: WA-020
status: todo
size: S
phase: 1-game-readiness
priority: 14
parent: WA-002
depends_on: WA-018
---
# Cloak as an upgrade for Wraith / DuskWing / Ghost + Wraith cost cut

## Direction (simplified & decided 2026-07-15)
Make cloak a **rollable count upgrade** for all three cloak-capable units, using the **stock SC2 cloak upgrades** (already implemented) — so it's just count-upgrade wiring per CLAUDE.md, no new data mechanic. Plus a Wraith cost cut. Dropped the speed-while-cloaked bonus to keep it simple (revisit later only if plain cloak feels like a weak roll).

| Unit | Slot | Stock cloak upgrade | Research id | Research at |
|---|---|---|---|---|
| Wraith | Starport s1 | `WraithCloak` | `WraithCloak1` | Fusion Core (col 0) |
| DuskWing | Starport s2 | `BansheeCloak` | `BansheeCloak2` | Fusion Core (col 1) |
| Ghost | Barracks s4 | `PersonalCloaking` | `PersonalCloaking4` | Ghost Academy (col 3) |

Plus for the **Wraith only**:
- **Reduce cost** to ≤ Mutalisk (100/100). Currently ~150/100 (stock, not overridden). *(Pick the exact number.)*
- **Remove the start-with-cloak grant** (`grantUpgrade(player, "WraithCloak")`, `upgradeInitializers.galaxy:285`) so cloak becomes a roll, not a default.

DuskWing cost unchanged (200/150). These cloak upgrades gate **native** cloak abilities (already on the units' cards), so no added-ability-button local-render concern.

## Confirmed current state
- Wraith: ~150/100 (stock, not overridden), **starts with** `WraithCloak` (`upgradeInitializers.galaxy:285`).
- DuskWing: 200/150 (mod override), no starting cloak.
- Mutalisk: 100/100 (the cost target ceiling).

## Acceptance criteria
- [ ] Cloak count upgrades wired for Wraith (`WraithCloak1`), DuskWing (`BansheeCloak2`), Ghost (`PersonalCloaking4`) — galaxy reg + tag + `CAbilResearch` + `CButton` + GameStrings each.
- [ ] Wraith cost reduced to ≤ 100/100 **and** its start-with-cloak grant removed (`upgradeInitializers.galaxy:285`).
- [ ] In-game: each unit can roll cloak and it works after research; Wraith is worth building at the new cost.

## Notes
Cloak icons: reuse each stock cloak upgrade's own icon (no new art). The cost cut is a trivial standalone edit; the three cloak upgrades are mechanical count-upgrade wiring (same shape as WA-031). Small ticket overall. Verify the stock upgrade ids in the reference during build (`WraithCloak`/`BansheeCloak` confirmed in `upgradeInitializers.galaxy:285/288`; `PersonalCloaking` to confirm).
