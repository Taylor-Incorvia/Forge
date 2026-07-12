---
id: WA-001
status: backlog
size: L
phase: 1-game-readiness
priority: 1
type: spike
---
# Arsenal / Roll Explanation modal  — SPIKE FIRST

The single biggest onboarding improvement: when a match starts, show each player exactly what they rolled, in plain language, so the chaos is legible. Too many unknowns to build directly — **this ticket is a timeboxed spike, not a build ticket.**

## Why
Reddit commenters worried about "marines with disruptor shots out of nowhere." The fix for a player's *own* confusion is legibility: show them what they were dealt at the start of the game.

## Do NOT groom this into build tickets yet
Writing detailed build tickets now would be guessing. Resolve the unknowns with a spike first.

## Open unknowns (what the spike exists to answer)
1. **How does it look?** Layout/contents of the dialog. (Genuine design unknown — answered by building an ugly version and looking at it.)
2. **What does the player click to open it?** Dialog button vs hotkey-bound command.
3. **Per-player content.** Each player must see *their own* rolls. (Likely the easy part — per-player roll data already exists in the galaxy code as stored slot→unit / slot→upgrade assignments, and SC2 dialogs are natively per-player. Prove it.)

## Big idea to test: forced 20s preview at game start
Fully pause the game for ~20s at the start and make players stare at their Arsenal modal.
- Technical caveat: truly pausing a multiplayer lockstep sim is finicky. Practical version is likely a "preview phase" — modal up, actions suppressed, countdown ticking. Nothing meaningful happens in the first 20s anyway.
- Design note: forced every game may annoy repeat players. Consider skippable-once-both-ready, shorter, or only first few games.

## Spike goal (timebox ~1 hr)
Prove out the three pieces cheaply:
- [ ] Create a per-player dialog populated from that player's stored roll data.
- [ ] Open it via a button and/or hotkey.
- [ ] Test the forced-preview pause approach.

## Spike output (the actual deliverable)
- [ ] Short doc in `docs/` recording what works, what doesn't, and the chosen approach.
- [ ] WA-001 broken into small build tickets based on findings (e.g. roll-data accessor, dialog UI, per-unit copy, preview-phase timer).

## Notes
Roll assignment lives in `initialize.galaxy` (units per slot) and `upgradeInitializers.galaxy` (upgrade per slot). Feeds off the unit→upgrade matrices from WA-003/WA-004. Website reference version (every unit + possible upgrades) is a separate, later docs task.
