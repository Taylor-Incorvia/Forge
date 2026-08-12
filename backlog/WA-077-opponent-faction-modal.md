---
id: WA-077
status: todo
size: L
phase: 2-depth
priority: 60
---
# Opponent Faction modal — accumulate discovered enemy rolls (Marc's idea)

## Why
The "Your Faction" modal (WA-001 / WA-039) shows *your* rolled hand. Marc's idea: a **mirror panel for your opponent** that starts empty and fills in as you scout. Each enemy structure you discover records that facility's rolls into the panel, and it **stays recorded for the rest of the game** even after you lose vision — a growing intel dossier on their faction.

## Why this instead of persistent floating icons
Taylor's instinct is that scouted info should persist ("see it once, remember it"). But making the floating scout icons (WA-070) persist would clutter the map with stale icons hovering over buildings you can't currently see. The clean split:
- **Floating icons = the LIVE layer** (WA-074, shipped): show only while you have active vision. Honest, uncluttered "what I see right now."
- **Opponent modal = the RECORD layer** (this ticket): accumulates and persists what you've discovered, openable anytime.

They compose — the shipped active-vision icons aren't wasted; this is the persistence idea done right. **Taylor prefers this direction over further icon-visibility tuning.**

## How it'd work (sketch)
- **Reuse the Your Faction modal UI**, populated with the opponent's roll data — already stored per-player (`getPlayerSlotUnitId` / `getSlotSelectedUpgrade`, keyed by facility + slot + player).
- Rolls are per-player, so **discovering any one structure of a facility type reveals that whole facility's lineup** (same rule as the icons): see one enemy Barracks → their full Barracks slot list is logged; see their Armory → its rolled upgrades are logged.
- Track discovery per **(viewer, opponent, facility-type)** in a DataTable; once set, it latches and persists.
- Open it via a tab/button, like the FACTION tab.

## Key open questions
- **Discovery detection.** Need "viewer gained vision of an enemy facility of type X" → mark X discovered. No native vision-enter event exists (see WA-074), but this is cheap because it only needs to fire **once** per facility-type per opponent: a light periodic `VisIsVisibleForPlayer` check over enemy facilities that **latches and stops checking** once discovered, or piggyback on the scout-tag visibility.
- **Multi-opponent (FFA/team):** a tab per opponent, or ship 1v1-first.
- **Units vs upgrades:** show both, mirroring the Your Faction layout (units from Barracks/Factory/Starport, upgrades from Ghost Academy/Armory/Fusion Core).
- **Add-on-gated top slots:** show all discovered slots, or mark the add-on-gated one? Minor, mirror the Your Faction treatment.

## Notes
Credit: **Marc**. Family: WA-001 / WA-039 (Your Faction modal), WA-070 (scout icons — the live layer), WA-074 (vision-gate — shipped). Post-S1 polish; Season 1 already has what it needs content-wise.
