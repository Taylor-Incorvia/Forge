---
id: WA-033
status: done
size: M
phase: 1-game-readiness
priority: 28
---
# Fix hotkey collisions (from WA-005 audit)

## ✅ Resolved 2026-07-14
Full in-game sweep (Standard profile) via the `testCaseNumber` 1–7 tool. All collisions fixed and verified in-game:
- **Addon consolidation:** all addon-build buttons → `X` (Barracks Tech Lab already X; Factory Reactor `C→X`; Starport Tech Reactor `Z→X`). Reactor leaving `C` also cleared the Colossus/Reactor clash.
- **Factory:** Stalker `S→F`, Lurker (`LurkerMP`) `E→R`, Archon `N→A`.
- **Starport:** Phoenix `X→E`, Void Ray `V→G`, Dusk Wings `I→F`, Corsair `O→E`.
- **Barracks:** already clean, no changes.
- **Gotchas hit:** the Lurker change had to land on face id `LurkerMP` (not `Lurker`/`LurkerFromHydraliskBurrowed`); Corsair needed a re-save to persist.
- **Overseer** wouldn't accept a hotkey (F_Blink cursed-morph-button class) → split out to **WA-036**.

Cleanup before commit: reset `devMode = false` + `testCaseNumber = 0` in `nativeHelpers.galaxy` (the sweep code stays — it's gated and inert).

---

Full re-pass on production hotkeys. PiG played the mod before several units were added, and the WA-005 audit read `GameHotkeys.txt` (a **derived, possibly-stale** file), so it can't be trusted. This ticket grounds everything in **what actually renders in-game under the Standard hotkey profile** — you record the letters, I compute the conflicts.

## Why we don't trust the editor / GameHotkeys.txt
- The **default hotkey** is set by `<Hotkey>` in **ButtonData.xml** (edited in the editor); the editor then syncs `GameHotkeys.txt`. `HotkeyAlias` is only for players on a *custom* profile.
- We already caught a desync: `GravitonBeam`/`GuardianShield` read `V` (fixed) while `ForceField`/`Transfusion`/`BlindingCloud`/`Yoink` still read `G`. So: **observe in-game, don't infer from files.**

## Setup (do this once)
1. In-game: **set hotkey profile to Standard.**
2. `devMode = true` (`nativeHelpers.galaxy`).
3. `testCaseNumber = 1` (`nativeHelpers.galaxy`).

## The loop (no Claude needed between runs)
Launch → build all 3 facilities → for each of the 10 slots, read the letter off the **train button** and write it in the recording sheet. If the slot's unit is a **caster** (flagged below), also jot its **ability** hotkeys. Then bump `testCaseNumber` (1→2→…→7) and relaunch. That's it — hand me the filled sheet when done.

Each slot forces its **Nth unit** (wrapping for slots with fewer options). 7 runs cover all 37 units. `↺` = repeat of a unit you already saw, skip it.

### What each run rolls
- **N=1:** B1 Zergling · B2 Hydralisk · B3 Firebat · B4 Ghost · F1 Vulture · F2 Diamondback · F3 ThorAP · S1 CorsairMP · S2 Liberator · S3 Raven
- **N=2:** B1 Zealot · B2 Marine · B3 Marauder · B4 Infestor · F1 Hellion · F2 Immortal · F3 Ultralisk · S1 Phoenix · S2 Mutalisk · S3 Tempest
- **N=3:** B1 Zergling↺ · B2 **Queen** · B3 **Sentry** · B4 **HighTemplar** · F1 Stalker · F2 SiegeTank · F3 Colossus · S1 Wraith · S2 DuskWing · S3 **Viper**
- **N=4:** B1 Zealot↺ · B2 Hydralisk↺ · B3 Medic · B4 Ghost↺ · F1 Vulture↺ · F2 WarHound · F3 ThorAP↺ · S1 VikingFighter · S2 VoidRay · S3 Battlecruiser
- **N=5:** F2 **Archon** — *every other slot repeats; only record Factory slot 2*
- **N=6:** F2 LurkerMP — *only record Factory slot 2*
- **N=7:** F2 Goliath — *only record Factory slot 2*

So runs 1–4 are the real work; 5–7 are three quick relaunches just to catch Factory slot 2's tail.

---

## Recording sheet (fill in the letters)

**Barracks** _(4 slots — every unit's train key must differ from the other 3 slots)_
| Slot | Unit | Train key | Caster ability keys |
|---|---|---|---|
| 1 | Zergling | | |
| 1 | Zealot | | |
| 2 | Hydralisk | | |
| 2 | Marine | | |
| 2 | Queen | | Transfusion: |
| 3 | Firebat | | |
| 3 | Marauder | | |
| 3 | Sentry | | ForceField: __ GuardianShield: __ |
| 3 | Medic | | |
| 4 | Ghost | | |
| 4 | Infestor | | FungalGrowth: __ Neural: __ |
| 4 | HighTemplar | | Feedback: __ PsiStorm: __ |

**Factory** _(3 slots)_
| Slot | Unit | Train key | Caster ability keys |
|---|---|---|---|
| 1 | Vulture | | |
| 1 | Hellion | | |
| 1 | Stalker | | |
| 2 | Diamondback | | |
| 2 | Immortal | | |
| 2 | SiegeTank | | |
| 2 | WarHound | | |
| 2 | Archon | | |
| 2 | LurkerMP | | |
| 2 | Goliath | | |
| 3 | ThorAP | | |
| 3 | Ultralisk | | |
| 3 | Colossus | | |

**Starport** _(3 slots)_
| Slot | Unit | Train key | Caster ability keys |
|---|---|---|---|
| 1 | CorsairMP | | DisruptionWeb: |
| 1 | Phoenix | | GravitonBeam: |
| 1 | Wraith | | |
| 1 | VikingFighter | | |
| 2 | Liberator | | |
| 2 | Mutalisk | | |
| 2 | DuskWing | | |
| 2 | VoidRay | | |
| 3 | Raven | | SeekerMissile: __ AutoTurret: __ |
| 3 | Tempest | | |
| 3 | Viper | | BlindingCloud: __ Yoink: __ ParasiticBomb: __ Consume: __ |
| 3 | Battlecruiser | | |

---

## What I do with the sheet
Two conflict checks (only these matter — same facility, different slots; same-slot units and cross-facility units never coexist):
1. **Rule 1 — train keys:** within each facility, flag any letter shared by units in *different* slots.
2. **Rule 2 — casters on `G`:** flag any **native caster ability** on `G` (that's the reserved key for rolled `F_*` upgrade abilities — collision the moment that unit rolls a spell). For each real one, I'll suggest a replacement letter that doesn't clash with anything else on that unit's own card.

Then the actual fixes are minimal `<Hotkey>` edits **in the editor** (ButtonData), changing as few keys as possible.

## Cleanup before commit
`testCaseNumber = 0`, `devMode = false`. (The sweep code stays — it's devMode+testCaseNumber gated and inert.)

## Caveat
`docs/static-prototype-attempt-1.md`: hotkey resolution here is historically a rabbit hole. One fix at a time; if one fights back >30 min, note it and move on.
