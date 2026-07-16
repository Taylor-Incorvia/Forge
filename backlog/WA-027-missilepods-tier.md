---
id: WA-027
status: done
size: S
phase: 1-game-readiness
priority: 23
---
# MissilePods: decide low-tier vs high-tier caster, then restrict the pool

## ✅ Done 2026-07-16
**Tier:** LOW-TIER (see below). **Damage:** removed the Light bonus → flat 60 to all air.
- Set `AttributeBonus index="Light" value="0"` on `HurricaneMissileDamage` (mod `EffectData.xml`). **Must be an explicit 0**, not a deleted line — the base campaign effect is `+5`, which would bleed through the catalog merge otherwise.
- Net: 10 missiles × 6 = **60 flat vs all air** (each missile splashes r=1.6), no light skew → stays useful vs heavy/armored/capital air late game. Energy 75, range 7.

## 🔨 Tier restriction done 2026-07-16
**Decision: LOW-TIER.** Added `NoneOf rax4` + `NoneOf starport3` on the MissilePods registration (`upgradeInitializers.galaxy`), mirroring Transfusion.
- **Still rolls on:** Queen (s2), Sentry + Medic (Barracks s3), Corsair + Phoenix + Wraith (Starport s1).
- **Removed from:** Ghost / Infestor / HighTemplar (Barracks s4), Raven / Viper (Starport s3). Doc updated.

**Current damage — full breakdown (confirmed via Liberty campaign effect chain):**
- Per missile (`HurricaneMissileDamage`, mod-overridden in `EffectData.xml`): `Amount = 6`, `AttributeBonus Light = 3`.
- Missile count: `HurricaneMissileDamagePersistent` `PeriodCount = 10` → **fires 10 missiles**.
- **Total: 60 base / 90 vs Light** (60 +30 bonus). Air-only (`SearchFilters="Air;..."`). Energy 75, range 7.
- Base campaign per-missile was 4 (=40 total); the mod already bumped it to 6.

**To retune:** change `Amount`(6) and/or Light `AttributeBonus`(3) on `HurricaneMissileDamage` — each unit is ×10 for the total. (Count lives in the base `HurricaneMissileDamagePersistent`/`HurricaneMissile` `PeriodCount` if you ever want fewer/more missiles.)

**Open:** user to decide whether to retune now that it's a low-tier spell.

`MissilePods` currently has only `AllOf caster` (no slot gate), so **every** caster and hybrid can roll it — low tier and high tier alike. It might be too strong to be that broadly available.

## Why
Wide availability of a strong burst-AA spell dilutes the roll pools and may be overpowered. It should belong to one tier, not all of them.

## The decision
- Is MissilePods a **low-tier** caster spell (Barracks slot 3 / Starport slot 1 casters) or a **high-tier** one (Barracks slot 4 / Starport slot 3 casters)?
- Pick one, then add slot-tag restrictions so it only appears in that tier's pool.

## Acceptance criteria
- [x] Decide the tier. → LOW-TIER
- [x] Add `AnyOf`/`NoneOf` slot-tag requirements to the `MissilePods` registration in `upgradeInitializers.galaxy` so it only rolls for the chosen tier.
- [x] Confirm the pools: it no longer appears for the other tier. (doc regenerated)
- [x] Damage tuning: removed Light bonus (explicit 0), kept `Amount`(6) → 60 flat vs all air.

## Notes
Flagged during WA-019 (Queen pool review) — the comment is already in `upgradeInitializers.galaxy` at the MissilePods registration.
