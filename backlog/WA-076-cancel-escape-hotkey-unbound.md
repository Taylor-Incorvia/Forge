---
id: WA-076
status: todo
size: S
phase: 1-game-readiness
priority: 30
---
# Cancel / Escape hotkey isn't bound on the default profile

## Symptom
On the **default hotkey profile**, the **Cancel** command appears to be bound to nothing — you can't cancel (e.g. a production order / build placement / menu) with the key you'd expect (Escape). Annoying, frequent, and a bad first impression.

## What we know (from the files)
- The mod's `enUS.SC2Data/LocalizedData/GameHotkeys.txt` is only ~76 lines and contains **only `Button/Hotkey/*`** entries for the mod's custom abilities. It does **not** define or override any `UI/Cancel_Hotkey` / cancel bindings.
- In stock SC2 (`reference/mods/core.sc2mod/.../GameHotkeys.txt`), Cancel is split across several keys, and notably it is **not** Escape by default:
  - `UI/Cancel_Hotkey=C`  (the command-card Cancel button)
  - `UI/CancelMulti_Hotkey=A`
  - `UI/GameMenuCancel_Hotkey=C`, `UI/StandardDialog_Cancel_Hotkey=C`
  - Escape is used for `UI/Hotkey/SelectionCancelDrag=Escape`, `UI/Hotkey/ChatCancel=Escape`, etc.
- So the cancel bindings should **inherit from core** — the mod isn't clearing them in GameHotkeys.txt. That means the breakage (if real) is coming from somewhere else, OR it's a stock-vs-expectation mismatch (see hypotheses).

## Hypotheses to check (in likely order)
1. **The command card is missing its Cancel button.** The mod heavily customizes production-building command cards (random-unit slots, add-on consolidated onto X, etc.). If a card's `LayoutButtons` omits the stock Cancel button (normally the bottom-right slot that appears during production/build/morph), then there is literally no Cancel button for the hotkey to act on — so "nothing happens." **Check the CommandCard / LayoutButtons for the production buildings** (Barracks/Factory/Starport + tech buildings) for a Cancel entry.
2. **Expectation vs. stock:** stock card-Cancel is **C**, not Escape. Confirm what the user is pressing and what they expect. If the ask is "make Escape cancel," that's a deliberate `UI/Cancel_Hotkey=Escape` (or profile) change, not a bug fix. Watch for a collision — Escape may already be reserved for menu/deselect.
3. **WA-033 collision fallout / a shipped custom profile.** The hotkey pass (WA-033) reworked bindings to avoid collisions. Verify it didn't clobber a cancel binding, and check whether the mod forces a custom hotkey profile that lacks Cancel. Related: WA-005, WA-033, and the hotkey-resolution notes (ButtonData `<Hotkey>` is source of truth; GameHotkeys.txt is derived/can be stale).
4. **Profile scope:** confirm whether this reproduces on Standard *and* Grid, and whether it's the localized `_USD`/`_GRS` variants that matter.

## Deliverable
- Root-cause which of the above it is.
- Fix so Cancel works on the default profile (either restore the missing card Cancel button, or bind cancel to the intended key — decide Escape vs. stock C with the user), without introducing a new collision.
- Note the fix + which file owns it, and re-run the hotkey collision check.

## Notes
- Publish-to-verify likely for card-button behavior (won't fully show in the local Test Document).
- If it's a missing card button, the fix probably touches multiple buildings' command cards — bump size to M.
