---
id: WA-101
status: todo
size: M
phase: 2-post-launch
priority: 2
---
# Vulture: fix the mid-game fall-off — figure out its kit (NO mines/traps)

## Problem
The Vulture is strong early (fast, cheap raider) and then **falls off hard** — by the time an upgrade lands, you don't want to keep building Vultures. Its current upgrade (Speed) is fine but unexciting and nobody uses it. It needs a reason to stay relevant into mid-game.

## Hard constraint
**No mines, no traps, ever** — see [[feedback-no-mines-or-traps.md]]. (Any spider-mine idea is dead. The Vulture already has its native spider mines removed — those AbilArray slots are `removed="1"`; only KD8 remains.)

## Starting point (done separately)
[[WA-097]] removes the base KD8 Charge grenade (native always-on, illegible). So post-WA-097 the Vulture is a clean mine-free fast raider with a gun — a bit bare. This ticket decides what *replaces* that as its identity/scaling.

## The real lever: TIMING, not just the upgrade
The fall-off is a timing problem — whatever upgrade it gets must arrive **cheap + fast** (via the WA-092 research cost/time engine) so it lands *while the Vulture is still relevant*, bridging the early spike into mid-game. A great upgrade that arrives too late doesn't help.

## Candidates (mine-free)
- **Rolled KD8 Charge** — the grenade as a *researched choice* (not native). Non-mine, legible (visible explosion), gives an anti-clump/anti-light burst. Uncomment `addAbilityToUpgrade("KD8Charge","F_KD8Charge")`, gate to Vulture only, make it cheap+fast (WA-092). Caveat: Taylor called the MP KD8 "underwhelming" as a freebie — as a cheap early spike it may be fine, or may be mid.
- **Damage / fire-rate / range scaling** — a straightforward "keeps hitting hard into mid-game" upgrade. Legible if it changes visible output; risk of being a boring stat bump.
- **Anti-worker / anti-light bite** — lean into the raider identity (bonus vs workers or Light) so it stays a harass menace. Watch the attribute-bonus over-performance lens ([[balance-attribute-bonuses-overperform]]).
- (Speed exists but is the status quo that isn't cutting it.)

## Decision needed
Pick a direction (Taylor unsure as of 2026-09-06). Whatever it is: mine-free, legible, and cheap+fast so it beats the fall-off timing. Related: [[WA-092]], [[balance-upgrade-legibility]].

## Priority note (2026-09-06)
DECISION: give the Vulture a rolled KD8 Charge upgrade -- cost 50/50, ~43s research on Faster (catalog 60.2 via the WA-092 engine). Cheap+fast to beat the mid-game fall-off. Mine-free (KD8 is a thrown grenade, not a mine).
