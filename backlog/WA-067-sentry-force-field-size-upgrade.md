---
id: WA-067
status: todo
size: S
phase: 1-game-readiness
priority: 15
---
# Sentry force-field size upgrade — one giant wall

## Why
The Sentry's rollable upgrade pool is one of the smaller ones. This is a signature, **highly visible** Sentry-specific upgrade (fits "noticeable upgrades over invisible ones"): a rolled upgrade that makes the Sentry's own Force Field dramatically bigger. Not a caster-pool spell — a per-unit catalog upgrade gated `AnyOf Sentry`.

## Design intent
- **Double the force-field radius** — one enormous wall. Force fields are **circles** (each is a round, temporary pathing-blocker unit, ~11s life), so this is a clean radius scale, not a shape change.
- Visible on sight: a huge blue disc reads instantly as "big force field."
- **The oppressive edge (intentional — challenge players to find it):** one Sentry can now wall an entire ramp/choke, or cut an army cleanly in half every engagement; mass Sentries become a movable Great Wall. Temporary duration is the release valve. Ship it and watch whether it's fun-oppressive or actually-broken.

## Technical breakdown
- Catalog upgrade, same pattern as `TempestRange`/`SiegeTankRange` (WA-031): create a `CUpgrade` that scales the ForceField unit's radius (verify exact field — likely `Unit,ForceField,Radius` and/or its inner/collision radius; the *visual* disc may be an actor scale that needs a matching bump, same class of problem as the flame-wall WA-066 visual).
- Wire like any catalog stat upgrade: `addUpgradeToUpgrade("SentryForceFieldSize","SentryForceFieldSize")` + `addUpgradeRequirementTag(..., AnyOf, Sentry)`, one `CAbilResearch`/`CButton` at the Sentry's slot, GameStrings.
- **Scope the exact radius field + whether the disc actor auto-scales before committing to a multiplier.**

## Stretch (separate, harder)
- The **2×2 square of four force fields** version (spawn four blockers in a pattern) — cooler, but a multi-create pattern, not a radius scale. Only after the radius version proves out.

## Acceptance criteria
- [ ] Rolling the upgrade makes the Sentry's Force Field visibly ~2× radius (disc art matches the new blocked area — no invisible blocking).
- [ ] Gated to Sentry only; costs energy as normal per cast.
- [ ] Verify on a **published** build if the disc scale is actor-driven (upgrade visuals have bitten us before).

## Notes
Low priority — **not before the "Your Faction" modal** (WA-039/WA-001). One of the "grow the small Sentry pool" ideas alongside WA-069 (Guardian Shield shockwave). Sibling visual-scaling work: WA-066.
