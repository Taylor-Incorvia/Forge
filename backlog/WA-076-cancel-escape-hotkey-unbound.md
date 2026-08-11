---
id: WA-076
status: todo
size: M
phase: 1-game-readiness
priority: 25
---
# Cancel command ships UNBOUND on the Standard hotkey profile (Escape doesn't cancel)

## Symptom (confirmed by Taylor, in-game)
Open the mod for the first time on **Standard hotkeys** and the **Cancel** command is bound to **nothing** (shows red / "no hotkey" in Options → Hotkeys). Consequences: you can't cancel an SCV's building, a production order, or e.g. a Void Ray's Prismatic Alignment with a key — the *only* way to cancel is clicking the command-card / UI button.

- **Profile-specific.** It's the **Standard** profile that ships broken. **Classic is unaffected** — Winter Gaming played the mod on stream on Classic hotkeys and never hit it. (Open question: did Classic dodge it, or had he already fixed it via SC Evo — see below.)
- **Correlated with a dependency.** Taylor recalls it started when he added the **Wings of Liberty (Campaign) dependency** — before that (VoidMulti only) Escape cancelled normally. Not 100% certain, but it's the strongest lead.
- **Not unique to this mod.** Someone in the **SC: Evo Complete** Discord reported the same issue (and gave up without fixing). A shared root cause is likely, so a real fix here probably mirrors theirs (and vice-versa).

## Per-player workaround (document this for players NOW — README/FAQ/Discord)
Until it's fixed at the mod level, each player fixes it once:
1. Options → Hotkeys.
2. Select any unit that can cancel (Barracks, Armory, Void Ray, etc.).
3. Click the **Cancel** command — it shows **red / no hotkey**.
4. Set Hotkey → press **Escape** → Save.
Escape now cancels for good (persists across games).

## What the data actually says (why this is confusing)
Verified against `reference/`:
- **All game-data layers bind card-Cancel to `C`, not Escape:** `UI/Cancel_Hotkey=C` in `core.sc2mod` (plus profile-variant keys `UI/Cancel_Hotkey_USD=C`, `UI/Cancel_Hotkey_USDL=C`). **VoidMulti and the Liberty campaign define NO Cancel lines and nothing on Escape** — they inherit core's `C`.
- So the "Escape cancels" behavior in normal LotV multiplayer is **NOT in the mod data** — it comes from the **retail client's built-in "Standard" hotkey preset**, which remaps card-Cancel to Escape on top of the data default.
- The mod's own `enUS.SC2Data/LocalizedData/GameHotkeys.txt` only defines `Button/Hotkey/*` for custom abilities — it **never touches `UI/Cancel_Hotkey`**. So Cancel resolution falls through entirely to the inherited layers + the client preset, and in this mod's dependency stack the Standard preset's Escape→Cancel mapping ends up **empty** (not even falling back to `C`).
- Dependency stack (from `DocumentHeader`): **Liberty (Campaign) → Void (Mod) → Void Multi (Mod)**. The presence of the Liberty campaign dep is consistent with the "campaign vs ladder hotkeys differ" class of bug (community-confirmed: some commands correctly bound for Ladder are unbound under Campaign).
- **Editor is misleading:** in the editor it *looks* like Cancel is bound to Escape, but the published build ships it unbound — which is why past fix attempts (Taylor has done a couple rounds) looked fine in-editor but didn't take. **Verify on a PUBLISHED build, not the editor view.**

## Lead fix hypothesis (test this first)
Explicitly set the Cancel hotkey in the **mod's own top-level `GameHotkeys.txt`** (highest precedence, so it overrides the messy inherited/preset resolution):
```
UI/Cancel_Hotkey=Escape
UI/Cancel_Hotkey_USD=Escape      # + whichever profile-variant suffix = Standard
```
- Determine which `_USD` / `_USDL` / (Grid) `_GRS` suffix corresponds to the **Standard** preset and set that variant too — the base key alone may not cover the active profile. Enumerate the variants that exist and cover Standard.
- Also check **`UI/CancelMulti_Hotkey`** (cancel-last-queued) and the dialog/menu cancels — set consistently if they're affected.
- Risk: colliding with another Escape use (deselect/menu). Verify no collision after.
- If the top-level override doesn't take, investigate **dependency order** (does giving VoidMulti precedence over Liberty for hotkeys restore it?) — but reordering deps is higher-risk, treat as plan B.

## Deliverable
- Root-cause confirmed on a published build; Cancel bound to Escape out-of-the-box on **Standard** (and not broken on Classic/Grid).
- Fix documented (which file/keys own it), collision check re-run.
- Ship the per-player workaround in player-facing docs until the real fix lands.

## Notes / links
- Community context: campaign-vs-ladder hotkey divergence is a known SC2 class of problem — s2editor-guides "Standard Dependencies", and a medium writeup on SC2's hotkey system.
- Related: WA-005 (hotkey collisions), WA-033 (collision fix pass), hotkey-resolution notes (ButtonData `<Hotkey>` = source of truth; GameHotkeys.txt derived/can be stale). Publish-to-verify.
