---
id: WA-027
status: todo
size: S
phase: 1-game-readiness
priority: 23
---
# MissilePods: decide low-tier vs high-tier caster, then restrict the pool

`MissilePods` currently has only `AllOf caster` (no slot gate), so **every** caster and hybrid can roll it — low tier and high tier alike. It might be too strong to be that broadly available.

## Why
Wide availability of a strong burst-AA spell dilutes the roll pools and may be overpowered. It should belong to one tier, not all of them.

## The decision
- Is MissilePods a **low-tier** caster spell (Barracks slot 3 / Starport slot 1 casters) or a **high-tier** one (Barracks slot 4 / Starport slot 3 casters)?
- Pick one, then add slot-tag restrictions so it only appears in that tier's pool.

## Acceptance criteria
- [ ] Decide the tier.
- [ ] Add `AnyOf`/`NoneOf` slot-tag requirements to the `MissilePods` registration in `upgradeInitializers.galaxy` so it only rolls for the chosen tier (mirror how other caster upgrades gate on `starport+"3"` / `rax+"4"` etc.).
- [ ] Confirm the pools: it no longer appears for the other tier.

## Notes
Flagged during WA-019 (Queen pool review) — the comment is already in `upgradeInitializers.galaxy` at the MissilePods registration.
