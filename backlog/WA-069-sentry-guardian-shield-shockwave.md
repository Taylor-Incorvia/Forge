---
id: WA-069
status: todo
size: M
phase: 1-game-readiness
priority: 13
---
# Sentry upgrade — Guardian Shield casts a mini shockwave

## Why
Grows the small Sentry pool with a spectacle upgrade: casting **Guardian Shield** also fires a **mini shockwave** (damage + knockback) around the Sentry. Turns a purely defensive button into a defense-*and*-punch power moment — and it's very visible.

## Design intent
- On Guardian Shield cast, emit a shockwave centered on the Sentry, **radius ≈ the shield bubble**, **enemy-only**, with **reduced damage** (the source effect is a WoL-campaign **boss** ability — stock damage is way too high).
- **Energy-gated by Guardian Shield itself** (50/cast) — this is the key difference from the removed standalone `PsionicShockwave`, which was free and felt un-approachable en masse (a wall of 20 Tempests knocking back anything that stepped in). Tying it to Guardian Shield's energy + cooldown tames it.

## The "hits allies" problem is smaller than it looked
Standalone `PsionicShockwave` was pulled because it knocked back **your own** units (see `upgradeInitializers.galaxy`, commented out). That's not a research project — it's a **`SearchFilters`** on the search effect (require Enemy; exclude Self/Ally/Player). Standard SC2. So "enemy-only" is a few lines, whether standalone or bundled. We're **not** re-adding the standalone (still don't want the free mass-knockback wall) — only this energy-gated, filtered, reduced-damage version bolted onto Guardian Shield.

## Technical breakdown
- Add an on-cast effect to `F_GuardianShield`'s effect chain (or grant it via the upgrade): a `CEffectEnumArea` (radius = shield) → damage effect, reusing/adapting the `PsionicShockwave` knockback + damage effects.
- Set `SearchFilters` to enemy-only. Reduce the damage `Amount` hard from the boss value.
- Per-unit upgrade gated `AnyOf Sentry`. Wire like the other upgrades (research entry, button, GameStrings).
- **Oppressive edge to watch:** mass Sentries all shielding at once = a coordinated AoE knockback+damage burst. Energy cost + Sentry fragility should cap it; confirm in play.

## Acceptance criteria
- [ ] With the upgrade, casting Guardian Shield deals a small AoE + knockback to **enemies only** (own units untouched).
- [ ] Damage is tuned down from the campaign boss value; feels like a punch, not a delete.
- [ ] Still costs/gated by Guardian Shield's normal energy; no free spam.
- [ ] Verify on a **published** build (added-ability visuals/behaviors have bitten us before).

## Notes
Low priority — **not before the "Your Faction" modal**. Reincarnates the removed `PsionicShockwave` in a tamed form. Sibling small-Sentry-pool idea: WA-067 (force-field size). Ring/perimeter force field was explicitly **cut** (not doing it).
