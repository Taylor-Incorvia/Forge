---
id: WA-096
status: todo
size: L
phase: 2-post-launch
priority: 45
---
# Blink Stalker upgrade-pool redesign — make the whole pool blink-centric

**LIVING DESIGN DOC — brainstorm ongoing, pool not finalized.** The Stalker's identity is Blink; its upgrades should all be *about blink*, not generic stat-stacking. Replace the generic junk with blink-flavored upgrades that change how blinking plays.

## Governing lens: LEGIBILITY (added 2026-09-05)
The real reason the current Stalker upgrades feel bad isn't power — it's that the **opponent can't tell what the upgrade does** ("why did I lose that fight?"). Rolls are hidden, so an invisible effect feels *unfair*. See [[balance-upgrade-legibility]]. Every candidate below is now judged on: **does it paint itself on screen?** Favor self-advertising effects; cut silent ones.

## Interim decision (2026-09-05): consider REMOVING the Stalker for now
It's outperforming its Factory-slot-1 pool-mates (Vulture > Hellion, and Stalker > both) AND it's illegible-unfun with everything but blink cooldown. Cleanest move: **comment it out of the Factory slot-1 pool** (one reversible line), rebuild it properly with the legible blink pool below, then re-add. Pulls the bad experience out of games now instead of shipping a half-fix. (Also cleans up the slot-1 overperformer.)

## Current pool (what a Blink Stalker can roll today) + verdict
- **RANGE** (generic +2.5 weapon range) — CUT. Outranges a bunker far too early; oppressive.
- **Stimpack** — CUT. Self-limiting (4 stims) but off-identity; doesn't belong.
- **Corrosive Bile** — CUT. Underpriced on it, and blink→bile is a sleeper-oppressive combo.
- **Stalker Blink Cooldown** — KEEP (flat reduction; Taylor likes it).
- **Stalker Blink Range** — FIX (see below). Good upgrade ruined by a free sight bonus.

## Key enabler
`onBlinkUsed()` is already wired (WA-015). Any "when you blink, do X" upgrade rides on it — apply a behavior/effect in onBlinkUsed, gated by which upgrade the stalker rolled (the stalker is at its *new* position when it fires, so "on-arrival" effects are trivial). Most ideas below are cheap because of this.

## Plan
### Surgical wins (cheap, high-value — do first)
1. **Cut generics from the Stalker roll pool** — exclude RANGE, Stimpack, Corrosive Bile via `logicType_NoneOf` tags (same pattern as the Speed exclusion, WA-095).
2. **Fix Blink Range: remove/reduce the SIGHT bonus.** The teleport-range increase is fine; the free sight is what makes it "blink in with zero warning." Cut the sight modification so using full blink range requires a spotter near the target — the opponent sees something coming. Catalog tweak to the existing `stalkerblinkrange` upgrade.

### New blink-centric upgrades (build candidates, via onBlinkUsed)
3. **Shields after blink** (~40 shields over ~4s post-blink; the Patches-mod one). Taylor likes it. Straightforward behavior on blink. **LEGIBLE** (visible shield-regen effect) — keep.
4. **Blink → SHOCKWAVE at destination** (LEAD CANDIDATE — Taylor idea, 2026-09-05). On arrival, a big obvious shockwave hits nearby enemies. Fun AND self-advertising — the shockwave IS the tell, so "why did that hurt / why were they knocked" answers itself. **LEGIBLE.** Flavors, in order of preference:
   - *Displacement (knockback / brief ~0.5s root)* wrapped in the shockwave — most on-theme (Lee Sin ward-hop→R), skill-expressive, a commitment window not a leash. Harder to build.
   - *Damage* wrapped in the shockwave — simpler; keep the number SMALL or it becomes blink-in/out attrition.
   - *Slow* — AVOID: this is the Concussive concern (slow on a hyper-mobile unit = can't-escape-either-way; likely oppressive).
5. **Bonus damage on the first attack after blink** — DEMOTED: fails the legibility test ("why am I taking extra damage?" — invisible). Cut it, OR only keep it wrapped in an unmistakable visual — at which point it collapses into #4 (the shockwave). Don't ship as a silent damage buff.

## Balance watch
- **On-blink effects combo with blink-cooldown-reduction into blink-in-hit-blink-out spam.** Healthy versions reward *committing* to a fight, not hit-and-run. Consider not letting cooldown-reduction + an on-blink-damage/effect both be trivially rollable together, or design the effect to reward staying.
- Attribute-bonus reminder ([[balance-attribute-bonuses-overperform]]): the Stalker already over-performs via its +Armored bonus in this mechanical-heavy pool, so keep new upgrades about *utility/positioning*, not raw damage.

## Rejected / parked
- **Concussive Shells on Stalker** — keep OFF. Blink + slow-on-hit = perma-kite that slows every chaser (marines slowed to death). Oppressive.
- **Conditional cooldown** (attack-reduces-cooldown) — NO. Per-unit cooldown desync makes group-blinking miserable (half ready, half not).
- **2 blink charges** — redundant with cooldown reduction.
- **Cloak-on-blink** (Patches tested + removed) — SKIP regardless of their reason: it's worse in Wildcard (Scan is nerfed/expensive here, so brief cloaks are far harder to punish) and pushes the Stalker back toward "uncatchable," which we just fought by excluding Speed (WA-095).
- **DoT-at-blink-origin + return-to-origin** — evocative but heavy (persistent origin marker + return-blink + per-unit origin tracking). Someday.

## Notes
Related: [[WA-095]] (Speed exclusion, same tag pattern), [[balance-for-forced-creativity]]. Don't build until the pool is locked — brainstorm still open.
