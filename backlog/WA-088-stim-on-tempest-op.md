---
id: WA-088
status: done
size: S
phase: 1-game-readiness
priority: 70
---
# Stim on Tempest is OP — exclude Tempest from the Stimpack roll

## What (done)
Excluded Tempest from rolling the Stimpack upgrade. One line in `upgradeInitializers.galaxy`, matching the existing VoidRay/Archon exclusions:

```galaxy
addUpgradeRequirementTag("Stimpack", logicType_NoneOf, "unitTag", "Tempest");
```

## Root cause (confirmed in data)
`F_Stimpack` cost is `<VitalFraction index="Life" value="0.2"/>` — **20% of Life only, shields untouched.** On a shielded Protoss unit whose shields do the tanking and regen for free, stimming is near-free. On a **range-15 kiter** specifically, near-free +50% attack speed + +50% move speed = no counterplay (same failure mode that got Tempest excluded from the Speed upgrade, WA line 209).

## Why exclude, not rework
- Stim's shield-bypass is only *oppressive* on a long-range air kiter. On melee/short-range shielded units (Zealot, Stalker, Immortal) cheap stim is **fun, not broken** — Immortal-stim just does its job better; stim Zealots are an advertised launch feature.
- Exclusion list scales one unit at a time for free. A cost-rework (charge shields on stim) is a big, complex fix for a problem we only have evidence for on ONE unit — and would nerf the fun cases.
- If Immortal/Phoenix/Colossus stim ever *actually* proves oppressive in a real game, add them to the same NoneOf list — one line each.

## Notes
Tempest keeps its real upgrades (TempestRange, Hyperjump/Tactical Jump). Stim was never a designed Tempest upgrade — just a broken random roll. Deleting a bug, not a feature.

Design note (deferred): idea of a *new* Tempest upgrade (root-on-attack) rejected — a root on a range-15 kiter is the definition of no-counterplay. A DoT/burn variant is safer but Tempest is a notoriously hard unit to buff (Patches' mod adds upgrades to everything *except* Tempest). If pursued, scope as its own ticket with careful magnitude playtesting; do not fold into balance passes.
