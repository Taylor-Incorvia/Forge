# Static Pre-Declaration Prototype — Attempt 1

## ⚠️ READ THIS FIRST: DO NOT FUCK WITH HOTKEYS AGAIN ⚠️

**Future Taylor: do not, under any circumstances, attempt to fix the Forge mod's hotkey resolution. It is a bottomless pit. You will lose an entire day to it and emerge with nothing but rage.**

We spent multiple full sessions trying to make the Zealot's statically-declared Blink button show hotkey `B` instead of `G`. Every approach failed in a different and increasingly bizarre way:

- Removing `<Hotkey value="G"/>` from F_Blink CButton → still G
- Adding `<HotkeyAlias value="Blink"/>` → still G
- Removing the `<CButton id="Blink"><HotkeyAlias value="Ghost"/></CButton>` block → still G
- Hardcoding `<Hotkey value="B"/>` on F_Blink → **nothing** (no hotkey at all)
- Removing the hardcode and only using GameHotkeys.txt `Button/Hotkey/F_Blink=B` → **G**
- Removing `G=G` fallback → **nothing**
- Using `Face="Blink"` (vanilla button) on Zealot's LayoutButtons → **G** with Standard, **nothing** with Classic
- Adding diagnostic `<CButton id="Blink"><Hotkey value="Z"/></CButton>` → **nothing** (not Z, not G)
- Clearing GameHotkeys.txt entirely and switching between Standard and Classic profiles → **G** in Standard, **nothing** in Classic, neither matched any explicit setting

The pattern: **whenever we set an explicit hotkey value via mod data, the result was nothing instead of that letter; whenever we relied on inheritance, the result was G regardless of profile.** None of it made sense, none of it was reproducible, and none of the conventional SC2 modding patterns (CButton.Hotkey, GameHotkeys.txt entries, HotkeyAlias chains) worked the way the documentation says they should.

### The hypothesis we landed on but never confirmed

The button id `Blink` in vanilla SC2 is **probably a generic placeholder/template button, not the actual Stalker blink button.** The cooldown link `BlinkBlink5BlinkF_Blink` on F_Blink suggests vanilla SC2 has both a `Blink` ability AND a `Blink5` ability, where `Blink5` is the actual Stalker level-5 blink (post-research) and `Blink` is a leftover/template. The Standard SC2 profile binds `Blink5` to B but `Blink` (the template) has no proper binding, which is why it falls through to G in Standard and to nothing in Classic.

**This was never tested.** We came up with the theory at the very end of the session and the user reverted before trying `Face="Blink5"`. If the next attempt wants to test this hypothesis (which it probably should), the change is to use `Face="Blink5"` instead of `Face="Blink"` in the LayoutButtons entry on Zealot. But **for the love of fuck do not turn it into another full-day debugging session.** Try it once. If it doesn't immediately work, give up on hotkeys for the day and move on with whatever G fallback the engine gives you.

### Other things that made the day fucking miserable

- **The SC2 Editor destroys sparse indexed arrays in `CUnit.AbilArray` and `CUnit.CardLayouts.LayoutButtons`** every time you save a unit through the data UI. See section 5 below. This caused hours of "the button is gone — wait no it's there — wait no it's gone again" confusion.
- **The SC2 Editor sometimes writes back stale in-memory data when saving**, undoing external edits silently. Hitting Ctrl+S in the editor is a hostile action toward your work.
- **The SC2 Editor crashed at least once during the session.**
- **The SC2 Editor's data UI introduced its own broken syntax** (like `<Hotkey value="Button/Hotkey/Blink"/>`, a reference-syntax leftover) into F_Blink CButton when the user manipulated it.

### Practical rules for next time

1. **If the mod's Blink hotkey is showing G, accept G. Move on.** Your time is more valuable than the difference between B and G on one ability button.
2. **The Vulture pattern works for visibility** — `Type="AbilCmd"` + explicit `index` on AbilArray + explicit `Requirements=""` (even if empty) on LayoutButtons. Stick to that recipe for static declarations and DO NOT improvise.
3. **Get visibility working first, completely separately from hotkeys.** Test that the button appears, that the requirement gates it correctly, that the research lifecycle works — all with whatever hotkey the engine happens to give you. Only after the entire visibility flow is locked in should you even consider touching hotkeys.
4. **Never edit `CUnit.AbilArray` or `CUnit.CardLayouts.LayoutButtons` via the SC2 Editor's data UI.** XML edits only. The editor will normalize sparse arrays and destroy your work.
5. **Never hit Ctrl+S in the SC2 Editor between external edits.** The editor will overwrite your disk state with its in-memory copy.
6. **Hotkeys for ability buttons in this mod are cursed and the Blink button id situation is fucked.** This is not your fault, it's not Claude's fault, it's the result of years of dynamic-add workarounds layered on top of vanilla SC2 button id ambiguity. The only path forward is the static pre-declaration approach with vanilla buttons, accepting whatever hotkey the engine produces, and only investigating hotkey overrides as a final cleanup pass long after everything else works.
7. **If a hotkey investigation has consumed more than 30 minutes, stop. Document the symptom in this file. Do not continue.** Each round of "one more diagnostic" added another hour to a day that should have been spent on more productive work.

---

## Goal

Convert the Blink × Zealot × Rax slot 1 path from `UnitAbilityAdd` (dynamic) to fully static pre-declaration in XML, gated by per-player `CUpgrade` requirements. This was Phase 0 of the broader Option A plan in `~/.claude/plans/snappy-hopping-boole.md`.

End-state we wanted:
- A `GhostAcademy` whose Rax slot 1 randomly rolled `Blink` shows a Blink research button at column 0
- Queueing the research hides the button on all of that player's GhostAcademies until cancel/complete
- Completing it permanently hides the research button AND grants every existing/future Zealot a Blink button
- Zealot Blink button uses the player-profile-respecting hotkey for vanilla Blink (default = B)
- No `UnitAbilityAdd` calls in the data path

## What worked

1. **Static `AbilArray` + `CardLayouts/LayoutButtons` declaration on units.** Adding `<AbilArray index="N" Link="..."/>` to a unit works as a way to give the unit a new ability statically. Adding a matching `<LayoutButtons index="N" Face="..." AbilCmd="...,Execute"/>` puts a button on the command card at the specified Row/Column.
2. **`CmdButtonArray.Requirements` gating.** Setting `Requirements="Forge_Granted_BlinkReq"` on `F_Blink`'s `CmdButtonArray index="Execute"` correctly hides the button until the upgrade count is granted. Using the same requirement on `LayoutButtons.Requirements` also works.
3. **`CAbilResearch.InfoArray.Button.Requirements` works** — research button on GhostAcademy correctly hid/showed based on `Forge_Rax1_BlinkResearchReq`. Approach 7's claim in `completed-research-button-attempts.md` that `InfoArray.Requirements` is silently ignored was wrong: that field IS read when placed on the `<Button>` sub-node (matching the existing pattern at the user's `Blink4` line 1285).
4. **Per-facility per-slot research ability (`Forge_Rax1_Blink`) with `Upgrade="Forge_Granted_Blink"`** — SC2 natively grants the linked CUpgrade by 1 when the research level completes. This means Galaxy code is technically optional for the upgrade-grant; SC2 handles it.
5. **The full requirement chain** — `CRequirementCountUpgrade` → `CRequirementGTE` → `CRequirementAnd` / `CRequirementNot` → `CRequirement` with `NodeArray index="Show"` and `index="Use"` — produces a working show/use gate when wired correctly.
6. **Galaxy early-return pattern for Blink** in `grantGenericUpgrade` and `setSlotSelectedUpgrade` correctly skipped the dynamic `UnitAbilityAdd` path for Blink while keeping the per-player upgrade count flowing.
7. **Test hacks for forcing rolls** — modifying `setRandomSlotUnitFromPoolForPlayer` (force Zealot in Rax slot 1) and `assignRandomUpgradeFromPoolToPlayerSlot` (force Blink in Rax slot 1) both worked cleanly.
8. **`CUnit.AbilArray` max index = 32** (valid indices 0–31). Confirmed via probe entries: indices 16/20/24/28 worked, 32/36/40/44/48 produced `Unable to use index: N` warnings. Standard SC2 ceiling. With ~9 vanilla slots typical and ~22 needed at the worst-case caster (HighTemplar), we have margin for the full Phase 1 rollout.
9. **End-to-end flow with F_Blink** (the original prototype state). Visibility worked: Blink button on GhostAcademy showed when slot 1 rolled Blink, hid when researching, returned when cancelled, hid permanently when completed. Then F_Blink button appeared on Zealots only after research completion. The user confirmed the flow worked structurally.

## What didn't work / unresolved mysteries

### 1. Hotkey resolution for F_ ability buttons is broken in ways we couldn't pin down

We tried, in roughly this order:
- Removing `<Hotkey value="G"/>` from F_Blink CButton + adding `Button/Hotkey/F_Blink=B` to GameHotkeys.txt → showed G
- Adding `<HotkeyAlias value="Blink"/>` to F_Blink CButton, removing the F_Blink GameHotkeys entry → showed G
- Removing the existing `<CButton id="Blink"><HotkeyAlias value="Ghost"/></CButton>` block (which was the source of G via the alias chain F_Blink → Blink → Ghost) → showed G
- Removing the empty `Button/Hotkey/Blink=` line that the editor had auto-written → showed G
- Removing `G=G` fallback line + hardcoding `<Hotkey value="B"/>` on F_Blink + `Button/Hotkey/F_Blink=B` in GameHotkeys → showed **nothing** (no hotkey at all)
- Removing `<Hotkey value="B"/>`, leaving only `Button/Hotkey/F_Blink=B` → showed **G**
- Setting `Button/Hotkey/F_Blink=K` (unique diagnostic letter) → user reported F_Blink showing as "unbound" in the in-game customize hotkeys UI
- Switching `LayoutButtons Face="F_Blink"` → `Face="Blink"` (vanilla button face) and back, with various ability/button-face combinations → either G or nothing

The pattern that **does** work in this mod is the one used by `ResearchStalkerTeleport1`: a `CButton` with **no `<Hotkey>` field** and a direct `Button/Hotkey/<id>=<letter>` entry in GameHotkeys.txt. We replicated that exact pattern for F_Blink and it didn't work — we don't know why. The research button at the GhostAcademy correctly displayed B the entire time using this pattern.

**Possible explanations we never confirmed:**
- F_Blink might be cached as "unbound" in the user's local SC2 hotkey profile (`Documents/StarCraft II/Accounts/.../*.SC2Hotkeys`), and the profile takes precedence over mod data. The user found F_Blink showing as unbound in the in-game customize UI, which is consistent with this. If true, the fix is to clear the profile entry, not change the mod data.
- The SC2 Editor's data cache may not be reading our changes consistently. Multiple times we got results that contradicted what was on disk; sometimes the user observed the editor writing back stale data on save.
- `HotkeyAlias` for ability buttons (vs unit/production buttons) may not propagate hotkey-letter resolution the way it does for unit-tab grouping. Confirmation evidence: the existing `Phoenix → VikingFighter`, `Mutalisk → Banshee` aliases in the mod are clearly for unit-tab grouping, not hotkey inheritance, but we're not 100% sure which use case `HotkeyAlias` actually supports for ability buttons.

### 2. Vanilla Blink ability, statically declared on Zealot, doesn't render its button

When we switched Zealot from F_Blink to vanilla `Blink` (both AbilArray and LayoutButtons), the button stopped rendering entirely. The Forge research completed, the upgrade was granted, the requirement was satisfied — but no button.

**Suspected cause:** the existing override at `AbilData.xml:325` is incomplete:
```xml
<CAbilEffectTarget id="Blink">
    <CmdButtonArray index="Execute" Requirements=""/>
</CAbilEffectTarget>
```
SC2 mod XML overrides for `CmdButtonArray` entries appear to **replace** the entire entry (specifically the indexed `Execute` slot), not merge field-by-field. So this override left vanilla Blink with no `DefaultButtonFace` and no flags inherited from base game data, which broke the button render path.

**Attempted fix:** add `DefaultButtonFace="Blink"` to the override:
```xml
<CAbilEffectTarget id="Blink">
    <CmdButtonArray index="Execute" DefaultButtonFace="Blink" Requirements=""/>
</CAbilEffectTarget>
```
We applied this but the button still didn't show in the next test. Whether the user wanted to add `UseDefaultButton`/`CreateDefaultButton` flags became contentious — the user (correctly) noted those flags are dynamic-add machinery and shouldn't be needed for static declaration. We never got definitive confirmation of whether this fix was sufficient because the editor crashed and the user decided to revert.

### 3. The `G=G` line in GameHotkeys.txt was load-bearing for SOMETHING

Removing it changed the F_Blink test result from "shows G" to "shows nothing." That tells us `G=G` was the actual source of the G fallback all along, not any specific button hotkey config. With it removed, dynamically-added abilities and other unconfigured F_ buttons would have no hotkey at all — meaning the line should be re-added if any of the mod's still-dynamic F_ ability paths are expected to keep working.

**Recommendation:** keep `G=G` until the entire Phase 1 migration off `UnitAbilityAdd` is done. Then it can be safely removed.

### 4. SC2 Editor cache and crashes made debugging unreliable

Multiple symptoms:
- The editor sometimes wrote back its in-memory representation of files (containing old or different data) when saving, overwriting external edits
- The editor wrote `<Hotkey value="Button/Hotkey/Blink"/>` (a reference syntax) into F_Blink CButton when the user manipulated it via the editor UI — we cleaned this up later
- The editor crashed at one point, requiring file integrity verification
- "Close and reopen the document" was the only reliable way to make external edits land — and even that was inconsistent

### 5. **CRITICAL: SC2 Editor destroys sparse indexed arrays in `CUnit`**

This is the actual root cause of most of the bizarre symptoms we chased all day. We didn't discover it until the very end of the session by inspecting the disk state of `Zealot` and `GhostAcademy` directly.

**The editor normalizes `CUnit.AbilArray` and `CUnit.CardLayouts.LayoutButtons` into dense arrays.** When you save or even open a unit that has sparse indexed entries (e.g. `<AbilArray index="10" Link="F_Blink"/>` with no entries at indices 6–9), the editor:
- Fills the gaps with empty placeholder entries (`<AbilArray/>`, `<LayoutButtons Row="0" Column="0"/>`)
- **Strips the `index` attribute from your high-index entries**, so they end up appended at the end of the array with no explicit index
- Converts `removed="1"` entries to `Link=""` (similar but possibly different semantics)
- For LayoutButtons, replaces specific `index="N"` markers with `Type="Undefined" AbilCmd=""` and dumps your real entry at the end without an index

Once this normalization happens, your custom entries no longer render correctly. The LayoutButtons reference to `F_Blink`/`Blink` button is still in the file but stripped of its index attribute, and the renderer doesn't place it where you intended.

We confirmed this on both Zealot and GhostAcademy at the end of the session. Both units had:
- `AbilArray index="N" Link="..."` → expanded to dense `index="2"`, `index="3"`, then 6 empty `<AbilArray/>` placeholders, then our entry with no index
- `LayoutButtons index="6" removed="1"` → became `<LayoutButtons index="6" Face="" Type="Undefined" AbilCmd="" Row="0"/>` followed by 23 empty `<LayoutButtons Row="0" Column="0"/>` placeholders, then our entry with no index attribute

**Why we didn't catch this earlier:** every time we made an external edit and tested, we (or the user via the editor's data UI) eventually opened Zealot or GhostAcademy in the editor, which triggered the normalization. The next test would see the destroyed state. Our edits were correct on disk between tests; they just got wiped on the next editor open.

**Recommendations for next attempt:**
1. **Never edit `CUnit.AbilArray` or `CUnit.CardLayouts.LayoutButtons` via the SC2 Editor's data UI** for units with custom static-declared abilities. Make all edits to these structures via external XML editors only.
2. **If you must use the editor data UI for these units, verify the disk state immediately after.** If the editor normalized your entries, restore them via external edit before testing.
3. **Use sequential dense indices, not sparse high ones.** Our use of `index="10"`, `index="30"`, `index="50"` was specifically to avoid colliding with vanilla indices, but it backfired because the editor "normalized" the gaps. The next attempt should figure out the highest-used vanilla index per unit and append sequentially. (E.g. if vanilla Zealot uses AbilArray indices 0–4, our entries should be at indices 5, 6, 7... not 10, 20, 50.)
4. **Strongly consider not opening these CUnit entries in the editor at all between sessions.** Treat the XML edits as the source of truth and never let the editor "save" the unit.

This finding alone is worth the entire investigation — without it, the next attempt would have hit the same wall.

## Files modified in this attempt

All under `ForgeModLowConfidence.SC2Mod\`.

### Data XML (`Base.SC2Data\GameData\`)
- **`UpgradeData.xml`** — added `<CUpgrade id="Forge_Granted_Blink">` and `<CUpgrade id="Forge_Selected_Rax1_Blink">`, both `MaxLevel=1`
- **`RequirementNodeData.xml`** — added 7 nodes:
  - `CountUpgradeForge_Selected_Rax1_BlinkCompleteOnly`
  - `GTECountUpgradeForge_Selected_Rax1_BlinkCompleteOnly1`
  - `CountUpgradeForge_Granted_BlinkCompleteOnly`
  - `GTECountUpgradeForge_Granted_BlinkCompleteOnly1`
  - `CountUpgradeForge_Granted_BlinkQueuedOrBetter`
  - `GTECountUpgradeForge_Granted_BlinkQueuedOrBetter1`
  - `NotGTECountUpgradeForge_Granted_BlinkQueuedOrBetter1`
  - `AndForge_Rax1_BlinkResearchAvailable`
- **`RequirementData.xml`** — added `Forge_Rax1_BlinkResearchReq` and `Forge_Granted_BlinkReq`
- **`AbilData.xml`** — added `<CAbilResearch id="Forge_Rax1_Blink">` (modeled on Blink1, with `Upgrade="Forge_Granted_Blink"`); added `Requirements="Forge_Granted_BlinkReq"` to `F_Blink`'s `CmdButtonArray`; modified the existing vanilla `Blink` override at line 325 to add `DefaultButtonFace="Blink"`
- **`UnitData.xml`** — added `AbilArray index="10" Link="Forge_Rax1_Blink"` and `LayoutButtons index="30"` to GhostAcademy; added `AbilArray index="10" Link="Blink"` (last state — was `F_Blink` earlier) and `LayoutButtons index="30"` to Zealot
- **`ButtonData.xml`** — removed the `<CButton id="Blink"><HotkeyAlias value="Ghost"/></CButton>` block; modified `F_Blink` CButton extensively across the debugging session (removed `<Hotkey value="G"/>`, tried `HotkeyAlias`, tried hardcoded `<Hotkey value="B"/>`, etc.). Final state: clean, no Hotkey, no HotkeyAlias

### Localized (`enUS.SC2Data\LocalizedData\`)
- **`GameHotkeys.txt`** — removed `Button/Hotkey/Blink=G`; removed `Button/Hotkey/F_Blink=G` (added `=B`, then `=K` for diagnostic, then removed entirely); **removed `G=G` fallback line**

### Galaxy (`Base.SC2Data\TriggerLibs\`)
- **`upgradeHelpers.galaxy`**:
  - `setSlotSelectedUpgrade`: special-cased `Blink && rax && slot==1` → calls `grantUpgrade(player, "Forge_Selected_Rax1_Blink")` then `return` to skip the dynamic `addToAbilityListForUnit` path for Blink only
  - `grantGenericUpgrade`: special-cased `upgrade == "Blink"` → calls `grantUpgrade(player, "Forge_Granted_Blink")` then `return` to skip the dynamic ability-add path for Blink
- **`upgradeInitializers.galaxy`** — `assignRandomUpgradeFromPoolToPlayerSlot` test hack: forces "Blink" for Rax slot 1 regardless of pool roll
- **`forgeProductionFacilityHeplers.galaxy`** — `setRandomSlotUnitFromPoolForPlayer` test hack: forces "Zealot" for Rax slot 1 regardless of pool roll

### Plan file (outside the mod)
- **`~/.claude/plans/snappy-hopping-boole.md`** — the original plan for the prototype, still valid for the next attempt

## Key architectural insight (worth keeping)

The original purpose of the `F_` ability prefix system was to work around `UnitAbilityAdd`'s hotkey flattening — when an ability was dynamically added to a unit at runtime, the hotkey was forced to G regardless of any config, AND it would also affect units that already had the underlying ability. The `F_` versions were parallel buttons that could be set to G safely without affecting vanilla casters.

**With static pre-declaration, that whole problem evaporates.** Each unit's static command card is independent. Adding vanilla Blink to Zealot's AbilArray gives Zealot blink with the vanilla button (and vanilla hotkey). Stalker's command card is untouched.

**`F_` buttons remain useful only for genuine hotkey collision cases** — e.g., if HighTemplar gains NeuralParasite as an upgrade but the player wants HighTemplar's NeuralParasite hotkey to be different from Infestor's. Then a parallel `F_NeuralParasite` button with a different hotkey makes sense.

For all non-collision cases (the vast majority), static declaration of vanilla abilities is the right move. This is the design we should commit to in the next attempt.

## Recommendations for the next attempt

1. **Start fresh from main**, not from the current state. The current state has too many half-applied changes to reason about.
2. **Check the user's local SC2 hotkey profile before assuming mod data is wrong.** The path is `Documents/StarCraft II/Accounts/<account_id>/<player_id>/Hotkeys/*.SC2Hotkeys`. If F_Blink is listed there with an unexpected binding, the profile is the source of truth and no amount of mod data tweaking will override it. Clear stale entries before testing.
3. **Use vanilla buttons by default, F_ only for collisions.** This means: add vanilla `Blink` to Zealot's AbilArray, use `Face="Blink"` in LayoutButtons. The hotkey naturally inherits from vanilla → player profile.
4. **Before relying on the existing vanilla Blink override**, verify it actually works for Stalker in the current mod. If Stalker can't blink either (because the override stripped DefaultButtonFace), the override needs to be either reverted or made structurally complete. Consider testing both with and without the override.
5. **Keep `G=G` in GameHotkeys.txt** until the entire dynamic-add path is migrated. It's load-bearing for the F_ buttons that haven't been moved yet.
6. **Avoid making structural data edits via external tools while the SC2 Editor has the document open.** The editor's in-memory cache will fight you. Either work entirely in the editor's data UI, or close the document before external edits.
7. **For the prototype slice, prefer one full slice working end-to-end** (research button + ability button + correct hotkey + visibility gating + research lifecycle) before generalizing. We had the visibility/lifecycle working but kept getting derailed by the hotkey rabbit hole. Decoupling them next time would help — get visibility working with a known-acceptable hotkey first, then tune the hotkey separately.
8. **Document `CUnit.AbilArray` max = 32** in CLAUDE.md or similar — this is reusable knowledge for the broader Phase 1 rollout.
9. **The Forge_Rax1_Blink research ability with `Upgrade="Forge_Granted_Blink"` is sound** — SC2 natively grants the linked CUpgrade on research completion, so the Galaxy code path is technically optional for prototyping. Keep this pattern in the next attempt.
10. **Avoid adding `UseDefaultButton`/`CreateDefaultButton` flags to ability CmdButtonArrays for static declarations.** Those flags are for the dynamic `UnitAbilityAdd` path. Static declaration via LayoutButtons does not require them — the user's instinct on this was correct.

---

## Addendum (2026-07-14): Hellion Factory-card column — a clean, isolated instance of the local-vs-production divergence

**New data point found during the WA-033 hotkey sweep** (forcing each slot to a specific unit via `testCaseNumber`). This is the cleanest example yet of the local-Test-Document-vs-production discrepancy, because it's a single wrong attribute with a known-correct production baseline.

### Symptom
In the local Test Document, **Hellion (Factory slot 1) renders in the SECOND command-card column**, covering whatever Factory slot 2 rolled. On **published/production builds it renders correctly in column 0** — the user has built Hellions many times in production with no overlap. Same class of bug as Stalker-not-showing-Blink and Vulture-not-showing-KD8: **local render ≠ production render.**

### Root cause (concrete, in source)
`UnitData.xml`, Factory `CUnit` → `CardLayouts` → the Hellion entry was **missing its `Column` attribute**:
```xml
<LayoutButtons index="15" Face="Hellion" AbilCmd="FactoryTrain,Train6" Row="0"/>   <!-- no Column! -->
```
Every other Factory **slot-1** unit has explicit `Column="0"` (Vulture, Stalker, Ravager, Roach, Predator). This is a leftover from Hellion's historical move from slot 2 → slot 1: the move never wrote an explicit `Column="0"`, it just left the attribute absent. (Confirmed the Barracks card, by contrast, sets `Column` explicitly on every train button — which is likely why Barracks never shows this bug.)

### Why this matters for the "caching" mystery — a testable hypothesis
Production resolves a **missing** `Column` → `0` (correct). The local Test Document renders it as column **1**. The most likely explanation, consistent with §4/§5 above: **the editor holds a stale/normalized cache, and the divergence surfaces specifically on OMITTED / defaulted attributes** — anything without an explicit value in source falls back to the editor's cached representation (here, the old slot-2 `Column="1"`), while an explicit value forces the render. This unifies all three known cases:
- Hellion — omitted `Column` → falls back to stale cached column.
- Stalker Blink / Vulture KD8 — buttons added at runtime via `UnitAbilityAdd`, i.e. **no explicit static card entry at all** → nothing to render locally.

Common thread: **no explicit static value in source ⇒ local editor uses stale cache / renders nothing; production resolves fresh.** If this holds, the general workaround is *set every button attribute explicitly in XML; never rely on defaults or dynamic add for anything you need to see locally.*

### Fix applied as a live diagnostic (2026-07-14)
Made the attribute explicit to match its siblings (safe — production was already correct, so this can only help or be neutral there):
```xml
<LayoutButtons index="15" Face="Hellion" AbilCmd="FactoryTrain,Train6" Row="0" Column="0"/>
```
**Next test tells us a lot:**
- If local now renders Hellion in **column 0** → confirms "explicit values bypass the cache, omitted ones leak it." Concrete workaround established.
- If local **still shows column 1** despite explicit `Column="0"` → the cache ignores even explicit source data, which is a deeper (worse) problem and points back at the profile/editor-cache theories in §1/§4.

### Caveat
Per §5: **do NOT open the Factory `CUnit` in the editor's data UI** — it will re-normalize `CardLayouts.LayoutButtons` and may strip the `Column` again. XML edits only; close the document before editing externally.

### ✅ CONFIRMED (2026-07-14, same day) + swept for siblings
Relaunched with the explicit `Column="0"` → **Hellion renders in slot 1 locally.** Hypothesis **confirmed**: explicit attributes render correctly in the local Test Document; omitted ones leak the stale cache. **Established workaround: set every command-card button attribute explicitly.**

Swept all of `UnitData.xml` for the same bug class (train `LayoutButtons` with a missing `Row`/`Column`) and fixed 5 more that would have misplaced a button and covered its slot-mate locally:
| Unit | Facility slot | Was | Fixed to |
|---|---|---|---|
| Immortal | Factory s2 | (no Row, no Col) | `Row="0" Column="1"` |
| Colossus | Factory s3 | (no Row, no Col) | `Row="0" Column="2"` |
| Viper | Starport s3 | (no Row, no Col) | `Row="0" Column="2"` |
| Medic | Barracks s3 | `Column="2"` (no Row) | `Row="0" Column="2"` |
| SiegeTank | Factory s2 | `Column="1"` (no Row) | `Row="0" Column="1"` |

Every Barracks train button was already fully explicit — which is exactly why the Barracks card never exhibited this. **Fragility:** these all live in `CUnit.CardLayouts.LayoutButtons`, which the editor re-normalizes on data-UI open (§5) and may strip again. If a train button starts misplacing locally after an editor session, re-add its explicit `Row`/`Column`. XML-only.

#### ⚠️ Correction (2026-07-14, later that day) — the sibling sweep was WRONG; only Hellion was real
The user reverted the 5 sibling fixes (Immortal / Colossus / Viper / Medic / SiegeTank) — they had **already tested those slots in-game and the buttons rendered fine** despite the omitted `Row`/`Column`. **Only Hellion was a genuine display bug** (its `Column="0"` fix was kept). So "omitted attribute → local mis-render" is **too broad**. Refined model: the local stale-cache leak needs the editor to hold a **historically DIFFERENT cached value** for that button — Hellion was physically moved slot 2 → slot 1, so the cache still had the old `Column="1"`. An attribute that was simply never set, with no prior conflicting position, defaults correctly even in local test. **Bug = (omitted/defaulted attribute) AND (a prior different cached position). Omission alone is not enough.** Do not proactively "fix" omitted Row/Column on buttons that render correctly.
