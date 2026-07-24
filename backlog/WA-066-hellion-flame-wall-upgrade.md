---
id: WA-066
status: todo
size: M
phase: 1-game-readiness
priority: 35
---
# Hellion "flame wall" upgrade — combine blue flame + a big, visually-matched splash

## Why
The Hellion had two flame upgrades: **`HighCapacityBarrels`** (SC2's internal id for **Infernal Pre-Igniter** — +vs-Light + blue flame; good) and **`TwinLinkedFlameThrowers`** (a WoL *campaign* upgrade that only widened the flame splash 0.15→0.3; a near-invisible dud that felt terrible and diluted the odds of rolling blue flame). `TwinLinkedFlameThrowers` has been **pulled from the pool** (`upgradeInitializers.galaxy`, commented with this ticket). This ticket is the fun replacement: one **"flame wall"** upgrade that's a spectacle — blue flame **+** a genuinely wide, visually-matched splash.

## Design intent
One Hellion flame upgrade that reads as a clear, exciting power spike:
- **Blue flame + bonus vs Light** (the existing `HighCapacityBarrels` effect — real, free visual signal).
- **A much wider flame splash** — a "wall of fire," not the current pinprick.
- **The flame model expanded to match the splash** (this is the crux — see below).

## Technical breakdown (from scoping)
Two very different difficulty tiers:

1. **Splash *damage* width — trivial (data).** `TwinLinkedFlameThrowers` already did `Operation="Set"` on `Effect,InfernalFlameThrowerE,AreaArray[0].Radius` (0.15→0.3). Override that `Set` to whatever ("wall" territory, e.g. 1.0–2.0), on the combined upgrade. Do the same for the Hellbat form (`HellionTankSearch` radius) if it should apply there too. ~5 minutes.
2. **Flame *visual* expansion — the real work (actor).** A wide splash with a normal-looking flame is a **readability bug** (units die outside the visible flame). Two ways to make the flame *look* as wide as it hits:
   - **(a) Scale the existing actor** — `SetScale` (or model-scale) on the `InfernalFlameThrower` flame actor via an `UpgradeComplete` event. Least moving parts; keeps the flame's line/orientation behavior.
   - **(b) Swap to a bigger flame model from another unit** — e.g. the WoL campaign **Odin**'s flame actor (user idea). More spectacular out of the box, but a model-swap is probably *harder* than a scale: you'd need to find the Odin flame actor id, and a big AoE flame may not tile/orient like the Hellion's forward *line* flame. Try (a) first; reach for (b) only if scaling looks bad.

   Either way it's fiddly, iterative, and **likely publish-to-verify** (upgrade-triggered visuals have bitten us before — cf. the Phoenix beam recolor). Scope the exact flame actor before committing to a width.

**Rule of thumb from scoping:** a *moderate* widening (~0.4) needs no visual work (mismatch is tolerable); a *"flame wall"* width **requires** the visual scaling. This ticket wants the wall, so budget for the actor part.

## Approach
- Bake everything into one CUpgrade (extend `HighCapacityBarrels`, or a new `HellionFlameWall`): the existing +vs-Light/blue effects **plus** the wide-splash `Set`.
- Add the actor scaling for the flame model (the hard part) so the visual matches.
- Keep it a single Hellion flame roll (pool stays [5] with `TwinLinkedFlameThrowers` gone).

## Acceptance criteria
- [ ] Rolling the Hellion flame upgrade gives blue flame + bonus vs Light + a clearly wider splash.
- [ ] The flame **visual** is expanded to roughly match the splash radius (no "invisible death" outside the flame).
- [ ] Verified on a **published** build (the visual half won't render in the local Test Document).

## Notes
Replaces the removed `TwinLinkedFlameThrowers`. Splash-radius reference: `Effect,InfernalFlameThrowerE,AreaArray[0].Radius` (mod base 0.15, currently points at `InfernalFlameThrowerConcussiveSet`). Blue-flame reference: stock `HighCapacityBarrels` (swarm: +5 vs Light on `InfernalFlameThrower`/`HellionTankDamage` + junker icon). Sibling to other actor-visual-on-upgrade work.
