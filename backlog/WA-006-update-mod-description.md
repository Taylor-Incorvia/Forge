---
id: WA-006
status: todo
size: S
phase: 1-game-readiness
priority: 6
---
# Update mod description

Rewrite the in-client mod description (the blurb players see in the SC2 Arcade / custom game list) so a stranger understands Wildcard Arena in one read and knows where to go next.

## Why
This is the first text a discovering player sees. The Reddit pitch already works ("Every game, your tech tree is different"). Reuse that voice. Include the Discord link — it's the top of your funnel (YouTube → Website → Discord → Queue).

## Acceptance criteria
- [ ] One-line hook ("Every game, your tech tree is different").
- [ ] 2-3 sentences on how it plays (macro like normal, but you roll random units + upgrades and adapt live).
- [ ] Discord invite.
- [ ] Reassures it's balanced/rule-bound, not pure chaos ("you can't roll marines with disruptor shots").

## Notes
Find where the description lives (likely `DocumentInfo` / editor metadata, or a GameStrings entry). First step of this ticket is locating it. Draft copy can lift directly from the Reddit post.
