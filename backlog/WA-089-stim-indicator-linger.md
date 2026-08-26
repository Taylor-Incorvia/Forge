---
id: WA-089
status: todo
size: S
phase: 1-game-readiness
priority: 30
---
# Stim indicator too brief on rolled-stim units — make the cue linger ~3s (band-aid)

## What
On units that ROLL Stimpack (Tempest, Queen, Hydra, etc. via `F_Stimpack`), the only "stimmed" cue is a one-shot flash at the moment of cast (~1s). There's no persistent overlay, so it's easy to miss that a unit is stimmed — Taylor lost to stimmed Tempests and never noticed the buff was up. WA-026 made the flash fire on `F_Stimpack`; this is about **duration/visibility**, not whether it fires.

## Decision (Taylor, 2026-08-25)
**Band-aid, ~3 seconds — NOT full buff duration.** A marker that lasts the whole stim duration would make the UI too busy. Just enough to catch it. Deferred (post-launch polish, low priority).

## Why it's not a one-field tweak (investigated 2026-08-25)
The stim visual is a **one-shot poof model** (`CModel StimpackStartImpact`, parent `OneShotSpellFX`). It plays once (~1s) and self-destroys — the actor inherits `ModelAnimationStyleOneShot`, which destroys on the BSD animation finishing (reference: `<On Terms="AnimBracketState.*.AfterClosing; AnimName BSD" Send="Destroy"/>`). There is **no valid anim time-scale field** on the actor to slow it, and the model has no persistent/stand state to hold. So extending it needs one of:

- **Pulse:** re-fire the poof ~3× over 3s via `TimerSet` / `TimerExpired` (proven pattern at reference `liberty.sc2mod` actordata ~line 11450: `TimerSet 3.0 Die` / `TimerExpired; TimerName Die → Destroy`). Reads as sustained ~3s but flickers; actor-timer scoping when created-on-event is finicky — **needs in-editor verification**.
- **Different model:** swap to a looping/persistent overhead marker created on `Behavior.Stimpack.On`, destroyed by a 3s timer. Cleaner look; needs a model choice + editor eyeball.
- Rejected: just scaling the single flash bigger — adds visibility at cast but no duration.

## Acceptance criteria
- [ ] Any unit that rolls stim shows a "stimmed" cue lasting ~3s from cast.
- [ ] Cue does NOT persist the full buff duration (avoid clutter).
- [ ] Verified in the Test Document on a non-native stim unit (Tempest / Queen).

## Notes
Actor: `StimpackStartImpact` (mod override already listens for `Abil.F_Stimpack.SourceCastStart`, per WA-026). Model asset: `CModel StimpackStartImpact`. End flash: `StimpackEndImpact` (keys on `Behavior.Stimpack.Off`). Actor visuals are the class of change that genuinely needs in-editor eyes — don't ship blind.
