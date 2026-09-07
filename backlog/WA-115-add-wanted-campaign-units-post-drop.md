---
id: WA-115
status: todo
size: M
phase: 2-post-launch
priority: 3
---
# Add desired campaign units into our own data (post dep-drop)

Part of [[WA-108]]. Item 12. Once the extraction recipe is proven and the dep is gone, pull in the
campaign units Taylor actually *wants* — as self-contained data, no dependency.

## Candidates
- **Dragoon** (Taylor's named example — also the WA-080 idea of Queen→Dragoon in barracks slot 2, and a
  candidate factory slot-1 unit per the [[WA-107]] discussion).
- Any other "interesting" campaign unit that survives the extraction recipe and earns a pool slot.

## Approach
Reuse the [[WA-078]] extraction recipe (raw XML from `reference/campaigns/liberty.sc2campaign`, keep
original ids, add the explicit `.m3` model path, swap custom missile movers to `MissileDefault`, re-apply
force-field collision on ground units, render-test in a VoidMulti-only staging mod first). Each new unit
is add-only.

## Notes
Do NOT start until the dep is cleanly dropped and a functional zero-WoL build exists — otherwise you're
piling new content onto a broken base. Gameplay/roster decisions (which slot, cost, whether it displaces
an existing unit) are separate design calls. Related: [[WA-080]] (Dragoon), [[WA-107]] (slot-1 options).
