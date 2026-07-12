---
id: WA-020
status: todo
size: S
phase: 1-game-readiness
priority: 14
parent: WA-002
depends_on: WA-018
---
# Decide desired cloak behavior for Wraith and DuskWing

Figure out what cloak *should* do for the Wraith and the DuskWing — start-with-cloak, roll-for-cloak, or no default cloak + hybrid-caster upside — then implement whatever we land on.

## Why
Current state is inconsistent and worth a deliberate decision: the Wraith starts with `WraithCloak`, while `BansheeCloak` for the DuskWing is already commented out. Before touching either, decide the intended design rather than patching ad hoc.

## The decision to make
- Should Wraith / DuskWing start cloaked, have cloak in their roll pool, or neither?
- Option on the table: DuskWing (and maybe Wraith) start **without** default cloak and instead become **hybrid casters** (roll from both caster and non-caster pools, per WA-018) — trading guaranteed cloak for wildcard upside.

## Acceptance criteria
- [ ] Confirm the current cloak state of both units in code (`grantInitialUpgrades()` and any relevant defaults).
- [ ] Decide the intended cloak design for each, and write down the reasoning.
- [ ] Implement the decision (may spin a small follow-up ticket if the chosen direction is bigger than expected).

## Notes
If the decision is "hybrid caster instead of cloak," this depends on WA-018. If it's a simpler cloak on/off tweak, it doesn't. Resolve the decision first, then size the implementation.
