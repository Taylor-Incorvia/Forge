---
id: WA-046
status: todo
size: M
phase: 1-game-readiness
priority: 900
---
# Nice-to-have: Ultralisk looks different when it has Chitinous Plating

Now that Chitinous Plating is a *rolled* research (WA-023) rather than always-on, you can't tell at a glance whether a given Ultralisk has the extra armor. Give the upgraded Ultralisk a **visual distinction** so opponents/allies can read it on the battlefield.

## Why
Pure readability/feel. A rolled defensive upgrade is invisible today — a visible cue (thicker plating, tint, glow, attachment) makes the roll legible and satisfying, same spirit as the ability telegraph (WA-030) and the stim/blink indicators already done.

## Options (pick during grooming)
- **Tint / team-color shift** on the Ultralisk model when the `ChitinousPlating` upgrade is present (cheapest).
- **Attachment / actor event** — add a plating/spikes model or a subtle glow via an actor that keys off the upgrade (`UpgradeComplete`-style actor term), like the stim/blink actor overrides we've used.
- **Model swap** to a visibly armored variant if one exists in the deps (heaviest; probably overkill).

## Acceptance criteria
- [ ] A freshly-built Ultralisk and an armor-upgraded one are visually distinguishable in-game.
- [ ] The cue only appears once the upgrade is actually researched (mirror the CountUpgrade-complete requirement pattern used elsewhere).

## Notes
Bottom of the priority list — genuine nice-to-have, do only if the visual bugs you. Sits **above** "support for other races," which is explicitly not planned (see icebox). Actor-event approach is the most in-keeping with existing indicators; start there in grooming.
