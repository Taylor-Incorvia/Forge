---
id: WA-062
status: done
size: S
phase: 1-game-readiness
priority: 30
---
# Increase F_BuildAutoTurret cast range — it's unusable on ground units

**Done 2026-07-22:** `AbilData.xml` `CAbilEffectTarget F_BuildAutoTurret` `<Range>` bumped `2 → 5` (rolled `F_` version only; Raven native untouched). One-line data change; committed to main.

## Why
When a **ground** unit rolls Build Auto Turret (`F_BuildAutoTurret`), the ability feels un-castable. Two things compound:
- **You can't drop it under yourself.** The Raven is an *air* unit, so the turret's ground footprint doesn't collide with it — a Raven can place a turret right beneath itself. A ground caster occupies the ground the turret needs, so the spot directly under (and immediately around) you is blocked.
- **Range is only 2.** With the under-you spot unavailable *and* a 2-range cast radius, there's a tiny ring of valid placement — you can't cast where you're standing, and you can't reach far enough to cast anywhere useful either. Net feel: "the ability doesn't work."

## The fix (one-line data change)
`AbilData.xml` → `CAbilEffectTarget id="F_BuildAutoTurret"` (line ~508):
```xml
<Range value="2"/>   <!-- bump to ~5 -->
```
- Bump `Range` from **2** to roughly **5** (tune to taste — high enough that you can comfortably drop a turret ahead of a ground caster, in front of an advancing army, without walking on top of the target spot). Start at 5, dial up if it still feels cramped.
- This is the **rolled `F_` override only.** The Raven's native `BuildAutoTurret` is a separate ability and is explicitly excluded from the Raven (`upgradeInitializers.galaxy:38`, `NoneOf Raven`), so this change cannot affect the Raven.
- Data-only. No galaxy, no button/string changes. The `Marker`/`Placeholder`/`PlaceUnit` visuals already exist and follow the cursor; only the cast radius changes.

## Acceptance criteria
- [ ] Roll Build Auto Turret onto a **ground** unit; you can place a turret a comfortable distance ahead of the caster (not just in a thin ring around it).
- [ ] The dead-zone-directly-under-you problem no longer makes the ability feel un-castable.
- [ ] Raven's native auto turret unchanged.

## Notes
Trivial enough to fold into any build. Sibling to the other `F_`-rolled ability tuning. If placement *directly under a ground unit* is ever wanted, that's a separate (harder) footprint/placement problem — out of scope here; extending range solves the actual pain.
