---
id: WA-065
status: todo
size: M
phase: 4-analytics
priority: 20
---
# Replay stats / analysis tool — extract game data from Wildcard Arena replays

## Why
Wildcard Arena is a data-rich game (rolls, upgrades, comps) but right now there's no way to look back at what actually happened across games. A local tool that parses `.SC2Replay` files would feed **balance decisions** ("which rolled upgrades actually get researched vs. ignored?"), eventually a **stats site**, and a personal **get-better-at-my-own-game** loop. Not urgent — worth building once the game feels like it's in a good spot.

## What to extract (priority order, from playtest wishlist)
1. **Who won.** (player results)
2. **What each player rolled.** The mod prints rolls in chat at 0:00 (temporary until the "Your Faction" modal, WA-039/WA-001), so **chat = rolls**. Parse the opening chat messages.
3. **Where units died + what type.** `UnitDiedEvent` carries unit name **+ x/y position + timestamp** → a per-game death list, and eventually a **heatmap**.
4. **Structures over time.** `UnitBornEvent` filtered to structures + timestamp → "what did each player have at minute N".
5. **Which rolled upgrades got researched vs. not.** `UpgradeCompleteEvent` cross-referenced against what was rolled (from #2). This is the balance gold: rolls that never get used are candidates for a rework.

## Approach
- **Tool: `sc2reader`** (Python). The tracker/message/game event streams are largely **protocol-level and mod-agnostic**, so chat, winner, and positions come through regardless of the custom mod.
- **NOT** generic stat sites (sc2replaystats, spawningtool) — they don't understand the custom units and won't read the roll-chat.
- **Phased build:** MVP = winner + rolls-from-chat + unit-death list (highest confidence). Then structures-over-time + upgrades-researched.
- Ship as a reusable `scripts/Parse-Replay.*` (drop a replay in → get a summary), mirroring the patch-notes script.

## Caveats / risks
- Custom-mod replays can trip up `sc2reader` on load; validate it parses at all first.
- Rolled units likely appear as their **Terran base types** (race-replacement) or the mod's internal ids — fine, but the mapping needs a small lookup to be human-readable.
- No Python installed in the dev env yet — setup is part of the ticket.

## Acceptance criteria
- [ ] Given a `.SC2Replay`, output winner + each player's rolls (from chat) + a unit-death list (type, x/y, time).
- [ ] Runs as a single command on any replay file.
- [ ] Extends to structures-over-time and upgrades-researched once the MVP parses cleanly.

## Notes
Seeds the eventual stats site (Phase 3/website) and informs balance. Related: [[WA-039]] / [[WA-001]] (the Faction modal will surface rolls in-game, replacing the chat print — but the replay parser still reads historical chat). Feeds [[docs/playtest-notes.md]] with hard data to back up feel-based observations.
