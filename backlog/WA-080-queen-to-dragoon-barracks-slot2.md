---
id: WA-080
status: todo
size: M
phase: 2-depth
priority: 70
---
# Consider replacing Queen with Dragoon in Barracks slot 2

## Why
The mod's Queen carries a lot of baggage: modded move speed (bumped to Hydralisk speed, WA — no creep in the mod) plus a full ability kit that gives her a complex, learn-it-first identity. That's a **hard sell** — players have to decode her. A **Dragoon** (SC1/BW) is the opposite: instantly legible, iconic, a simple ranged unit with zero learning curve. That's more on-thesis for the **readability pillar** (`docs/design-principles.md` — react to what you see; identity should be obvious). Trade a unit players must study for one they already know.

## Contingent on the extraction (WA-078)
This is a prime example of what own-your-unit-data unlocks: the roster is no longer limited to multiplayer units — you can curate the best-identity unit from **any** source. The Dragoon isn't in MP; it'd be **extracted from its campaign source** (Taylor: "Void campaign" — confirm exactly where the Dragoon model/data lives in SC2 before starting) using the **same recipe as WA-078**: staging with the source dependency → pull the campaign-only objects with original IDs → drop the dependency → add the explicit `.m3` path(s) → paste into Wildcard. The recipe generalizes beyond WoL to any campaign/mod.

## Design questions to settle
- **Slot 2 fit.** Barracks slot 2 today = Hydralisk / Marine / Queen (`initialize.galaxy`). Dragoon is a tanky ranged unit — does it slot cleanly next to Hydra/Marine, or is it stronger/weaker for slot 2? Cost/stats tuning per the Simple-Game-Plan-Effect rules (don't cut cost to buff; use build time / a rolled upgrade).
- **Queen's fate.** Remove her from the pool entirely, or keep her somewhere? (She was added in WA-019; if pulled, note it and clean her wiring.)
- **Dragoon upgrades.** What rolled upgrade fits a Dragoon (range? a shield/attack upgrade?) to keep it reactive rather than a-move fodder.

## Notes
Low priority, post-S1 (Season 1's roster is set). Enabler: **WA-078** (unit extraction). Related identity/readability rationale: `docs/design-principles.md`. Broader theme: extraction opens the full SC/BW/campaign roster for curation, not just the 7 WoL units.
