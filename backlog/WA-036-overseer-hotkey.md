---
id: WA-036
status: todo
size: S
phase: 1-game-readiness
priority: 45
---
# Overseer hotkey won't render (Starport detector button)

## Symptom
The Overseer (Starport detector, trained via `StarportTrain,Train16`) shows **no hotkey at all** on the command card. It was `V` before (also not showing), changed to `C` per WA-005/WA-033 planning — still shows nothing.

## 🔍 What's set (all correct on paper)
- Card face is **`MorphToOverseer`** (the vanilla Overlord→Overseer morph button, repurposed as the Starport train face). `UnitData.xml`: `<LayoutButtons Face="MorphToOverseer" AbilCmd="StarportTrain,Train16">`; `AbilData.xml` StarportTrain Train16 `<Button DefaultButtonFace="MorphToOverseer"/>`.
- `GameHotkeys.txt`: **`Button/Hotkey/MorphToOverseer=C`** is present.
- The stock `MorphToOverseer` CButton has **no `<Hotkey>` field** (clean) — so it matches the "working pattern" (no explicit Hotkey + a GameHotkeys entry) and *should* resolve to C, but doesn't.

## Diagnosis — this is the F_Blink cursed-hotkey class
Same failure documented in `docs/static-prototype-attempt-1.md` §1: for certain **ability/morph-derived button faces**, the `GameHotkeys.txt` entry is silently not honored, for reasons never pinned down. The tell: unit-*train* faces (Stalker, Phoenix, Void Ray, Archon, Reactor) bind fine via the same GameHotkeys mechanism, but `MorphToOverseer` — a vanilla **morph** button — resists it, exactly like `F_Blink`.

## Fixes to try (cheap first; TIME-BOX to ~30 min per the docs)
1. **Check the local hotkey profile.** `Documents/StarCraft II/Accounts/<id>/<id>/Hotkeys/*.SC2Hotkeys` — if `MorphToOverseer` is listed unbound/stale there, the profile overrides mod data (this is what happened with F_Blink). Test on the **Standard** profile.
2. **Swap to a clean custom train button (most promising).** Give the Overseer its own unit-style button instead of the morph button:
   - Add `<CButton id="BuildOverseer">` with the overseer icon, **no `<Hotkey>` field**.
   - `UnitData.xml`: change the Starport `LayoutButtons Face="MorphToOverseer"` → `Face="BuildOverseer"`.
   - `AbilData.xml`: StarportTrain Train16 `<Button DefaultButtonFace="BuildOverseer"/>`.
   - `GameHotkeys.txt`: `Button/Hotkey/BuildOverseer=C`.
   - This mirrors the working unit-train buttons and sidesteps the morph-button baggage.
3. **If both fail in the time-box, accept no hotkey** — Overseer is a rarely-used detector default; not worth a rabbit hole (docs rule 7).

## Acceptance
- [ ] Overseer train button shows **C** in game — OR documented as "no hotkey, accepted" with the reason, if it hits the cursed wall.

## Notes
Low priority / clarity polish. Do NOT open the Starport `CUnit` in the editor data UI (it re-normalizes LayoutButtons — see §5). Related: [[reference-hotkey-resolution]], `docs/static-prototype-attempt-1.md` §1.
