---
id: WA-001
status: epic
size: L
phase: 1-game-readiness
priority: 1
---
# Your Arsenal modal (EPIC) — show the player their rolled units + upgrades + tech tree

The single biggest onboarding improvement: at match start, show each player their full randomized faction — every rolled unit, its corresponding rolled upgrade, and how the slots relate — so the chaos is legible without a wall of text. The mod's identity feature.

## Why
Reddit worried about "marines with disruptor shots out of nowhere." The fix for a player's *own* confusion is legibility: show them what they were dealt.

## Historical note — feasibility is already established
The mod began as a **"design your race"** mod (not randomized). In that era the user built/experimented with this exact class of UI and **confirmed it's technically possible** in SC2's dialog system. The open question was never "can it be done" but "how to lay it out" — and it's known that **it will be inconvenient to build no matter the design** (SC2 dialogs are manual and finicky). So: **no blocking feasibility spike.** Design first, build second, validating the chosen layout with a minimal version before completing it.

Two de-risks worth stating:
- **Local-testable.** Runtime galaxy dialogs are NOT command-card buttons → they don't hit the "publish to verify" quirk; iterate in the editor Test Document.
- **Data already exists.** Rolled units live in per-slot state (assignment in `initialize.galaxy`), rolled upgrades in the per-slot selected-upgrade string (`upgradeInitializers.galaxy`). Population = reading existing state. SC2 dialogs are natively per-player.

## Plan (design-first). Build phases get ticketed AFTER the design lands.
1. **[WA-039] Paper design + info/interaction spec** ← grab first. The ideal layout on paper, then compromise; define always-visible vs tooltip info + open/close/reopen behavior.
2. **Static modal shell** — build the chosen layout in galaxy with placeholder data; start minimal (1 facility) to validate rendering, then complete all slots. Decide if a tiny grid helper removes coordinate repetition (NO framework).
3. **Populate rolled units** — each slot from the player's actual roll (icon/name/cost/tooltip), per-player.
4. **Populate rolled upgrades** — each slot's rolled upgrade; unit↔upgrade pairing clear; **flag starting upgrades** (ShieldWall/GroovedSpines) so they don't read as randomly rolled.
5. **Tech-tree explanation pass** — slot correspondence + production-vs-upgrade-facility legible via layout/cues, minimal text.
6. **Readability & clutter pass** — test at aspect ratios / UI scales; tune sizes/spacing/hierarchy; detail → tooltips.
7. **Integration & cleanup** — remove placeholders/dev controls; init after rolls assign; dev reopen shortcut; document the UI structure.

(Adapted from the user's high-level plan: feasibility folded in given the historical confirmation; grid-helper folded into phase 2.)

## Design consideration to carry into WA-039: forced preview at game start
Idea: hold a ~20s "preview phase" at match start with the Arsenal up and actions suppressed (truly pausing a lockstep MP sim is finicky; a preview phase is the practical version — nothing meaningful happens in the first 20s anyway). Forced every game may annoy repeat players → consider skippable-when-both-ready, shorter, or first-few-games-only. Decide the open/close behavior in WA-039.

## Why the build phases aren't tickets yet
Phases 2–7 depend on the chosen layout + info set — ticketing them before WA-039 would be speculative and get rewritten. Full plan is captured here so nothing's lost; groom the next phase into a ticket when WA-039 lands.
