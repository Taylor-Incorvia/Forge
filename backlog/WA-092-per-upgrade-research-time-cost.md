---
id: WA-092
status: todo
size: M
phase: 2-post-launch
priority: 65
---
# Per-(upgrade, slot) research TIME + COST — a tuning lever for over/under-powered rolls

## Why
Right now every upgrade researches for the same **120 game-seconds** (≈85.7s Faster; 3 outliers at 100) and **150/150**. That's too blunt. Some unit+upgrade combos scream "too strong for how fast/cheap it arrives" — the clearest is **Blink Zealot one-base all-in** (Taylor: "never seen it fail"; the problem is blink arriving *too early*, not blink being too strong). Same logic for premium late rolls (**Yamato on a Tempest** — fine to be strong, but cost/time should reflect it). Taylor wants to tune the *research* of specific rolls rather than nerf units or abilities globally.

## Great news: the mechanism already exists
`setSlotResearchPrice(player, upgradeKey, slot, amount)` (upgradeHelpers.galaxy ~175) already sets research **cost** dynamically, per-player, per-(rolled-upgrade + slot), via:
```galaxy
CatalogFieldValueSet(c_gameCatalogAbil, upgradeKey + IntToString(slot),
                     "InfoArray[Research1].Resource[Minerals]", player, amount);
```
Research **time** lives on the same ability, one field over: `InfoArray[Research1].Time`. So time control is a near-copy:
```galaxy
void setSlotResearchTime(int player, string upgradeKey, int slot, string timeValue) {
    CatalogFieldValueSet(c_gameCatalogAbil, upgradeKey + IntToString(slot),
                         "InfoArray[Research1].Time", player, timeValue);
}
```

## Granularity
- **Per (rolled-upgrade key + slot), per-player.** Meets Taylor's stated compromise ("slot number + ability is enough").
- **NOT per-facility** — the research ability `<upgradeKey><slot>` is shared across facilities for the same upgrade+slot. Taylor OK with this (rarely matters).
- Research time/cost lives on the **CAbilResearch ability** (`InfoArray[Research1].Time` / `.Resource[Minerals/Vespene]`), NOT on the CUpgrade. Static default: Time=120, cost=150/150.

## Implementation plan
1. Add `setSlotResearchTime(...)` (above).
2. Add a **lookup** `(upgradeKey, slot) -> {time, cost}` with defaults (120 / 150) and overrides only for screamers. Keep it a simple, readable config (a function of if/else or a data-table seed) — this is the dial Taylor turns as combos reveal themselves.
3. Wire it into faction setup — apply each slot's configured time+cost after upgrades are assigned (same spot as `assignUpgradesToAllSlotsWithinCaps` in `initializeUpgradePoolsForPlayer`, upgradeInitializers.galaxy ~603).
4. **Refactor the price-restore:** `onResearchCancelled` (upgradeHelpers ~211) hardcodes restore to "150" — route it through the lookup so cancels restore the *configured* price, not 150. (The 999999 re-research lock on start/complete stays.)
5. Test: confirm a configured combo (e.g. Blink slot 1) shows the new time+cost at the upgrade facility, and that cancel/re-research restores the configured price (not 150).

## First combos to tune (from play)
- **(Blink, barracks slot 1)** → longer research time (delay the one-base blink-Zealot all-in). Primary motivator.
- **(Yamato, starport slot 3)** → higher cost + time (premium late roll should cost accordingly). Ties to the recurring "Tempest + any upgrade feels strong" read — tune the *upgrade's research*, not the Tempest.
- Add others as they scream. Do NOT try to tune all ~200 combos up front — only the outliers.

## Notes
This is the granular lever Taylor flagged he wants to lean on going forward. Effort is MEDIUM (core plumbing already proven by setSlotResearchPrice); ongoing cost is just adding lookup lines. Related balance lens: [[balance-for-forced-creativity]] (tune the screamers, don't over-tune everything). Blink-Zealot context: [[WA-087]]. Tempest context: [[WA-090]].
