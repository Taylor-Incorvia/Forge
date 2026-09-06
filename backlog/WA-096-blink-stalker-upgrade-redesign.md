---
id: WA-096
status: done
size: M
phase: 2-post-launch
priority: 45
---
# Stalker rework: blink is no longer native — it's a rolled upgrade

## The rework (DONE — shipped via the stalker-rework PR)
**Blink stops being native.** Previously `grantUpgrade(player, "BlinkTech")` gave every stalker free blink at spawn — the root cause of "blink shows up before 3:00" and of blink+X being uncatchable. Now blink is granted **only when the stalker rolls a blink upgrade**, so it's gated behind a roll + research (later, tunable via WA-092), not free/early.

### How (reuses existing machinery — no new upgrades, no F_Blink repointing)
`grantGenericUpgrade` grants *every* entry in an upgrade's ability/behavior/upgrade lists, so one key can grant multiple things. So the two existing stalker blink upgrades now **also grant `BlinkTech`** (the stock blink), making them the blink "variants":
- `stalkerblinkcooldown` → grants BlinkTech + reduced cooldown = **blink + faster**.
- `stalkerblinkrange` → grants BlinkTech + longer range = **blink + longer**.
- Commented out the unconditional `grantUpgrade(player, "BlinkTech")` (upgradeInitializers ~443).
- Kept the `Blink` (F_Blink) NoneOf-Stalker exclusion — the stalker uses BlinkTech via the variants, not the custom F_Blink.

### What this unlocks
- **Speed/Corrosive Bile/Range are fine on the Stalker again** — they were only bad because they stacked on *native* blink. Speed re-allowed ([[WA-099]], reverts WA-095). Bile/Range were never excluded.
- **Renamed back to "Stalker"** — no native blink, so "Blink Stalker" was wrong ([[WA-100]], reverts WA-091).
- Every blink stalker now has a *flavor* (faster or longer), and blink is an exciting roll, not a baseline. One upgrade per unit = legible ([[balance-upgrade-legibility]]).

### NEEDS editor verification (behavioral — the code is small but this is a core mechanic)
- Roll `stalkerblinkcooldown` / `stalkerblinkrange` → confirm the stalker **actually blinks** (BlinkTech-via-roll enables the Blink button) AND has the modifier.
- Roll Speed/Bile/Range → confirm the stalker has **no blink** (blink button greyed/inert is acceptable; if it looks bad, hide it via requirement later).
- Confirm blink comes online only after researching, not at unit spawn.

## FUTURE — cool blink upgrades to ADD as more variants (kept from the brainstorm)
Each would be a new roll option that grants BlinkTech + an effect. Judge every one by **legibility** (opponent must see it):
- **Blink → SHOCKWAVE at destination** (LEAD): displacement/knockback (Lee Sin ward-hop→R) or small damage, wrapped in an obvious shockwave. Self-advertising. Displacement preferred over slow (slow = Concussive-style un-escapable).
- **Shields after blink** (~40 over ~4s; Patches-style). Legible (visible regen).
- Watch: on-blink effects + reduced cooldown = blink-in/out spam; reward committing, not hit-and-run.
- Rejected: bonus-damage-after-blink (illegible), cloak-on-blink (worse here — Scan nerfed), 2 charges (redundant), conditional cooldown (group desync), Concussive (perma-kite). Full reasoning in git history of this ticket.

## Notes
Supersedes the old "all-new on-blink upgrades" plan with a simpler root-cause fix. Related: [[WA-099]], [[WA-100]], [[balance-upgrade-legibility]], [[balance-for-forced-creativity]], [[WA-092]] (tune blink research timing).
