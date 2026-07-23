---
id: WA-064
status: todo
size: S
phase: 1-game-readiness
priority: 25
---
# Neural Parasite tube lingers after the effect ends (visual only)

## Symptom
When a unit that **rolled** Neural Parasite (a Viper, in testing) casts it, the little tube/beam that attaches caster→target **does not disappear** when the parasite wears off. Purely cosmetic — no gameplay impact — but it looks 10/10 weird (dangling tube with nothing happening).

## Likely cause (investigated, shallow)
- The tube is the stock actor **`NeuralParasiteEffect`** (`CActorModel`, parent `ModelAddition`) — the mod has **no** override for it, so it's using stock creation/destruction events.
- The rolled ability is **`F_NeuralParasite`** (`addAbilityToUpgrade("NeuralParasite", "F_NeuralParasite")`), not the Infestor's native `NeuralParasite`. Stock's tube-destroy event almost certainly keys on the native ability / behavior ending; the `F_` version (and/or a non-Infestor caster like the Viper) likely doesn't fire that exact term, so the destroy never triggers.

## Next steps
1. Read `NeuralParasiteEffect`'s actor events (`CActorModel` `On` terms) in the reference (`swarm.sc2mod` actordata ~line 5220) — find what **Create** and **Destroy** key on (behavior on/off, ability start/stop, or the persistent/channel effect ending).
2. Check what `F_NeuralParasite` actually applies (its effect/behavior chain in `AbilData`/`EffectData`) vs the stock `NeuralParasite`. The mismatch between the two is the destroy term that never fires.
3. Fix options (cheapest first):
   - If the destroy keys on a behavior, make sure `F_NeuralParasite` applies/removes that same behavior.
   - Otherwise add a mod `CActorModel` override for `NeuralParasiteEffect` with a Destroy `On` term that matches the `F_` version's behavior-off / channel-end.
4. Test on the Viper (rolled) specifically, since that's where it reproduces.

## Acceptance criteria
- [ ] The neural tube disappears when the parasite effect ends, on rolled `F_NeuralParasite` (Viper + any other roller).
- [ ] Native Infestor Neural Parasite (if present) still cleans up normally.

## Notes
Low priority — cosmetic, no gameplay effect. Related: the `F_`-rolled-ability class generally (F_Blink et al.). Effect/actor ids: `F_NeuralParasite`, `NeuralParasiteEffect` (the tube), `NeuralParasiteRange`.
