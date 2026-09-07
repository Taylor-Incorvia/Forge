---
id: WA-111
status: todo
size: S
phase: 2-post-launch
priority: 2
---
# Cleanup: delete data for WoL upgrade abilities that never made it into any pool

Part of [[WA-108]]. Item 2. Pure hygiene — shrinks the WoL surface before the big removal.

## What
Several WoL-dependent *upgrade abilities* were built but are **not wired into any active pool** (their
`addAbilityToUpgrade` is commented out or never added). Delete their data outright — CAbil, F_ ability,
CAbilResearch `<X>1-4`, effects/behaviors/search actors, CButtons (`<X>` + `Research<X>1-4`), and
GameStrings. This is the upgrade analogue of what [[WA-082]] did for dead *units/abilities* (WA-082
explicitly did NOT cover these).

## Candidates (confirm each is truly unrollable + WoL-only before deleting)
- **PsionicShockwave** ("shockwave")
- **TempestDisruptionBlast** ("disruption blast" — the tempest stun)
- **OdinBarrage**
- **HERC grapple** — `Grapple` / `F_Grapple` (the "some herc ability"; commented out per upgradeInitializers ~393). Note `F_Yoink`/Abduct is a *different*, possibly-live thing — don't confuse them.
- Also sweep: `LightningBomb`, `DefensiveMatrix` (both commented in the caster section) — delete if WoL-only and unrollable.

## Watch-outs
- Verify "not in any pool" by grep for uncommented `addAbilityToUpgrade("<key>"` / `addUpgradeToUpgrade("<key>"`.
- WA-102 just added `Abil/Name/<key>1-4` + `Button/Name/Research<key>1-4` strings for these; deleting the
  abilities makes those strings dead too — remove them in the same pass (harmless if left, cleaner if gone).
- Command-card-button trap does NOT apply here — these buttons are on upgrade-facility research cards for
  unrollable upgrades, not static unit command cards. Safe to remove with the ability.
- No XML comments (editor mangles `--`).

## Acceptance
- [ ] Deleted ids gone from `GameData/*.xml`, `TriggerLibs/*.galaxy`, and localized strings (grep-verified).
- [ ] No active pool/roll changed (`testCaseNumber` sweep clean).
- [ ] Verify no new editor warnings on next load.
