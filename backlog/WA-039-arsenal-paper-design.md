---
id: WA-039
status: todo
size: S
phase: 1-game-readiness
priority: 2
---
# Arsenal modal — paper design + info/interaction spec

First ticket of the **[WA-001] Your Arsenal** epic. **Design-first: figure out the ideal on paper, then compromise toward what's buildable.** No code — this produces a design doc the build phases follow. Feasibility is already established (see WA-001), so design freely.

## What it must convey (the goal is clear; the *look* is the open question)
- Every rolled production unit, grouped by its building (Barracks / Factory / Starport).
- Each unit's corresponding rolled upgrade, with the **unit↔upgrade relationship obvious**.
- Which slot each unit occupies, and that the matching upgrade-facility slot upgrades that unit.
- The whole faction at a glance — no wall of instructional text.

## Leading candidate (2026-07-15): facility-paired icon grid
Icon-forward, text in tooltips (icons + tooltips confirmed feasible in the design-your-race era). Each **production-facility icon sits directly above its upgrade-facility icon**, and each **unit sits directly above its upgrade** — the vertical alignment + the facility pairing carry the "this unit is upgraded at that facility's matching slot" relationship with **no extra text**:
```
┌──────────────── YOUR ARSENAL ────────────────┐
│  [🏢 Barracks]      [U1][U2][U3][U4]   build   │
│  [🎖 Ghost Acad.]   [u1][u2][u3][u4]   upgrade │
│                                               │
│  [🏭 Factory]       [U1][U2][U3]              │
│  [⚙ Armory]         [u1][u2][u3]              │
│                                               │
│  [🚀 Starport]      [U1][U2][U3]              │
│  [🔧 Fusion Core]   [u1][u2][u3]              │
└───────────────────────────────────────────────┘
```
Implied decisions:
- **Always visible:** icons only (units, production facilities, upgrade facilities, upgrades).
- **Tooltip-only:** names, costs, effect descriptions. Keeps it ~20 icons, un-busy.
- The Barracks↔Ghost Academy / Factory↔Armory / Starport↔Fusion Core pairing is the device that teaches the tech tree — no instructional copy.
- Optional subtle slot-bond cue: faint alternating column shade, only if alignment alone isn't obvious enough.

Still open (decide in this ticket): open/close/reopen behavior (auto at start? button? forced preview?); whether to mark **starting** upgrades distinctly from rolled; whether to show empty/blank slots.

## Work (mostly you, on paper; I assist)
- Sketch several **materially different** layouts. Try: facilities as rows vs columns; units above upgrades; paired unit+upgrade columns; facility grouping/separators; where cost/name/short-explanation live.
- _(Ask me for ASCII mockups of options to react to, and I'll capture decisions — this is aimed at your "I don't know how it'd look" gap.)_
- Decide **always-visible vs tooltip** for units (icon / name / cost / slot # / role?) and for upgrades (icon / name / effect).
- Decide **open / close / reopen** behavior: auto at game start? manual button? both? Icons clickable or informational only? Show empty/unused slots? (Carry the forced-preview idea from WA-001 into this decision.)
- Pick ONE layout to prototype.

## Output / acceptance
- [ ] A design doc in `docs/` capturing: the chosen layout, the always-visible-vs-tooltip info set (units + upgrades), and open/close/reopen behavior.
- [ ] The chosen design clearly communicates slot correspondence and doesn't lean on long text.
- [ ] No unresolved interaction decisions left that would block the build.

## Next
Once this lands, groom WA-001 phase 2 (static modal shell) into a ticket.
