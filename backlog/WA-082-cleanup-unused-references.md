---
id: WA-082
status: done
size: S
phase: 1-game-readiness
priority: 40
---
# Cleanup pass: remove references to units/abilities that can't roll

## What
Strip dead references for content that is **not in any active production pool** and isn't wired to anything live. Hygiene only — no behavior change for anything a player can actually roll. Branch: `wa-082-cleanup-unused-references` (off main).

Explicitly **NOT** in scope: this is not the "remove all Wings of Liberty content" branch. Goliath/Medic/Firebat/Wraith/Vulture/Diamondback/DuskWing, Irradiate, Missile Pods, and the Starport Tech Reactor all stay — they're active. (See [[WA-078]] for the shelved dependency-drop work.)

## Remove (all confirmed dead — not in any uncommented `addUnitToSlotPool`)
- **WarPig** — CUnit, its BarracksTrain `Train18` entry, the `HireKelmorianMiners` train button + card slot, `setUnitUnlockUpgrade` mapping, commented pool line, and its hotkey/strings.
- **ScienceVessel** — CUnit, StarportTrain `Train13`, `BuildScienceVessel` card slot, faction icon/name entries, `tagPureCaster`, unlock mapping, commented pool line. `NanoRepair` card ref goes with the unit.
- **ArbiterMP (unit)** — StarportTrain `Train14`, `Arbiter` train button + card slot, faction icon/name, `tagPureCaster`, unlock mapping, commented pool line.
- **ArbiterMP StasisField chain** — the whole dead ability: `ArbiterMPStasisField` / `F_ArbiterMPStasisField` abils, `ArbiterMPStasisField1-4` research, `ArbiterMPStasisFieldSearch` effect, `ArbiterMPStasisField(+TimedLife)` behaviors, `F_ArbiterMPStasisField` + `ResearchArbiterMPStasisField1-4` buttons, and their strings. (Its `addAbilityToUpgrade` is commented out.)
- **bare Lurker / LurkerBurrowed** (the old BW forms — NOT the active `LurkerMP`) — both CUnits, BarracksTrain `Train17`, the barracks card slot pointing at it, the empty `CWeaponLegacy id="Lurker"` stub, commented unlock mapping, `Button/Hotkey/Lurker`, `Weapon/Name/Lurker`, `Weapon/EditorPrefix/Lurker`.
- **DarkTemplarBlinkAttackDelay** — orphan `CBehaviorBuff` stub, referenced by nothing.
- **Dark-blink leftover `<On>` lines** — three line-level leftovers inside otherwise-active actors (`BlinkOriginModel2`, `BlinkStopModel2`, `CombatVisibilityClearer2` in ActorData.xml): the `ModelSwap DarkBlinkOriginModel` / `DarkBlinkStopModel` and `Effect.DarkTemplarBlink` events. Remove only those lines; keep the parent actors.

## KEEP (dead-sounding names that are actually live — do NOT remove)
- **Irradiate** — now an active grantable upgrade (`Irradiate1-4`, `F_Irradiate`, `ResearchIrradiate1-4`), independent of the Science Vessel.
- **ArbiterMP *Recall*** — active grantable upgrade (`addAbilityToUpgrade("ArbiterMPRecall", "F_ArbiterMPRecall")` is uncommented, gated to starport-3 / rax-4 casters). Keep `F_ArbiterMPRecall`, `ArbiterMPRecall1-4`, `ArbiterMPRecallSearch`, buttons, icons, strings.
- **`LurkerFromHydraliskBurrowed` button** (+ its tooltip/hotkey) — live via the factory LurkerMP `Train20` InfoArray `DefaultButtonFace`. Only the *barracks* Train17 card slot that used it is dead.
- **LurkerMP family** — `LurkerMP`, `LurkerMPBurrowed`, `BurrowLurkerMP(Up)`, `LurkerRange` upgrade. All active (factory slot 2).
- **There is no "dark Nexus"** — the only Nexus is the normal active Protoss Nexus. Nothing to remove there.
- **DefilerMP / DarkSwarm** — out of scope (its own unit; pool add commented but unlock mapping still active — leave for a separate decision).

## Acceptance
- [~] No editor errors/warnings introduced — **verify on next editor load** (can't check from outside the editor).
- [x] Removed ids no longer appear in `GameData/*.xml` or `TriggerLibs/*.galaxy` (except generated `Preload*`). Verified by grep.
- [x] Kept ids (Irradiate, ArbiterMPRecall, LurkerFromHydraliskBurrowed, LurkerMP*) all still present. Verified by grep.
- [x] Active pools/rolls unchanged — no active `addUnitToSlotPool` line was touched; only dead `setUnitUnlockUpgrade`/commented lines removed. (Empirical `testCaseNumber` sweep is the user's final confirmation.)

## Resolution (2026-08-17)
Executed on branch `wa-082-cleanup-unused-references`. Removed WarPig, ScienceVessel, the ArbiterMP unit + its dead StasisField ability chain, the old bare Lurker/LurkerBurrowed, and the orphaned dark-blink leftovers across UnitData/AbilData/ButtonData/EffectData/BehaviorData/WeaponData/ActorData + the galaxy trigger libs + localized strings. Kept (verified live): Irradiate, ArbiterMP **Recall**, the shared `LurkerFromHydraliskBurrowed` button, and the whole LurkerMP family. XML tag balance confirmed (the 59/58 CUnit count is the self-closing `<CUnit id="Stalker"/>`).

**Left as harmless out-of-scope orphans** (dead defs nothing references now; a future micro-pass could remove): the requirement/upgrade defs `barracks2warpigupg`/`barracks2warpigtreq`, `starport3arbiterreq`, `starport3sciencevessel(req|upg)`, `barracks4lurkerreq`; and the now-orphaned `DarkBlinkOriginModel`/`DarkBlinkStopModel` actor definitions (their referencing `<On>` lines were removed).

**On next editor load:** confirm no new warnings, and (optional) run a `testCaseNumber` sweep to confirm every slot still rolls a valid unit.

## Notes
Catalog produced 2026-08-17. `PreloadAssetDB.txt` / `Preload.xml` are generated caches — not hand-edited; the editor rebuilds them.
