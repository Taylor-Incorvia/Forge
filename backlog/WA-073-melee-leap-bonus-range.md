---
id: WA-073
status: todo
size: M
phase: 1-game-readiness
priority: 20
---
# Melee units get brief bonus range after leap (anti-kite)

## Why
A melee unit that rolls leap can be kited into uselessness. Real game: leap Zealots vs. a single Marine doing ring-around-the-Rosie on a Barracks — the Zealot leaps, lands on the Marine, but the Marine has already walked out of range before the attack windup finishes, so it **never lands a single hit** and just leaps again. Infuriating, and it makes leap feel bad on the units where it should feel great.

## Design intent
- **Grant a brief bonus to weapon range immediately after a leap** so the landing attack actually connects even if the target keeps moving. Something like **+0.5–1 range for ~0.5s** after the leap resolves.
- **Range, NOT attack-windup reduction** (decided). Windup-reduction only fixes "no time to swing"; it still whiffs if the target simply keeps walking out of range. A short range bump is *forgiving* — it tolerates the target's continued movement, which is the actual failure mode here.
- **Not all units — whitelist by unit type.** Start with **Zealot only**. Firebats with leap are already very strong and don't need it; Ultralisks may not need it either. Widen the whitelist later only if a specific unit shows the same problem.

## Technical breakdown
- **Trigger, mirroring `onBlinkUsed()` (WA-015).** Event = unit uses the leap ability (complete stage) → `onLeapUsed()` → `if UnitGetType(u)` is in the whitelist, apply a short-lived `PostLeap` behavior via `UnitBehaviorAdd`. Trigger-based selection is what makes the per-unit whitelist trivial (data-only would apply to everyone who has leap).
  - Confirm the exact leap ability id in the mod's data first (the raptor/leap ability — verify against `AbilData.xml` / `upgradeInitializers`; not assumed here).
- **The range bump itself is weapon-level, and behaviors modify the unit, not the weapon cleanly** — so a plain `CBehaviorBuff` "+1 weapon range" likely won't work. Expected pattern: a **validator-gated weapon variant**. Give the unit a second "post-leap" weapon (same as normal but +range) whose enable is gated on the `PostLeap` behavior being present; normal weapon gated on its absence. Behavior lasts ~0.5s, then reverts. (This mechanism also happens to support windup-reduction if we ever change our minds — the design lever stays independent of the implementation.)
  - **Verify against catalogs before committing:** whether weapon range is truly un-buffable via behavior Modification, and that dual-weapon validator gating behaves (no double-fire, clean revert).
- Range-indicator desync (the CLAUDE.md behavior caveat) is a **non-issue** here — it's a sub-second transient combat buff, nobody needs a range ring for it.

## ⚠️ Findings (2026-08-25) — difficulty inverted; re-scope before doing
Investigated the actual data. Two corrections that flip the plan:

1. **The range buff is trivial, NOT the hard part.** The dual-weapon / validator-gated plan above is **unnecessary** — a `CBehaviorBuff` *can* cleanly modify weapon range. Proof: the shipped `CBehaviorBuff id="Range"` uses `<Modification WeaponRange="2.5"/>`. So `PostLeap` = a `~0.5s` `<Duration>` `CBehaviorBuff` with `<Modification WeaponRange="1"/>`. Reverts on expiry. Done.

2. **"Leap" is a PASSIVE behavior, not a cast ability — so the `onBlinkUsed` mirror doesn't apply.** Leap = the **`ReaperJump`** behavior (HotS Raptor strain), granted via `addAbilityToUpgrade("HotSRaptorCharge2", ...)` + `addBehaviorToUpgrade("HotSRaptorCharge2", "ReaperJump")` (upgradeInitializers ~167). The unit auto-pounces onto its attack target; the player never casts anything. **There is no `Abil.*.SourceCastStart`-style event to hook `onLeapUsed()` to.** This is now the whole problem.

### Revised approach options (spike needed)
- **A — find a hook:** dig into `ReaperJump` (behavior + its mover / any sub-ability / model events) for *any* event exposable to an editor trigger on each leap. Uncertain one exists at the trigger layer.
- **B — data-only, always-on:** bake `WeaponRange +1` into the leaper's attack/behavior permanently. Simple, but (a) not "brief after leap" and (b) applies to **every** unit that rolls leap (leap is a shared upgrade → shared `ReaperJump`), losing the Zealot-only whitelist unless a per-unit gate is added.
- **C — reconsider the mechanic:** e.g., small always-on attack-range/leash bump specifically for Zealots via a Zealot-only path, accepting it's not leap-gated.

**Deferred past Season 1 launch** — low-priority QoL, and it needs a real spike on leap-detection, not a quick edit.

## Acceptance criteria
- [ ] A leap Zealot that leaps onto a moving target lands at least one hit against a target that would previously kite it (the ring-around-the-Barracks scenario).
- [ ] The bonus range is brief (~0.5s) and reverts cleanly — no permanent range gain, no stuck buff, no double-firing weapon.
- [ ] Applies to **Zealot only** (whitelist); Firebat/Ultralisk with leap are unaffected.
- [ ] Verify on a **published** build if any part is actor/validator-driven (upgrade/weapon visuals have bitten us before).

## Notes
Not a priority — a "sure was annoying" quality-of-play fix from real play. Same trigger shape as WA-015 (`onBlinkUsed`), so the mechanism is familiar. Decision locked: **brief +range, not windup.**
