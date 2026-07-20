---
id: WA-056
status: todo
size: M
phase: 1-game-readiness
priority: 24
---
# Weapon projectile-count upgrades (Phoenix, Liberator)

## Feasibility verdict (researched)
Projectile count is **not** a single weapon field. `DisplayAttackCount` on the weapon is **cosmetic only**. The real count lives in the effect tree, in one of two shapes:

- **Persistent-based (multipliable ✅)** — count = `PeriodCount` × (number of launch entries). A `CUpgrade` can multiply `Effect,<id>,PeriodCount`.
- **Set-based (not multipliable ⚠️)** — count = number of `EffectArray` entries in a `CEffectSet`. No scalar; you must **append** array entries via catalog.

## Phoenix ✅ (clean)
Weapon `IonCannons` → `<CEffectCreatePersistent id="IonCannons">` with `PeriodCount=2` × 2 launch entries (`IonCannonsLMLeft`, `IonCannonsLMRight`) = **4 missiles per attack today** (the "2" you see is `DisplayAttackCount`, cosmetic). Each missile = 5 dmg (+5 Light).
- Upgrade multiplies `Effect,IonCannons,PeriodCount` ×2 → **8 missiles** (i.e. 4→8, and **DPS doubles**).
- ⚠️ **Confirm intent/magnitude:** you pictured 2→4, but it's already 4; a ×2 gives 4→8 and doubles Phoenix damage. Maybe want ×1.5, or accept the big buff. Also decide whether to bump `DisplayAttackCount` to match visually.

## Liberator ⚠️ (harder)
Sieged weapon `LiberatorAGWeapon` → `<CEffectSet id="LiberatorAGMissileLMSet">` with a **single** `EffectArray` (`LiberatorAGMissileLM`) = **1 projectile**. No `PeriodCount` to multiply.
- To reach ~5: **append 4 more `EffectArray` entries** to the set (catalog array-append, indices 1-4, all → `LiberatorAGMissileLM`). Each added entry = another full-damage missile, so **5 projectiles = ~5× damage** — very strong; likely needs its own damage retune.

## Suggested split
Ship **Phoenix first** (clean scalar multiply). Treat the Liberator as a follow-up given the array-append + the 5× damage balance problem. Both are rollable per-unit upgrades (AnyOf Phoenix / AnyOf Liberator) with the usual research UI + GameStrings.

## Open questions
- Phoenix multiplier (×1.5 vs ×2) and whether to touch `DisplayAttackCount`.
- Liberator: is 5× damage acceptable, or scale each missile's damage down so total is ~2×?
