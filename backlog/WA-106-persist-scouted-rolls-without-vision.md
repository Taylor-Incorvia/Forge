---
id: WA-106
status: todo
size: L
phase: 2-post-launch
priority: 45
---
# Make scouted enemy rolls referenceable WITHOUT active vision

## Problem
The floating production icons over enemy structures (WA-070, done) only show **while you currently have vision** of the building — they vanish the moment you lose sight. Taylor considers this a **major limitation**: you scout once, learn what the enemy can produce, then can't reference it again without re-scouting.

## Preferred direction (Taylor)
**Persist the icons over the enemy structures even without active vision** — a fog-of-war "memory" of what you last saw a building could produce/research, shown greyed/dimmed over the last-known structure location. Players are already trained to *look at the opponent's base* to read what it can make, so keeping the info *there* is the intuitive home for it.

## On the Opponent Faction modal ([[WA-077]])
Taylor is **not fully sold** on the separate opponent-faction modal. It can be a **gap-closer**, but it's **not ideal** — it pulls the info out of the base, away from where players instinctively look. Prefer the persistent-over-structures approach here; treat WA-077 as a fallback, not the target.

## Do (spike)
Figure out how to render remembered production icons over a structure the player no longer has vision of — likely tied to fog "snapshot" / last-known state. This is the harder part (SC2 fog + UI). Size L until scoped.

## Notes
Spun out of WA-070 (done). Related: [[WA-077]] (the modal alternative), scoutTags system.
