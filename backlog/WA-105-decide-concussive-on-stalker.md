---
id: WA-105
status: todo
size: S
phase: 2-post-launch
priority: 3
---
# Add Concussive Shells to the Stalker upgrade pool

## ✅ DECISION (2026-09-06): YES — add it
Taylor: add Concussive Shells to the Stalker pool now; it's removable later if it proves un-fun. This ticket is now an **ADD** task, not a decision.

**Current state (verified 2026-09-06):** `ConcussiveStalker` does **not** exist anywhere — no galaxy wiring, no XML, no strings. The body below claiming "Stalker was added in WA-053" is **stale/wrong**; only Firebat (`ConcussiveFirebat`) actually shipped. So this is a from-scratch per-unit concussive slice, wired exactly like `ConcussiveVoidRay`/`ConcussiveFirebat`: `addUpgradeToUpgrade("ConcussiveStalker","ConcussiveStalker")` + `AnyOf` Stalker tag, the marker `CUpgrade`, the weapon periodic apply-Slow gated by the marker, `CAbilResearch ConcussiveStalker<slot>`, `CButton`, GameStrings.

## The question
Firebat + Stalker were added to the concussive-shells pool (WA-053, done). But does Concussive Shells belong on the **Stalker** specifically?

## The concern
Concussive (slow-on-attack) on a mobile ranged unit is a **perma-kite tool** — slow the chaser, stay out of range, repeat. On the Stalker this could be oppressive (imagine marines trying to close on Stalkers while constantly slowed). This was flagged when Concussive was kept OFF the Stalker in the earlier blink discussion.

## New context
The [[WA-096]] Stalker rework makes blink a *rolled* upgrade (no longer native). So a concussive-Stalker no longer *also* has native blink — it's just a slowing ranged unit, which is less oppressive than concussive + blink would have been. That may make concussive-on-Stalker acceptable now. Needs a play test to decide.

## Do
Play games with concussive-Stalkers (post-WA-096) and judge: is the slow-kite oppressive, or fine now that blink isn't guaranteed? If oppressive, exclude Stalker from the concussive pool (NoneOf tag). If fine, keep.

## Notes
Spun out of WA-053 (closed). Related: [[balance-upgrade-legibility]] (concussive's slow is legible, at least), [[WA-096]].

## Priority note (2026-09-06)
DECISION MADE: add Concussive Shells to the Stalker pool now; remove later only if it proves un-fun. This ticket = ADD it (not decide).

## Update (2026-09-08) — leaning NOT doing it
Taylor reconsidered: the only reason to add it is the Stalker's now-smaller pool (post [[WA-107]]), but he
thinks that's fine — stim Stalkers / fast Stalkers are enough, no major problem expected. "I don't think
I'm going to do that." Leaving the ticket open at low priority in case the pool feels thin in practice, but
not planned.
