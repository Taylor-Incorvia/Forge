---
id: WA-006
status: done
size: S
phase: 1-game-readiness
priority: 6
---
# Update mod description

## ✅ Done 2026-07-16 (PR)
Rewrote `DocInfo/DescLong` (enUS `GameStrings.txt`). New copy leads with the hook, explains the roll-and-adapt loop in 2 sentences, reassures it's rule-bound (not chaos), and ends with the Discord link:
> Every game, your tech tree is different. Play normal SC2 macro, but your Barracks, Factory, and Starport roll a semi-random mix of units from all three races at each tech tier, and your tech buildings roll random upgrades to match. Read what you're dealt and adapt. It's curated, not chaos: rolls are balanced and rule-bound (you won't roll Marines with Disruptor shots). Join the Discord: https://discord.gg/hdT6FCyM

Dropped the now-inaccurate "PICK TERRAN (P/Z don't work)" opener (P/Z auto-switch to Terran now) and the "re-researching gives no bonus" aside (kept brief; still covered in HowToPlay strings).

**Left alone (out of scope, flag if wanted):** `DocInfo/Name` still reads "Wildcard Arena - (Terran Only)" and `HowToPlayBasic00` still says "All players must choose Terran" — both mildly stale now that race auto-switches, but changing the published Name affects discoverability, so left for a deliberate call.

Rewrite the in-client mod description (the blurb players see in the SC2 Arcade / custom game list) so a stranger understands Wildcard Arena in one read and knows where to go next.

## Why
This is the first text a discovering player sees. The Reddit pitch already works ("Every game, your tech tree is different"). Reuse that voice. Include the Discord link — it's the top of your funnel (YouTube → Website → Discord → Queue).

## Acceptance criteria
- [x] One-line hook ("Every game, your tech tree is different").
- [x] 2-3 sentences on how it plays (macro like normal, but you roll random units + upgrades and adapt live).
- [x] Discord invite.
- [x] Reassures it's balanced/rule-bound, not pure chaos ("you can't roll marines with disruptor shots").

## Notes
Find where the description lives (likely `DocumentInfo` / editor metadata, or a GameStrings entry). First step of this ticket is locating it. Draft copy can lift directly from the Reddit post.
