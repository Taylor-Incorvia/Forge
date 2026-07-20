---
id: WA-053
status: todo
size: S
phase: 1-game-readiness
priority: 21
---
# Add Firebat + Stalker to the concussive-shells pool

## What
Two more units can roll **Concussive Shells** (slow-on-attack), following the WA-034 per-unit recipe:
- **Firebat** — Barracks slot 3 (research at Ghost Academy, column 2). `ConcussiveFirebat3`.
- **Stalker** — Factory slot 1 (research at Armory, column 0). `ConcussiveStalker1`.

Both join the `ConcussiveShells` family, so the cap-1 rule still means only one of your units gets concussive per game (see [[WA-049]] / [[WA-050]]).

## Per-unit recipe (mirror the 13 existing concussive units)
For each unit: marker `CUpgrade Concussive<Unit>` → `CountUpgradeConcussive<Unit>CompleteOnly` node → `UseConcussive<Unit>` requirement → `<Unit>ConcussiveResearched` validator → `<Unit>ConcussiveSlow` apply-behavior (with `NotStructure` + `NotFrenzied` + `NotMassive`) injected into the weapon effect; research UI `Concussive<Unit><slot>`; GameStrings; `addUpgradeToUpgrade` + `AnyOf <Unit>`; `setUpgradeFamily(..., "ConcussiveShells")` in `initializeUpgradeFamilies`.

## Injection points (need the weapon-effect trace, same as the other units)
- **Stalker** — `ParticleDisruptors`, single-target. Find its damage effect / set and inject the slow (like the Diamondback/Void Ray single-target pattern).
- **Firebat** — flame weapon, **splash** (cone). Inject into the AREA/search effect so all targets in the cone slow (like the Hellion). Confirm whether the Firebat shares `InfernalFlameThrower*` with the Hellion or has its own flame effect — if shared, gate carefully so only the researched unit slows.

## Acceptance criteria
- [ ] Firebat and Stalker each roll Concussive in their slot; researching it slows their attacks.
- [ ] Firebat slows all cone targets; Stalker slows its single target.
- [ ] Both in the ConcussiveShells family (cap 1 across the army).
- [ ] Massive targets immune; no purple-square icons.
