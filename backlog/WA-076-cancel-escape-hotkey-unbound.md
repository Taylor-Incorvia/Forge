---
id: WA-076
status: todo
size: L
phase: 1-game-readiness
priority: 55
---
# Cancel/Escape unbinds when switching hotkey profiles IN-GAME (data-level command-card conflict)

## STATUS: root cause CORRECTED 2026-08-16 — deprioritized (narrow impact, has a one-click workaround)
The original theory in this ticket (**the Liberty Campaign dependency causes it**) is **DISPROVEN**. So is every other clean hypothesis we tried. After a full A/B investigation (2026-08-12 → 2026-08-16), the real cause is a **data-level hotkey conflict inside this mod's own command-card content**: when the game re-resolves the **Standard** profile against the mod's command set on an **in-game profile switch**, it drops the Cancel binding.

It is **fixable** (it's our data, not the client, not the dependencies), but the impact is narrow and there's a trivial player workaround, so it's deprioritized behind Season 1 gameplay work. **Verify any future fix on a PUBLISHED build — the editor's Test Document shows a fake hotkey environment and lied to us for days.**

## Symptom (corrected — this is the precise trigger)
- Entering the mod with **Standard already selected** (from the main menu before the game) → **Escape cancels normally. No bug.**
- The bug fires **only when a player changes hotkey profiles WHILE IN A GAME** — e.g. a player on a custom profile switches to **Standard** mid-match. That switch leaves **Cancel bound to nothing** (and reportedly scrambles some other keys too), so Escape no longer cancels building/production/abilities. The only cancel left is clicking the command-card button.
- **Recovery is one click:** Options → Hotkeys → **Restore Race Defaults** → Accept. (Restore Race Defaults loads the mod's own hotkey defaults, which are correct — confirming the mod's *defaults* are fine and it's the *Standard re-resolution* that breaks.)

## Who hits it / why it mattered
Roughly the ~10% of players who change hotkeys mid-game — disproportionately Terran players who fiddle. **PiG hit it live:** he's left-handed and uses a custom profile, got into the mod, was told "you have to use Standard hotkeys for mods like this," switched to Standard **in-game**, and hit the dead-Escape bug — it noticeably hurt his experience. That stream moment is the origin of this whole investigation.

## Root cause (PROVEN): a hotkey conflict in the mod's command-card data
On an in-game switch to Standard, the client re-resolves all hotkeys against the mod's command set and **drops Cancel**. A blank Void Multi mod, Patches (also a melee extension mod), and standard multiplayer **all survive the exact same switch** with Escape intact — **only this mod's content triggers it.** Therefore the cause is in our `Base.SC2Data/GameData` command-card data, not the client, not the dependency stack, not GameHotkeys.txt.

### The control experiment that proved it (2026-08-16)
Published a **blank Void Multi mod** to production, entered with a custom profile, switched to Standard in-game → **Escape survived.** Same result for Patches and for standard MP. Wildcard is the only one that breaks → it's our data content.

## What it is NOT (full elimination log — do not re-try these)
Every one of these was tested and ruled out; most were verified on **published** builds:
1. **NOT the Liberty (Campaign) dependency.** Extracted the 7 WoL units into our own data and dropped the campaign dep (see [[WA-078]]); bug persisted.
2. **NOT Void (Mod).** Removed it from `DocumentHeader` (Void Multi includes it transitively; `DocumentInfo` already listed Void Multi only); bug persisted. Note: `Void (Mod)` is base melee data, NOT a "story/campaign" layer.
3. **NOT map-vs-mod structure.** Patches is *also* a melee extension mod and survives profile switches. So being an extension mod isn't the cause.
4. **NOT the "Story" hotkey category.** The 6 extracted F_ units carried `<HotkeyCategory value="Unit/Category/TerranStory"/>` (a category *value*, not an id — the F_ rename never touched it), which is why they showed under "Terran Story" in the hotkey menu. Retargeted all 6 to `Unit/Category/TerranUnits` (commit `0b23a8e`); the "Terran Story" category disappeared from the menu **but the Escape bug persisted.** The Story category was cosmetic.
5. **NOT GameHotkeys.txt.** Emptied the entire file to 0 bytes (diagnostic branch `wa-076-diag-empty-gamehotkeys`, commit `1deaa77`), published, tested the switch → **still broken.** So no custom `Button/Hotkey/*` entry (including the load-bearing `G=G` line) causes it.
6. **NOT a data-layer Cancel override.** Setting `UI/Cancel_Hotkey=Escape` (+ `_USD` / `_USDL` variants) in the mod's GameHotkeys did nothing (commit `81439f1`). The client's **profile** system is authoritative for this, not mod data — which is also why "Escape cancels" in retail is a client Standard-preset remap, not anything in the game data (all data layers bind card-Cancel to `C`).
7. **NOT the editor / not the client / not the account.** All early confusion came from the editor's Test Document faking the hotkey environment. On real published builds the blank mod / Patches / standard MP all work, so it isn't a universal client limitation and isn't the user's account or profile state.

## Strongest lead for the actual fix (start here)
It's a **hotkey collision in the command-card data.** Facts gathered on `main`:
- The mod does **not** override the `Cancel` or `CancelBuilding` CButtons in `ButtonData.xml`, and there is **no** `Button/Hotkey/Cancel` in `GameHotkeys.txt` — so the Cancel command's own key is inherited from base and isn't remapped. The conflict is therefore **another custom button sharing Cancel's key/slot**, exposed only when Standard re-resolves.
- The mod heavily customizes command cards: **13** card layouts define a `Face="Cancel"` button and **5** define `Face="CancelBuilding"` (`UnitData.xml`), e.g. Armory/Barracks with `Face="Cancel" AbilCmd="que5,CancelLast"` and `Face="CancelBuilding" AbilCmd="BuildInProgress,Cancel"`.

Concrete fix procedure (all on a **PUBLISHED** build):
1. Reproduce: enter on a custom profile, switch to Standard in-game.
2. Open Options → Hotkeys on Standard and find the **RED (conflicted) commands** — whatever is fighting over Cancel's key is the culprit.
3. Trace that button back to its `ButtonData.xml <Hotkey>` and/or the command-card `LayoutButtons` entry, and resolve the collision (rebind the offending custom button, or explicitly pin Cancel).
4. Re-test the switch; confirm no regression to the other cancels (`CancelBuilding`, `CancelLast`/que5) or to deselect/menu Escape.
- This is effectively a targeted collision hunt; related prior collision work: [[WA-005]] (collision audit), [[WA-033]] (collision fix pass).

## Player-facing workaround (ship this in README / FAQ / Discord NOW)
- **Prevent it:** pick **Standard** from the main menu **before** entering a game — it works perfectly this way.
- **If it already happened** (you switched mid-game and Escape stopped cancelling): Options → Hotkeys → **Restore Race Defaults** → Accept. One click, persists.

## Branches & evidence trail
- `wa-076-diag-empty-gamehotkeys` — **KEEP.** Empty-GameHotkeys diagnostic that proved GameHotkeys is not the cause. Commit `1deaa77` (branches off the WA-078 branch).
- `wa-078-integrate-wol-units-drop-campaign-dependency` — campaign extraction + dep drop + Story-category fix (`0b23a8e`) + Void (Mod) removal (`0ab4041`). **Has regressions — do NOT merge as-is:** Tech Reactor is broken (`Unable to create duplicate entry` / `Unable to find parent F_TechReactor` — from the insert-before extraction leaving duplicate ids), blank DuskWing icon, and duplicate-entry XML warnings. See [[WA-078]] for the shelved-until-needed decision.
- `wa-076-cancel-escape-hotkey` / PR #40 — earlier abandoned data-only attempt (predates the corrected root cause).
- Superseded/misleading commits based on the disproven campaign theory: `6b43d44` ("root cause PROVEN via A/B"), `65b2bf8`, `ce798f3`.

## Acceptance
- [ ] On a **published** build: switch custom → Standard **in-game** and Cancel stays bound to Escape.
- [ ] No regression to CancelBuilding / CancelLast / deselect / menu-Escape.
- [ ] Player workaround documented (README/FAQ/Discord) regardless of when the code fix lands.

## Notes
- **VERIFY ON PUBLISHED BUILDS ONLY.** The editor Test Document misrepresents hotkeys and cost this investigation several days of dead ends.
- Everything above (the 7-point elimination + control experiment) is documented so nobody re-runs the dead ends. The remaining work is the command-card collision hunt in step "Strongest lead."
