---
id: WA-098
status: done
size: S
phase: 2-post-launch
priority: 50
---
# Remove the Stalker temporarily (Factory slot 1)

## What
Pull the Stalker from the Factory slot-1 roll pool for now. Comment out its `addUnitToSlotPool` line in `initialize.galaxy` (line ~127) — the standard reversible way units are shelved (Banshee, Predator, etc. are commented out the same way).

## Why
The Stalker is illegible-unfun with everything but Blink Cooldown, and it outperforms its Factory slot-1 pool-mates (Vulture > Hellion, Stalker > both). Rather than ship a half-fixed unit, pull it now (removes the bad experience from games) and rebuild it with a legible, blink-centric upgrade pool per [[WA-096]], then re-add. Also cleans up the slot-1 overperformer in one move.

## Why this is safe (no cache trap)
Commenting the pool line means the Stalker simply never gets assigned to a slot — nothing is removed from a static command card. Its train button (`FactoryTrain,Train22`) only ever appears when a Stalker is the rolled unit, so it just never activates. Fully reversible: uncomment to bring it back.

## Done
`initialize.galaxy` line 127 commented out with a note pointing to WA-096. Shipped via PR (not main).

## Re-add criteria
Bring the Stalker back once WA-096's legible blink pool is built (cut generics, fix Blink Range sight, keep Blink Cooldown, add shields-on-blink / blink-shockwave).
