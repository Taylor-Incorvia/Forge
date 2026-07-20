---
id: WA-058
status: todo
size: S
phase: 1-game-readiness
priority: 26
---
# Let the Battlecruiser roll Yamato

## What
The Battlecruiser (Starport slot 3) is currently blocked from rolling `Yamato`. Make it eligible.

## Data (from research)
- The exclusion: `addUpgradeRequirementTag("Yamato", logicType_NoneOf, "unitTag", "Battlecruiser")` (`upgradeInitializers.galaxy:196`, tagged WA-032). Yamato otherwise rolls on Factory s3 + Starport s3 units.
- **Why it was excluded:** the BC has Yamato **natively** — `AbilArray Link="Yamato"` on the unit (liberty), button `YamatoGun`.
- The rollable upgrade grants **`F_Yamato`** (MOD `AbilData.xml`), a **distinct ability id** whose effect is the **same `Yamato` effect** as the native.

## The catch (decide before building)
Simply removing the exclusion makes a rolled BC show a **second Yamato button** — separate cooldown, same strike (a duplicate, not a no-op). Two ways to go:

- **(a) Remove the BC's native Yamato** (hide/remove `Yamato` from its card, like WA-020/WA-028 did for other native abilities), so the rolled `F_Yamato` is the only one. Clean, no duplicate button, no hotkey clash. **Recommended.**
- **(b) Allow two Yamatos** — leave native, just delete the exclusion. On-theme chaos (double Yamato BC), but two identical buttons + a likely hotkey collision (native `YamatoGun` vs `F_Yamato`). Messier.

## Wiring
- Delete the `NoneOf Battlecruiser` line on Yamato.
- If (a): remove/hide the native `Yamato` from the Battlecruiser's command card so only the rolled one shows.

## Acceptance criteria
- [ ] A Battlecruiser can roll Yamato.
- [ ] Exactly one working Yamato button on a rolled BC (per the chosen option), no hotkey collision.
