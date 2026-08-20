---
id: WA-083
status: todo
size: S
phase: 1-game-readiness
priority: 60
---
# Seeker Missile (mass Raven) — gauge player feedback: clarity vs balance

## What
Observation, not a confirmed problem. First time Seeker Missile was seen used in anger (Taylor vs Goog, 2026-08-19). Mass Raven + Seeker Missile *felt* oppressive in-game — but replay review points to a **clarity/unfamiliarity** issue more than a balance one. **No change made. Gathering player feedback before acting.**

## The ability (F_SeekerMissile = extracted WoL Hunter Seeker Missile)
- **100 damage AoE** with falloff: 100 @ 0.6 radius, 50 @ 1.2, 25 @ 2.4. Hits everything except structures/missiles. Primary target takes the full hit; clustered neighbors take the falloff.
- **100 energy per cast** (Raven caps 200 → holds 2). Purely **energy-gated** — the Charge/Cooldown link (`Abil/HunterF_SeekerMissile`) dangles, same as the base liberty ability, so there's no real cooldown. The **+25% caster regen** buff feeds cast frequency.
- **Homing missile, ~5s lifetime → dodgeable.** Move the marked unit away and it fizzles. Fast units (Blink Stalkers) escape easily; slow/engaged units (Immortal) can't.
- **Telegraph works** — the red target mark DOES render (confirmed via replay). *Not* a bug; earlier "broken telegraph" theory was wrong (the dangling marker link is tolerated by the engine, just like base).

## Why it "murdered"
Clumped air (Corsairs stack when repeatedly right-clicked to one spot) ate the splash — ~10 then ~7 Corsairs deleted in single volleys. Learnable: spread air, run the marked unit, don't a-move into a Raven ball.

## Decision criterion (the signal to watch)
- Players **adapt** after a few exposures (spread / dodge / focus Ravens) → healthy depth, **keep as-is**.
- Experienced players **keep getting deleted and still can't tell why** → genuinely unclear → act.

## Action spectrum (only if feedback says "unclear/unfair", in order)
1. Louder telegraph / impact sound.
2. One-line tooltip that teaches the dodge ("move the marked unit to escape").
3. Throttle cast frequency via **energy cost** and/or reassess the **+25% caster regen** — the real lever for "too many nukes."
4. Nerf the 100 damage / tighten radii, or 1-per-faction cap.
5. Replace with Snipe — LAST resort (Snipe is biological-only, an awkward fit for Wildcard's mech/armored rolls).

**Do NOT revert the Raven mineral cost (100→75).** Taylor won vs Raven+Fungal at the same cost — the *upgrade* is the lever, not the unit, and cheaper Ravens getting used is a win. Goog (early adopter, Terran main who thinks Ravens are weak in retail) is championing them — mass Raven is a healthy emerging strat.

## Notes
Unfamiliar campaign abilities are part of the mod's texture — "players don't know what an HSM is" is a thing to learn, not inherently a defect. Revisit after several more games of feedback.
