---
id: WA-107
status: in-progress
size: S
phase: 2-post-launch
priority: 1
---
# Stalker: retire the enhanced-Blink experiment — make it a normal factory slot-1 unit

## Decision (2026-09-06)
The enhanced-Blink experiment (WA-096) is being **undone**. Root problem: it was trying to
force **two upgrades** (blink + a blink enhancement) into a system that only allows **one upgrade
per slot**. Taylor: "I'm trying to get creative without getting creative... make my own game without
making my own game. My own game is a separate project. This is a StarCraft 2 mod."

### Landing (what this ticket ships)
- **Stalker stays in factory slot 1.** No slot-2 move, no Goliath/Dragoon/Slayer/Roach shuffle
  (all considered and rejected — see options below).
- **No baseline Blink.** A Stalker that *starts* with Blink in slot 1 is over-tuned: it's the
  first unit out, beats Vulture/Hellion, is armored-favored in a mostly-armored pool, shoots
  up + down, and is highly mobile. Too strong that early.
- **Blink becomes a normal roll.** The Stalker rolls Blink like any non-caster unit, plus speed,
  corrosive bile, range, stim, concussive ([[WA-105]]) — the standard pool.
- **Speed stays in the pool.** With one-upgrade-per-slot you can't get blink + speed together, so
  the old WA-095 speed exclusion isn't needed. (Blink + speed was the "uncatchable" fear; it can't
  happen anymore.)
- **`stalkerblinkrange` and `stalkerblinkcooldown` retired.** Removed from the roll pool. Their
  XML defs (CUpgrade / CAbilResearch / CButton / GameStrings) + the factionIcons icon-path entries
  are left in place as dead/unused data — pulling a command-card button can poison the editor cache,
  and an unrolled upgrade never appears anyway.

## Code (branch wa-107-stalker-normal-rolls-remove-enhanced-blink, PR pending)
`upgradeInitializers.galaxy`:
- Removed the `Blink NoneOf Stalker` exclusion (~163) → Stalker can roll plain Blink.
- Removed the `stalkerblinkrange` pool wiring (~296-298) and `stalkerblinkcooldown` pool wiring (~346-348).
- Left baseline blink off (`grantUpgrade(player,"BlinkTech")` stays commented, ~446) and updated the stale comments.

## Why this over the alternatives (all considered, rejected)
- **Slot 2 + baseline Blink:** plausible ("late enough that baseline blink is OK"), but forces a new
  slot-1 unit. Goliath (too big for slot 1, weird vs smaller slot-2 units), Lurker/Siege Tank (no),
  Dragoon (real work), Slayer (a recolored Stalker — too much deviation, "what the fuck is that"),
  Roach (history of being oppressive; the scrap/mecha-Roach nerf looked bad). Not worth it.
- **Remove Stalker entirely:** slot 1 being always Hellion/Vulture is acceptable, but not necessary
  now that the enhanced-blink pressure is gone.

## Shelved ideas (do NOT build now)
1. **Enhanced Blink (rapid / long-range):** genuinely cool micro (esp. the cooldown variant), but it
   only makes sense as a *second* upgrade. Save for a **Forge mechanic** (the long-planned phase-3
   "second upgrade / hero unit" system) or Taylor's separate game. See the phase history in notes.
2. **Stalker splash-on-attack upgrade:** small-radius splash. Taylor is open to this. **Hard requirement:
   the on-hit visual must be obvious** — the opponent has to be able to see what's happening
   ([[balance-upgrade-legibility]]). Spin into its own ticket if pursued.

## Notes
Original move (Stalker: barracks slot 2 → factory slot 1) was to fix fit (only armored option, weak AA)
and it unblocked adding the Void Ray to starport slot 2 (previously some rolls had no non-armored AA vs
Void Ray). That history stands; this ticket only reverts the *baseline-blink / enhanced-blink* layer, not
the slot placement. Supersedes the WA-096 approach. Related: [[WA-087]] (blink-zealot watch), [[WA-105]].

## Test when back at setup
- Roll a Stalker; confirm it has **no** blink until an upgrade is researched.
- Confirm Blink **can** roll on the Stalker (Armory research) and works.
- Confirm speed/bile/range/stim/concussive still roll fine.
- Confirm no orphaned/broken buttons appear at the Armory from the retired upgrades.
