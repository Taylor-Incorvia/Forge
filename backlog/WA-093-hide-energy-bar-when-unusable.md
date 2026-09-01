---
id: WA-093
status: todo
size: M
phase: 2-post-launch
priority: 35
---
# Remove the energy bar from fighter-casters that can't spend energy (dead-energy = Feedback liability)

## Why
A fighter-caster that rolled a **non-energy** upgrade (e.g. a Wraith or DuskWing that rolled Range instead of a caster spell / cloak) still shows a full energy bar it can never use. Two problems:
1. **Deceptive** — the bar implies an ability the unit doesn't have.
2. **Feedback liability** — High Templar Feedback deals damage equal to the target's *current energy*. A unit with energy it can't spend is a prime Feedback target for free; the energy is pure downside.

Taylor (2026-08-31): "If you cannot spend your energy, then your energy is nothing but a Feedback liability." Not urgent — documenting for later, no testing done yet.

## The real determinant (not the caster tag)
Hide/remove energy when the unit has **no way to spend it given its current roll**. Energy sinks:
- a **native** energy ability (always present regardless of roll), OR
- a **rolled caster-spell** upgrade (energy ability), OR
- a **rolled cloak** upgrade (cloak drains energy).

If none of those → the energy bar is dead → remove it.

## Which units are actually affected (verified 2026-08-31)
Hybrid casters (tagHybridCaster, roll BOTH pools): **Queen, Ghost, Wraith, Phoenix, CorsairMP**.
- **Keep energy always** (native energy ability): Queen (Transfusion), Phoenix (Graviton Beam), Corsair (Disruption Web).
- **Conditional** (no native energy ability; energy only via rolled cloak/caster spell): **Wraith, Ghost**.
- **DuskWing** — NOT caster-tagged (rolls fighter upgrades only), but it's a Banshee so it carries an energy bar. Dead unless it rolled cloak. Taylor named Wraith + DuskWing; Ghost belongs in the same bucket, confirm in play.

## Approach (mirror WA-092's faction-setup apply)
At faction setup (after units + upgrades assigned — same hook as `applyAllResearchConfigForPlayer`), for each energy-bar unit above, check whether it has an energy sink for its rolled upgrade; if not, **zero out its max energy per player** via `CatalogFieldValueSet(c_gameCatalogUnit, unitId, <energy-max field>, player, "0")`.

Why zero max energy rather than just hide the UI: it removes the bar **and** the Feedback liability (Feedback value = current energy; 0 energy = 0 damage to the caster and nothing for the enemy to Feedback). Hiding only the UI leaves the Feedback exploit intact — so zeroing is the correct fix.

## Needs verification before building
- Exact catalog field for max energy (likely a Vital / `EnergyMax`-style field on CUnit) — confirm the `CatalogFieldValueSet` path in the editor (same class of unknown as WA-092's `InfoArray[Research1].Time`).
- Confirm zeroing max energy actually hides the bar (vs. showing an empty 0/0 bar).
- Classification of "rolled an energy sink": reuse the caster-spell detection (upgrade tagged AllOf "caster") + explicitly include the cloak upgrade(s).
- Make sure native-energy units (Queen/Phoenix/Corsair) are never zeroed.

## Notes
Per-player, per-roll — the roll is fixed for the game, so set once at setup. Related: [[WA-092]] (same faction-setup apply pattern + CatalogFieldValueSet-field caveat), [[reference-research-ability-per-slot-shared]]. Cloak is 1-per-faction (WA-049), so a cloak-rolled unit keeping energy is fine.
