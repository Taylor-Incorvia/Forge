---
id: WA-110
status: todo
size: M
phase: 2-post-launch
priority: 2
---
# `isWolDependent` upgrade marking + toggle to exclude WoL upgrades from all pools

Part of [[WA-108]]. Items 5 + 6 (one feature). Taylor asked specifically for a clean-approach recommendation.

## The clean approach (recommended)
Upgrades are registered in `initializeUpgrades()` (upgradeInitializers.galaxy) via
`addAbilityToUpgrade(key, ability)` / `addUpgradeToUpgrade(key, grant)` + requirement tags. There is no
per-upgrade "meta tag" mechanism the way there is for units, and WoL-ness isn't derivable — it's an
assertion about which granted ability references campaign data. So **declare it explicitly with a thin
wrapper**, exactly parallel to WA-109's `addWolUnitToSlotPool`:

- Add `bool excludeWolUpgrades` near `devMode` (commit `false`).
- Add `void addWolAbilityToUpgrade(key, ability)` and `void addWolUpgradeToUpgrade(key, grant)` that:
  1. record `key` in a `wolDependentUpgrades` list (self-documenting — the WoL set = whatever went
     through the wrapper), and
  2. call the underlying `addAbilityToUpgrade` / `addUpgradeToUpgrade` **only if `!excludeWolUpgrades`**.
- Route the WoL upgrades through the wrappers. Known WoL-dependent, still-live upgrades (Taylor's recall +
  audit): **Irradiate, SeekerMissile, MissilePods (Hurricane Missiles), ArbiterMPRecall** — plus verify
  BlindingCloud, CorsairMPDisruptionWeb, FungalGrowth, GravitonBeam, ForceField, GuardianShield against
  the dependency "Used By" report before tagging (some may be Void/Swarm, not campaign — don't over-tag).

**Why a wrapper, not the caster-tag pattern:** the caster regen rides on a tag list already populated
from unit-tags; WoL-ness has no such source. A wrapper keeps the declaration at the single registration
site (one line each), gates at the source (no post-hoc pool surgery), and the `wolDependentUpgrades` list
falls out for free (useful for the modal or WA-114's checklist).

## Confirm which upgrades are actually WoL-dependent
Don't guess — cross-check each candidate's granted ability/effect chain against `reference/` to see if it
resolves only from `liberty.sc2campaign`. Unrollable WoL abilities (shockwave, disruption blast, HERC
grapple, Odin barrage) are handled by deletion in [[WA-111]], not by this toggle.

## ⚠️ Scope caveat
Same as WA-109: gates **pool membership**, not command-card DATA. Gameplay isolation only, not the
escape-key experiment.

## Acceptance
- [ ] `excludeWolUpgrades = true` → no WoL upgrade appears in any upgrade pool; non-WoL upgrades unaffected.
- [ ] `wolDependentUpgrades` list matches the audited set.
- [ ] Toggle committed `false`.
