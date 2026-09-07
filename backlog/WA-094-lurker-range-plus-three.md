---
id: WA-094
status: done
size: S
phase: 2-post-launch
priority: 45
---
# Lurker range upgrade: +2 → +3 (make it POP more than ladder)

## What
Bump the Lurker range upgrade (stock Seismic Spines / `LurkerRange`) from **+2 to +3** — both the weapon range AND the spike count, so they stay in sync and the impale visibly reaches farther.

- Range: base 8 → **11** (was 8 → 10)
- Spikes: base `PeriodCount` 9 → **12** (was 9 → 11)

## Why
On ladder, Seismic Spines is +2 range / +2 spikes. Taylor wants the Wildcard version to feel more impactful — "I want the Lurker range upgrade to POP more than it does in ladder." Wildcard upgrades are allowed to be a bit stronger than their ladder counterparts (same philosophy as Concussive being cheap/fast), and the Lurker range roll should feel like a real payoff, not a marginal +2.

## Why range alone (a stat/behavior) can't do this
The Lurker attack is a persistent effect that ticks `PeriodCount` times, firing one spike per tick down the line — so reach is governed by `Effect,LurkerMP,PeriodCount`, not just `Weapon,LurkerMP,Range`. That's why the generic Range behavior does nothing on a Lurker (it only touches the weapon Range number). The stock `LurkerRange` CUpgrade correctly modifies BOTH, which is why we ride on it. See [[reference-research-ability-per-slot-shared]] context / catalog-merge notes.

## Implementation (done)
Created a mod override of the stock `LurkerRange` CUpgrade in `UpgradeData.xml`, overriding the two existing EffectArray entries **by explicit index** (index 0 = weapon range, index 1 = PeriodCount) so the values replace rather than append — appending without indices would DUPLICATE the effects (base +2 at 0/1 PLUS new entries = +5). Both set to 3:
```xml
<CUpgrade id="LurkerRange">
    <EffectArray index="0" Reference="Weapon,LurkerMP,Range" Value="3"/>
    <EffectArray index="1" Reference="Effect,LurkerMP,PeriodCount" Value="3"/>
</CUpgrade>
```
Everything else (icon, affected units, research wiring at Factory slot 2 via `LurkerRange2`) is unchanged — inherited from stock + existing mod wiring.

## Verify in Test Document
- Research LurkerRange on a Lurker (Factory slot 2 → Armory), confirm attack range goes to ~11 (Faster) and the spike line is visibly longer (12 spikes) — not doubled/+5 (that would mean the array appended instead of overriding).
