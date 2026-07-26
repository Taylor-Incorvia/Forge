---
id: WA-068
status: todo
size: M
phase: 1-game-readiness
priority: 14
---
# Ravager → Barracks slot 3 + Corrosive Bile buff upgrades

## Why
Ravager was tried at **Factory slot 2** and felt weak vs the other factory-s2 options — and factory s2 is already crowded (~7 units). Moving it to **Barracks slot 3** gives it a home where it stands out. Corrosive Bile is a delayed AoE skillshot — it rewards reading the fight and aiming, which is dead-on for the "react to your hand" identity. Pair the re-slot with a signature, **obvious** bile upgrade or two.

## Part A — re-slot (easy)
- Move Ravager from Factory slot 2 to Barracks slot 3 in the slot pools (`initialize.galaxy`). One-list change.
- Sanity-check what else lives in Barracks s3 so the tier makes sense; confirm the roster/pool sizes still balance.

## Part B — Corrosive Bile buff upgrade(s) (the fun part)
Per-unit catalog upgrades gated `AnyOf Ravager` (families so caps count them as one). Prefer the **visible** ones:
- **Bigger bile splash radius** — most visible (matches "obvious upgrades"); also slightly offsets how hard bile is to land. Top pick.
- **Double bile damage** — keeps it hard to hit but devastating ("leave it hard to hit, just have it fuck shit up"). Strong second.
- **Longer bile range** — least visible; skip unless the others aren't enough.
- Catalog refs: the bile effect's `AreaArray[0].Radius` (splash), the damage effect `Amount` (damage), the launch/ability `Range`. Same `CUpgrade` mechanics as the other stat upgrades.

## Design intent
Give Ravager a signature identity in Barracks s3: a skillshot siege caster whose rolled upgrade makes the skillshot *matter* — a huge splash you can see, or a hit that deletes a clump.

## Acceptance criteria
- [ ] Ravager rolls in Barracks slot 3; no longer in Factory s2; pools/caps still fill correctly.
- [ ] At least one bile upgrade (splash or damage) rolls on Ravager and visibly/feelably changes the bile.
- [ ] Corrosive Bile roll-cap/family still behaves (bile is a capped family).

## Notes
Low priority — **not before the "Your Faction" modal**. Part A is quick; Part B is the value. Corrosive Bile eligibility history: WA-040. This is the "existing unit re-slot," not a new unit (user is not adding new units pre-S2).
