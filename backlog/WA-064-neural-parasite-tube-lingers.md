---
id: WA-064
status: todo
size: S
phase: 1-game-readiness
priority: 25
---
# Neural Parasite tube lingers after the effect ends (visual only)

## 🔁 REOPENED (2026-08-25) — still reproduces on a Ghost
Taylor rolled Neural Parasite on a **Ghost** and saw the tube linger again. The PR #35 fix keys the tube-destroy on `Abil.F_NeuralParasite.SourceFinishStop`, which worked for the tested Viper case but leaks here. Two leading hypotheses:
1. **End-condition gap (most likely):** `SourceFinishStop` fires when the *channel* stops normally. If the parasite ended another way — target died, caster died/moved, the effect hit its duration cap, or control was interrupted — that term may not fire, so the destroy never triggers. **Fix direction: key the tube-destroy on the behavior turning OFF (`Behavior.<x>.Off`) instead of / in addition to `SourceFinishStop`** — behavior-off fires regardless of how control ends, covering every case in one term.
2. **Second actor:** there may be more than one tube/tentacle/beam actor; PR #35 only overrode `NeuralParasiteTentacle`.

### Next steps (this reopen)
- Repro on a Ghost and note **how** the parasite ended (target died? duration expired? manual stop?).
- Identify the behavior `F_NeuralParasite` applies to the caster/target and add a `Behavior.*.Off → Destroy` term to `NeuralParasiteTentacle` (and any sibling beam actor).
- Re-verify on Viper (original), Ghost, and at least one death-of-target case.

## ✅ RESOLVED (PR #35, merged) — PARTIAL, see reopen above
Fixed. Correction to the guess below: the tube is **`NeuralParasiteTentacle`** (model `Infestor_Ex3_Tentacle`), confirmed in-editor — NOT `NeuralParasiteEffect` (that's the Infestor's head-glow, and voidmulti never applies that behavior, so the first attempt's `Duration` was inert dead code). `NeuralParasiteTentacle` has **no lifecycle terms of its own**; LotV gates its cleanup to a **burrowed Infestor casting the stock `NeuralParasite`** (`Abil.NeuralParasite.SourceFinishStop → Destroy`, on a host actor). A Viper casting the mod's copy `F_NeuralParasite`, non-burrowed, matches neither condition → the tentacle is created but never told to die → lingers.

**Fix:** a mod `ActorData` override adds `<On Terms="Abil.F_NeuralParasite.SourceFinishStop" Send="Destroy"/>` to `NeuralParasiteTentacle` — the F_ mirror of the Infestor's cleanup. Confirmed in-game: the tube clears when the channel ends, and a second cast recreates + cleans up correctly.

### Edge case noted (not investigated — super rare, low priority)
Neural Parasite'd an enemy SCV, then used the SCV to build a structure; on the structure's **completion the neural parasite control ended**. Unclear whether this generalizes to other channeled abilities or ever happens in a standard ladder game (parasiting a worker and building with it basically never comes up). Not caused by this fix (visual-only) — a separate control-mechanics quirk. Parked here in case it recurs.

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
