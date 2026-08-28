---
id: WA-091
status: todo
size: S
phase: 2-post-launch
priority: 40
---
# Rename Stalker → "Blink Stalker" everywhere in the UI

## What
Show the Stalker as **"Blink Stalker"** consistently in the UI (it's the mod's blink-centric Stalker; matches the flavor rename pattern like Roach → "Mech Roach").

## Current state (already partly done)
- ✅ `Unit/Name/Stalker=Blink Stalker` already set (GameStrings.txt ~line 418) — so the **selection panel / unit info** already reads "Blink Stalker."
- ❌ **The production-card TRAIN button still reads plain "Stalker" (or base "Warp In Stalker").** There is **no `Button/Name/Stalker` in the mod**, so the train button (face `Stalker`, on Barracks `BarracksTrain,Train7` and Factory `FactoryTrain,Train22`) falls back to base game strings. This is almost certainly where "Stalker" still shows on hover.

## Fix
Add to `enUS.SC2Data/LocalizedData/GameStrings.txt`:
```
Button/Name/Stalker=Blink Stalker
Button/Tooltip/Stalker=<train tooltip, optional>
```
(GameStrings, not XML — no XML-comment concern.) Then confirm both the Barracks and Factory train buttons show "Blink Stalker" (Stalker rolls in Factory slot 1 per current pools; the Barracks train entry exists in the card too).

## Also check (don't assume)
- Selection panel, command-card train button hover, and any alert/notification text.
- **Site `/units` shows "Stalker"** — that's the **separate website repo/agent** (generated export). Flag it there separately; NOT fixed by this mod-side change. See [[project-website-architecture]].

## Acceptance
- [ ] Train button on the production card reads "Blink Stalker."
- [ ] Selection panel still reads "Blink Stalker" (already true).
- [ ] Verify in Test Document (train buttons render locally — this is static data, not a UnitAbilityAdd button).
