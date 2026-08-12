---
id: WA-079
status: todo
size: M
phase: 2-depth
priority: 75
---
# Multi-Lock upgrade — fire air + ground weapons simultaneously (any dual-weapon unit)

## Why
A rolled upgrade that lets a unit attack **air and ground at the same time** (different targets at once), instead of picking one weapon per moment. On-thesis for the decoupled-upgrade design (any upgrade on any eligible unit) and a readable, exciting power spike. The mechanic already exists in WoL as the Goliath's **Ares-Class / Multi-Lock Targeting Systems** — deliberately dropped from the Goliath extraction (WA-078 went simplified) to avoid the campaign tangle, but the mechanic itself is worth reviving as a general upgrade.

## Eligibility
Any unit with **separate air and ground weapons** — Goliath (Hellfire Missiles + Autocannon), Thor, Tempest (`Tempest` + `TempestGround`), and any future dual-weapon unit. Filter by "has both an air weapon and a ground weapon" — units with a single dual-target weapon don't qualify (nothing to multi-lock). Small but interesting pool.

## Implementation sketch
- The campaign `MultilockTargetingSystems` behavior/upgrade is what enables simultaneous dual-weapon fire (granted by Ares-Class). Source: `reference/campaigns/liberty.sc2campaign/` (behaviordata / upgradedata / weapondata).
- Adapt as a rolled upgrade: `addBehaviorToUpgrade`/`addUpgradeToUpgrade("MultiLock", …)` + `addUpgradeRequirementTag` gated to dual-weapon units, per the standard upgrade-wiring pattern.
- Tricky part: making "fire both weapons at once" generic. The campaign version is Goliath-specific weapon plumbing (a per-weapon "can fire while another weapon fires" allowance). May need per-unit weapon-flag tweaks rather than one universal behavior — investigate whether a shared behavior can flip that flag on all of a unit's weapons.

## Notes
Low priority, post-S1 depth. Deferred from **WA-078** (Goliath extraction went simplified — no multi-lock). Reference ids: `MultilockTargetingSystems`, `AresClassWeaponsSystem`, `GoliathAUpgraded`/`GoliathGUpgraded`.
