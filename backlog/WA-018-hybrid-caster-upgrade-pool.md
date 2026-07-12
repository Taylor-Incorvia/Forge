---
id: WA-018
status: todo
size: L
phase: 1-game-readiness
priority: 12
parent: WA-002
---
# Let hybrid units roll BOTH caster and non-caster upgrades

Introduce a "hybrid" path in the upgrade-pool system so units that have **both an energy bar and an attack** (Queen, DuskWing) can draw from both the caster and non-caster upgrade pools.

## Why
Today the pool splits cleanly in two and the halves are mutually exclusive: caster upgrades require the `caster` tag (`logicType_AllOf`), non-caster upgrades forbid it (`logicType_NoneOf`). A unit is either/or. Units that both cast and fight can't currently draw from both, which is exactly what the Queen and DuskWing need.

## ⭐ Do before WA-019 and WA-020 — it unblocks both.

## Acceptance criteria
- [ ] Document how caster vs non-caster eligibility is currently decided (`upgradeInitializers.galaxy` + `unitInitializers.galaxy` tagging; eligibility math in `initializeUpgradePoolForPlayerSlot()`).
- [ ] Design a mechanism (e.g. a new `hybrid` tag, or decoupling "can cast energy abilities" from "has an attack") so a hybrid unit is eligible for both pools.
- [ ] Pure casters and pure fighters are **unchanged**.
- [ ] No broken rolls — a hybrid must not roll an upgrade it can't actually use.
- [ ] Mechanism is general (works for any future hybrid), with Queen + DuskWing as the first two targets.

## Notes
Eligibility rule today: a unit gets an upgrade only if it satisfies NoneOf AND AllOf AND AnyOf on its unit tags. The `caster` tag is the pivot to rethink.
