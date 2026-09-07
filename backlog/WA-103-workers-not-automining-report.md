---
id: WA-103
status: todo
size: S
phase: 2-post-launch
priority: 30
---
# BUG (unconfirmed): a player reported workers didn't auto-mine at game start

## What
One player reported their workers did **not** auto-mine at 0:00. Taylor has **never reproduced this** — auto-mine (WA-060) has worked perfectly in every one of his tests.

## Big caveat — likely not real, or race-specific
Taylor does all his testing **as Zerg**. The reporter may have picked **Protoss** (or Terran), so if there's a bug it could be a **race-specific gap** in the auto-mine trigger that Zerg testing never hits. Or it's user error / a one-off. Low confidence this is a real bug.

## Do
1. Test auto-mine at 0:00 as **all three races** (Terran, Protoss, Zerg) — the auto-mine-after-race-replacement path especially, since which race you pick shouldn't matter.
2. If it only fails for one race, fix that branch. If it never fails, close as not-reproducible.

## Notes
Spun out of WA-060 (which is confirmed working and now closed). Related: race-replacement auto-mine logic.
