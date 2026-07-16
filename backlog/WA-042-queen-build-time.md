---
id: WA-042
status: done
size: S
phase: 1-game-readiness
priority: 32
---
# Reconsider Queen build time (likely too long)

## ✅ Done 2026-07-16
Retuned the whole Barracks slot-2 pool so none of the three is especially cost- or supply-efficient:
- **Queen build 40 → 32s** (`AbilData.xml` BarracksTrain `Train21` Time). Real time on Faster ≈ 22.9s.
- **Marine build 17 → 15s** (BarracksTrain `Train20` Time). Real time on Faster ≈ 10.7s.
- **Queen supply 2 → 3** (`UnitData.xml` `<Food value="-3"/>`, new override) — the lever to stop a cheap-ish 175-HP self-healing caster from being supply-efficient late game.
- **Queen cost unchanged at 175/50** (user chose to keep price; nerf via supply instead).

All catalog `Time` values are standard-speed; multiplayer Faster = ÷1.4.

## Problem
The Queen is the Barracks slot-2 option with by far the longest build time, and it feels too slow.

| Unit | Cost (M/G) | Build time |
|---|---|---|
| Marine | 50/25 | 17s |
| Hydralisk | 100/50 | 26s |
| **Queen** | **175/50** | **40s** |

40s is ~54% longer than the Hydralisk and 2.35× the Marine. She's also the most expensive and slowest-moving unit in the slot, so the long build compounds an already-slow, back-loaded unit. She's a tanky hybrid caster (175 HP + Transfusion + roll pool), so *some* premium is fair — but 40s may be overtuned.

## Where it lives
Build times are **mod values**, not base: `Base.SC2Data/GameData/AbilData.xml` → `BarracksTrain` train ability, the Queen's `InfoArray` (`Train21`) `Time` field. (Base `TrainQueen` was 50s; the mod already lowered it to 40.)

## Decide
Pick a new Time. Rough anchors: matching the Hydralisk (26s) is probably too fast for a 225-cost tanky caster; something in the **~30–34s** range keeps her the slowest in the slot without feeling punishing. User to choose the exact value.

## Acceptance criteria
- [x] Pick a new Queen build time and set it in `BarracksTrain` `Train21` `Time`. (32)
- [ ] Confirm in-game it still trains slower than Marine/Hydra (intended) but no longer feels dead-slow.

## Notes
Data from the slot-2 stat comparison (2026-07-15). Only touches the train ability's `Time` — no CUnit card-layout edits, so no §5 risk.
