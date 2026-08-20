---
id: WA-085
status: todo
size: M
phase: 1-game-readiness
priority: 50
---
# Defuse the "unbound hotkeys" modal (the on-ramp to the escape bug)

## What
At game start SC2 warns "you have unbound hotkeys, please change your hotkeys" because the mod has command-card commands with no key (the **"Unbound"** category). The modal **prompts players to go change their hotkeys** — and *that* switch to Standard is what triggers the Cancel/escape bug ([[WA-076]]). So the modal is the on-ramp. Kill the modal → far fewer players get pushed into the escape trap.

Worst case observed: a player's *first* game is a 4v4, they follow the modal's advice, and their hotkeys (incl. Escape/Cancel) break mid-game with no easy pause.

## Data (as of 2026-08-17)
- 253 CButtons in the mod; only ~34 carry an inline `<Hotkey>` and ~75 have a `Button/Hotkey/` entry in GameHotkeys.txt → many command-card buttons potentially resolve to no key.
- The literal target list = whatever is sitting in the **"Unbound" category** of the mod's hotkey menu. That category IS the to-do list.

## Approach
Give every unbound mod command a hotkey — inline `<Hotkey>` in ButtonData (source of truth) — so nothing lands in Unbound → no modal.

## TWO unknowns to settle with a cheap spike BEFORE committing
1. **Is the Unbound list mostly the mod's custom buttons?** Open the Unbound category and read it. If it's F_ abilities / custom research/train buttons → bindable.
2. **Does binding in mod data actually silence the modal — or does the client evaluate against the *Standard* profile (which can't know custom commands no matter what the data says)?** This is the crux; if it's the client profile, it's the same wall as WA-076 and no data change wins. **TEST:** bind ONE currently-unbound command, publish, check if the unbound count drops. One build, decisive.

## Size
- If the spike says "mod bindings count" → bounded, mechanical audit: one `<Hotkey>` line per unbound button (likely a few dozen), low-risk.
- If it says "client wall" → stop; don't sink effort (same intractability as WA-076).

## Honest payoff
Does **NOT** fix the escape bug. It removes the *prompt* that shoves players into it. So the practical harm drops a lot (nobody's told to go break their hotkeys mid-game), but a player who manually picks Standard still hits it. Harm-reduction, not a cure.

## Acceptance
- [ ] Spike done: know whether mod-data bindings reduce the unbound count (go/no-go).
- [ ] If go: Unbound category empty (of mod commands) → no "unbound hotkeys" modal on a published build.
- [ ] No hotkey collisions introduced (re-run the collision check — see [[WA-005]] / [[WA-033]]).

## Notes
Post-launch. Related: [[WA-076]] (the escape/Cancel bug itself), [[WA-082]] (cleanup already removed dead buttons, shrinking the surface). First move is 10 min: open the Unbound category, list it, then bind-one-and-test.
