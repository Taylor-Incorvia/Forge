# Caster energy-cost audit — `F_` abilities vs stock counterparts

Generated 2026-07-24. Compares the mod's rolled caster spells (`F_` prefix, granted by an upgrade) against the **default energy cost of the same ability in the base game**. Stock values pulled from `reference/` catalogs (retail multiplayer where the ability exists in retail; campaign/co-op otherwise).

**Design intent on record:** when a unit researches a caster ability, the `F_` version was meant to be *slightly more energy-efficient than its non-`F_` counterpart* — so the reward for the upgrade is "this spell, but cheaper." This audit checks how consistently that held, and gathers the data for the proposed **switch to an energy-regen buff** instead of a per-cast discount.

---

## Energy-based caster spells (the ones this philosophy applies to)

| `F_` spell (unit) | `F_` energy | Stock energy | Δ vs stock | Source of stock value |
|---|---|---|---|---|
| **CorsairMPDisruptionWeb** (Corsair) | 50 | 100 | **−50** | co-op/campaign |
| **MissilePods** (Wraith) | 75 | 125 | **−50** | campaign |
| **FungalGrowth** (Infestor) | 50 | 75 | −25 | retail MP |
| **GuardianShield** (Sentry) | 50 | 75 | −25 | retail MP |
| **NeuralParasite** (Infestor) | 75 | 100 | −25 | HotS MP |
| **ParasiticBomb** (Viper) | 100 | 125 | −25 | retail MP |
| **SeekerMissile** (Raven) | 100 | 125 | −25 | campaign |
| **Yoink / Abduct** (Viper) | 50 | 75 | −25 | HotS MP |
| **ForceField** (Sentry) | 40 | 50 | −10 | retail MP |
| **GravitonBeam** (Phoenix) | 40 | 50 | −10 | retail MP |
| **Transfusion** (Queen) | 40 | 50 | −10 | retail MP |
| **BuildAutoTurret** (Raven) | 40 | 50 | −10 | retail MP |
| **BlindingCloud** (Viper) | 100 | 100 | 0 | retail MP |
| **DefensiveMatrix** (Raven) | 100 | 100 | 0 | campaign |
| **OdinBarrage** (?) | 75 | 75 | 0 | campaign |
| **RavenScramblerMissile** (Raven) | 75 | 75 | 0 | Raven stock |
| ⚠️ **Irradiate** (Science Vessel) | 40 | 25 | **+15** | campaign |
| ⚠️ **ArbiterMPStasisField / Stasis** | 125 | 100 | **+25** | co-op/campaign |
| ⚠️ **ArbiterMPRecall / Recall** | 125 | 100 | **+25** | co-op/campaign |

### Anomalies (the ⚠️ rows — F_ is *more* expensive, contradicting the stated philosophy)
- **Irradiate +15** — this one is **intentional**. Stock 25 was "insane," you bumped `F_Irradiate` to 40 deliberately. Working as designed; the philosophy just doesn't apply here.
- **Stasis +25** and **Recall +25** — these are the two to eyeball. Both are *powerful, game-swinging* utility spells (mass freeze / army teleport), so pricing them *above* stock may be deliberate caution — but it's the opposite of the "reward = cheaper" rule, so worth a conscious decision rather than an accident.

### Parity rows (0)
BlindingCloud, DefensiveMatrix, OdinBarrage, RavenScramblerMissile sit exactly at stock — no discount reward. Not wrong, just not following the "slightly cheaper" pattern.

---

## Non-energy `F_` abilities (excluded — not gated by energy)
These are cooldown- or life-gated, so the energy-efficiency philosophy doesn't touch them:

`F_Blink`, `F_Charge`, `F_D8Charge`, `F_KD8Charge`, `F_Hyperjump`, `F_Grapple`, `F_LightningBomb`, `F_PsionicShockwave`, `F_RavagerCorrosiveBile`, `F_Yamato` (100s cooldown), `F_Stimpack` (costs life), `F_TempestDisruptionBlast` (cooldown).

---

## The proposed alternative: energy-regen buff instead of a per-cast discount

The idea: on upgrade completion, raise the unit's **energy regen rate** so you get *more spells of any type over time*, rather than one hyper-efficient spell — to avoid "show up to one fight and drop 27 auto-turrets."

### The key mechanical insight (this is why the idea is good)
- **Max energy is capped at 200 and a fight lasts seconds.** Regen during a single engagement is tiny (~0.79/s even with a healthy boost → a handful of energy over a 20s fight).
- Therefore: **a regen buff improves *sustain* (casts across the whole game) but does *not* raise the single-fight *burst* ceiling** — the burst is capped by the 200 pool, which regen barely moves mid-fight.
- A **per-cast discount does the opposite** — it raises *both* burst (more casts per full 200 pool) *and* sustain.

So switching discount → regen **surgically removes the burst amplification** while keeping (even improving) sustain. That is exactly your stated goal.

### Two honest caveats
1. **It doesn't fix hard burst-spam on cheap abilities by itself.** "27 auto-turrets" comes from *cheap spell (40) + big banked pool (200)* = 5 turrets per pool — the −10 discount only adds *one* turret. Removing the discount claws back that one; it doesn't cap the other five. If auto-turret burst is genuinely oppressive, the real lever is **cost-up, a charge limit, or a cooldown** on the turret — not regen. Regen vs discount is the right call for the *general* case; auto-turret may still want its own cap.
2. **"Enough energy to actually cast the new spell" needs a second mechanic for expensive spells.** A discount lowers the threshold so a fresh caster can fire sooner; pure regen just fills the same 100-cost bar faster. To guarantee the just-upgraded unit can use its new toy, pair the regen buff with a **one-time energy grant on upgrade** (or a small `EnergyStart` bump) — then keep the *per-cast cost at stock*. That combo hits all three of your goals: can cast it right away, sustains more over the game, and no burst discount.

### Fit with existing design principles
- **Consistency win:** a uniform "+X regen per caster upgrade" erases the current grab-bag (−50 to +25, some at parity, Stasis/Recall inverted). Cleaner mental model.
- **Visibility:** a regen buff is invisible on its own — but the *new spell you didn't have before* is the visible, decision-driving part of the upgrade, so the roll still satisfies "noticeable upgrades over invisible ones." The regen is just plumbing.
- **Implementation:** grant a `CBehaviorBuff` with an energy-regen `Modification` on upgrade (behaviors are fine here — no catalog-sourced visual to desync, unlike range/`CActorRange`). The mod already grants behaviors via upgrades (`addBehaviorToUpgrade`), so this is a trodden path.

### Recommendation
The regen switch is **sound for the stated goal** and worth prototyping — but implement it as the **three-part combo**, not regen alone:
1. `F_` per-cast cost → **stock** (drop the discounts; removes the burst amplifier).
2. **+energy regen** behavior on upgrade (sustain).
3. **One-time energy grant** (or `EnergyStart` bump) on upgrade so the new spell is immediately usable.

Then treat genuinely burst-abusable cheap spells (auto-turret the prime suspect) as *separate* cases needing a charge/cooldown/cost cap — regen won't police them.
