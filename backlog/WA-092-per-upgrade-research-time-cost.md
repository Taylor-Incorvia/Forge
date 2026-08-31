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

## Granularity tiers Taylor wants (design expansion, 2026-08-29)
Now that it's clearly doable via triggers, Taylor wants a *layered* config, cheapest-to-maintain first. Design the lookup so most ground is covered by broad rules and per-combo overrides are rare:
1. **Global upgrade lists** — an "extended-research" list and a "shrunk-research" list keyed by upgrade. E.g. Concussive Shells → *shrunk* (fast); Yamato → *extended* (slow). One list membership tunes an upgrade everywhere it rolls. **This should cover most cases.**
2. **Per-(upgrade, slot)** — as already scoped above.
3. **Per-unit factor** — the same upgrade should cost more on a stronger host. "Range on a **Tempest** should take longer than range on a **Hydralisk**." Implies an optional per-unit multiplier/override layer on top of the upgrade's base time.
4. **Per-(unit, upgrade)** — most granular. Taylor wants to **avoid** relying on this ("no time for 200 combos"); support it only as a rare escape hatch.

Design goal: cover ~everything with tiers 1 + 3 (upgrade lists + a few per-unit bumps); use 2 and 4 sparingly.

## Concrete targets identified
- **Yamato Cannon → longer research (and likely costlier).** Also a *reputation* problem, not just balance: opponents facing **Thor + Yamato** have twice told Taylor "your mod sucks" in one hour. Yamato is a good upgrade — it just arrives too easily. (Ref: BC default build time in this mod = **70 catalog / 50s Faster**, StarportTrain Train4.)
- **All Tempest upgrades → longer + costlier.** Taylor's thesis: it's Tempest *with upgrades* that's OP, not the Tempest itself.

## Possible Tempest strategy pivot (do NOT do yet — needs this system first)
Taylor is considering: **revert the just-shipped Tempest unit nerf** (v0.7.1 raised it 250/175 → 275/200) and instead nerf **all Tempest upgrades** via longer/costlier research, leaving the Tempest itself at its old 250/175. Rationale: raising the *unit* price is a band-aid — the upgrades are what's OP, so nerf the upgrades. **Sequencing (important): do not un-nerf the Tempest unit until this research-nerf system exists AND the Tempest-upgrade research nerfs are in place — otherwise you un-nerf the unit with no compensating nerf.** Tension to hold: earlier reasoning (see [[balance-for-forced-creativity]]) was "stock unit costs assume no upgrades exist, so a unit that gains upgrades is genuinely undercosted" — which argues the unit bump is *also* legitimate. Decide at build time whether to revert the unit nerf or keep both.

## REQUIRED companion: surface build/research times on the "Your Faction" modal
Once research time/cost vary per roll, players must be able to **see** them to plan — otherwise a longer/costlier research is an invisible gotcha. The natural place is the faction modal's hover tooltip.

**Finding (2026-08-29):** the modal does NOT reuse the live command-card tooltip. `factionModal.galaxy` builds **custom tooltip text** via `DialogControlSetPropertyAsText` (unit row = name + a "Build:" building-chain line, `fm_addUnitRow` ~167; upgrade row = GameStrings tooltip text via `getUpgradeTooltipKey`/`getUpgradeFallbackTooltip`, `fm_addUpgradeRow` ~180). So **per-player `CatalogFieldValueSet` overrides do NOT appear in the modal automatically** — the custom text would show stale/uniform info.

**So this must be built alongside WA-092:** add lines to the modal tooltips showing the *actual* values for that player — unit **build time**, and upgrade **research time + cost** — sourced from the WA-092 lookup (or read back per-player via `CatalogFieldValueGet`). Neither build time nor research time/cost is shown in the modal today, so this is additive display work regardless, but it becomes *mandatory* the moment times stop being uniform. Treat as a blocking sub-task of WA-092, not optional polish.

## Notes
This is the granular lever Taylor flagged he wants to lean on going forward. Effort is MEDIUM (core plumbing already proven by setSlotResearchPrice); ongoing cost is just adding lookup/list lines. Related balance lens: [[balance-for-forced-creativity]] (tune the screamers, don't over-tune everything). Blink-Zealot context: [[WA-087]]. Tempest context: [[WA-090]].
