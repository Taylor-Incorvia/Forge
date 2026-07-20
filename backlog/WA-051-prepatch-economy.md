---
id: WA-051
status: in-progress
size: S
phase: 1-game-readiness
priority: 10
---
# Revert to the pre-patch economy for Season 1 (12 workers, 400 CC, old CC supply)

## 🔨 Slice 1 done 2026-07-20 (PR) — CC cost 400
Change #1 shipped: `<CUnit id="CommandCenter"><CostResource index="Minerals" value="400"/></CUnit>` in the mod's `UnitData.xml` (mirrors the Archon cost override). The live patch drops the CC to 300, so this forces it back to 400. **The other two are deferred** — first prod push tests on **8 workers** (Taylor's call), and the CC-supply revert (#2) still needs the confirmed pre-patch value. Reopen #2/#3 when ready.

## Decision (2026-07-16)
Freeze Wildcard Arena on the **pre-patch economy** — **12 worker start, 400-mineral Command Center, and the pre-patch CC supply** — at least until the ladder economy stabilizes into something coherent. Blizzard is shipping large economy changes weekly; anchoring Season 1 to a moving target is a trap. Reconsider adopting a new value only if/when it settles.

## Why
- **All existing balance testing was done on 12/400** (hydra cost, queen supply, the barracks slot-2 cost-efficiency findings, proxy testing). Adopting 8/300 silently re-times when players afford their rolled units and invalidates that work.
- **The audience prefers it** (~60% want the 12-worker start; the 300 CC is near-universally disliked). Reverting isn't drifting from ladder-classic — it *preserves* it. Framing: "the economy you wish ladder had kept."
- **Wildcard favors the greedy/defender more than ladder does** — you can't reliably all-in with a randomized army, so a cheaper 300 CC tilts even further toward degenerate greed. More reason than ladder to keep the 400.
- The patch's **CC supply bump** is what lets you go ~3 CCs before a second depot while still making workers — silly; revert it.

## Changes (the mod tracks live VoidMulti `0.0/999`, so each needs an explicit mod override)
1. **CC cost 300 → 400** — add a `<CUnit id="CommandCenter">` override in the mod's `UnitData.xml`: `<CostResource index="Minerals" value="400"/>`.
2. **CC supply → pre-patch value** — same `CommandCenter` override, set the supply-provided (`<Food value="N"/>`, positive = provides) back to the pre-patch number. **Confirm exact old value** (Taylor knows it — reference is ambiguous / possibly post-patch). Revert the increase that enables the 3-CC opening.
3. **Worker start 8 → 12** — NOT a `UnitData` field; the starting worker count comes from melee-initialization / starting-units setup. Two options:
   - (a) Investigate whether the mod can override the melee starting-units data directly.
   - (b) **Fallback (reliable):** a game-start galaxy function that spawns **+4 SCVs** at each player's main (Ember writes the function, Taylor wires it to game start — same pattern as the other triggers). Gets from 8 → 12.

## Acceptance criteria
- [ ] Games start with 12 workers.
- [ ] Command Center costs 400 minerals.
- [ ] CC supply is back to the pre-patch value (no 3-CC-before-2nd-depot opening).
- [ ] Confirm nothing else in the mod's economy quietly shifted with the patch (e.g. worker cost/build time, starting supply cap).

## Notes
Fully reversible — these are 2 data values + a small starting-units lever, so flipping back to a future ladder standard is a five-minute change. Do this as its own small PR; unrelated to the upgrade-cap work (WA-049/WA-050). Decision rationale from the 2026-07-16 discussion.
