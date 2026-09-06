---
id: WA-099
status: done
size: S
phase: 2-post-launch
priority: 50
---
# Revert WA-095 — re-allow the Speed upgrade on the Stalker

## What
Removed the `addUpgradeRequirementTag("Speed", logicType_NoneOf, "unitTag", "Stalker")` line (added by WA-095). The Stalker can roll Speed again.

## Why
WA-095 excluded Speed because native Blink + Speed = uncatchable, no counterplay. But the [[WA-096]] rework removes native blink — blink is now only granted via a rolled blink variant. So a speed-stalker no longer *also* has blink to escape a surround; it's fast-but-catchable and legible (one upgrade). The reason for the exclusion is gone.

## Sequencing (important)
This revert is only safe **because it ships in the same PR as the WA-096 blink-removal**. Re-allowing Speed while blink was still native would reintroduce the exact uncatchable combo — that's why it wasn't split into its own PR.

## Done
Line removed in `upgradeInitializers.galaxy`, in the stalker-rework PR alongside WA-096/WA-100.
