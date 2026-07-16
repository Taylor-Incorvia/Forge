---
id: WA-035
status: in-progress
size: M
phase: 1-game-readiness
priority: 41
---
# Lifesteal — per-unit upgrades on non-shielded units

## 🔨 Slice 1 done 2026-07-16 (PR) — Marine, 15%
Proved the mechanism on the Marine. `CUpgrade LifestealMarine` adds `LeechFraction[Life] = 0.15` to the Marine's damage effect (`Effect,GuassRifle`) — confirmed the bracket-index EffectArray syntax against working examples (`AttributeBonus[Structure]` etc.). Count-upgrade wiring: `addUpgradeToUpgrade` + `AnyOf Marine`; `CAbilResearch LifestealMarine2` (Marine = Barracks slot 2 → Ghost Academy col 1); `CButton LifestealMarine2`; GameStrings. Fraction = **15%**, Life only. No XML comments.

**⚠️ Needs an in-game test:** there is **no reference example of a `CUpgrade` modifying `LeechFraction`**, so while the bracket syntax is confirmed for other indexed fields, verify in-game that a researched Marine actually heals on attack. If it doesn't apply, the likely fix is the field path (`LeechFraction[Life]` vs a different index form).

**Deferred to slice 2+:** (a) the green heal-indicator actor (AC #2) — pure polish, gated on the upgrade; (b) replicate to the rest of the non-shielded basic-attacker list (needs your unit pick — see Open decisions). Multi-weapon units modify all weapons.

## Why
Pool diversity for basic attackers (sibling of WA-034). Lifesteal (attacks heal the attacker for ~15–20% of damage dealt) is a flavorful, build-defining option.

## 🔍 Findings

### Native field, but per-weapon-effect; no behavior route (confirmed)
`CEffectDamage` has `<LeechFraction index="Life" value="0.15"/>` — the engine returns that fraction of damage dealt to the attacker's vital, no trigger code. But it lives on the **damage effect**, and **no behavior can add it** (searched every reference `behaviordata.xml`). So: **per-unit count upgrades that modify the unit's weapon damage effect(s)** — same structure as WA-034.

### Scope decision: non-shielded units only
Restricting to **non-shielded (Terran / Zerg)** units sidesteps the shields-vs-life question entirely — `index="Life"` is always correct, nothing is wasted on shields. Non-shielded **attackers** currently in the pools:
- **Barracks:** Zergling, Hydralisk, Marine, Firebat, Marauder, Ghost, Queen
- **Factory:** Vulture, Hellion, Diamondback, SiegeTank, WarHound, LurkerMP, Goliath, ThorAP, Ultralisk
- **Starport:** Wraith, VikingFighter, Liberator, Mutalisk, DuskWing, Battlecruiser
- _(Excluded: all Protoss = shields; support/casters Medic, Infestor, Viper, Raven.)_

Pick the **"basic attacker" subset** — likely skip the already-strong heavies (Thor / Battlecruiser / Ultralisk) and multi-weapon complexity unless you want them.

### Per-unit recipe
For unit U: `CUpgrade LifestealU` modifies U's weapon damage effect(s) `LeechFraction[Life]` from 0 → 0.15 (verify exact upgrade reference syntax against a working example). **Multi-weapon units → modify all weapons.** Then `addUpgradeToUpgrade("LifestealU","LifestealU")` + `addUpgradeRequirementTag(... AnyOf unitTag U)` + research UI for U's slot.

### Icon / clarity (data actor; independent of WA-030)
Green heal-model `CActorModel` keyed to the weapon damage impact on the attacker — pure data. **Nuance:** because leech rides on the base damage effect, that actor would also fire *before* research unless gated — so gate the actor with a validator on the upgrade (or key it on a small companion marker the upgrade adds). Solve during impl. Does **not** depend on [[WA-030]] (that's TextTag for ability casts; this is a heal model).

## Suggested sequencing
Prove on ONE single-weapon unit (e.g. Marine) — validate heal + icon in-game — then mechanically replicate to the chosen list.

## Open decisions (grooming)
- Fraction: **15%** or 20%? (suggest 15%, tune)
- Exact unit list (the basic-attacker subset of the non-shielded list above).

## Acceptance criteria
- [ ] Each chosen non-shielded unit can roll Lifesteal; after research, attacks heal it for the % (Life), across **all** its weapons.
- [ ] A heal indicator appears above the unit when it lifesteals.
- [ ] A unit's version is rollable only by that unit.

## Notes
Native mechanism = low engine risk; the cost is per-unit wiring volume + the actor gating. Sibling: [[WA-034]] — identical per-unit/multi-weapon structure; behavior route ruled out for both.
