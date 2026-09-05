---
id: WA-095
status: done
size: S
phase: 2-post-launch
priority: 50
---
# Exclude Stalker from the Speed upgrade (blink + speed = uncatchable)

## What
Add `addUpgradeRequirementTag("Speed", logicType_NoneOf, "unitTag", "Stalker")` so a Stalker can no longer roll the generic Speed upgrade.

## Why
The Stalker already has **native Blink** (it's the "Blink Stalker"). Speed on its own is a fine upgrade — the problem is the **combination**: blink + super-speed makes the Stalker effectively uncatchable, with no counterplay to punishing it (it always slips a surround and out-runs pursuit). Taylor: "Speed would be fine if it didn't also have blink."

Surgical fix over a blunt cost/build nerf: this removes the degenerate *double-mobility* combo while leaving the base Blink Stalker (and its identity) untouched. Cost/build-time nerfs would punish the base unit, which is fine as-is.

## Why exclusion, not WA-092 research tuning
The research ability is `Speed<slot>`, shared across all Factory-slot-1 units (Vulture/Hellion/Stalker), so per-unit research cost/time can't isolate Stalker (see [[reference-research-ability-per-slot-shared]]). Tag exclusion is the right tool — same pattern as Tempest (mobility → no counterplay).

## Context
Surfaced vs HeavyDrinker (a stronger player). The *loss* was confounded (skill gap + Taylor's own macro misplay + Phoenix-lift was a working counter), so it's not a "Stalker too strong" nerf — it's a targeted removal of a specific no-counterplay combo. See [[balance-for-forced-creativity]].

## Done
One-line tag added in `upgradeInitializers.galaxy` next to the other Speed exclusions. Shipped via PR (not main) for review.
