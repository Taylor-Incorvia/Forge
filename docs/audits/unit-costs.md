# Unit Cost Audit (WA-003)

_Generated 2026-07-13. Review artifact — no game data was changed. "Effective" cost = mod `UnitData.xml` override where present, else inherited stock (Liberty/Swarm/Void). Granted upgrades are the free starting upgrades from `grantInitialUpgrades()`._

## Full cost table (by facility/slot)

| Facility | Slot | Unit | Min | Gas | Supply | Override? | Notes (granted upgrade / source) |
|---|---|---|---|---|---|---|---|
| Barracks | 1 | Zergling | 25 | 0 | 0.5 | stock | Stock price assumes 2-per-egg; produced singly here |
| Barracks | 1 | Zealot | 100 | 0 | 2 | stock | |
| Barracks | 2 | Hydralisk | 100 | 50 | 2 | stock | **+EvolveGroovedSpines** (free, +range) |
| Barracks | 2 | Marine | 50 | 25 | 1 | **mod** Gas=25 | **+ShieldWall** (free, +10 HP) |
| Barracks | 2 | Queen | 150 | 50 | 2 | **mod** Gas=50 | |
| Barracks | 3 | Firebat | 100 | 50 | 2 | **mod** Gas=50 | **+BearclawNozzles** (free, +range) |
| Barracks | 3 | Marauder | 100 | 25 | 2 | stock | |
| Barracks | 3 | Sentry | 50 | 100 | 2 | stock | gas-heavy |
| Barracks | 3 | Medic | 75 | 50 | 2 | stock | |
| Barracks | 4 | Ghost | 150 | 150 | 2 | stock | cloak/nuke disabled per comment |
| Barracks | 4 | Infestor | 100 | 150 | 2 | stock | EnergyStart 100 (mod) |
| Barracks | 4 | HighTemplar | 50 | 150 | 2 | stock | **+PsiStormTech** (free Psi Storm) |
| Factory | 1 | Vulture | 100 | 0 | 2 | **mod** Min=100 | stock campaign was 75 |
| Factory | 1 | Hellion | 100 | 0 | 2 | stock | |
| Factory | 1 | Stalker | 125 | 50 | 2 | stock | **+BlinkTech** (free blink) |
| Factory | 2 | Diamondback | 150 | 150 | 4 | stock | supply -4 is heavy |
| Factory | 2 | Immortal | 250 | 100 | 4 | stock | |
| Factory | 2 | SiegeTank | 150 | 125 | 3 | stock | |
| Factory | 2 | WarHound | 175 | 125 | 3 | **mod** | stock swarm was 150/75 |
| Factory | 2 | Archon | 225 | 150 | 4 | **mod** | normally merge-only; recently re-priced (WA-029) |
| Factory | 2 | LurkerMP | 150 | 150 | 3 | stock | |
| Factory | 2 | Goliath | 150 | 50 | 2 | stock | cheapest in slot |
| Factory | 3 | ThorAP | 300 | 200 | 6 | stock | |
| Factory | 3 | Ultralisk | 300 | 200 | 6 | stock | **+ChitinousPlating** (free armor) |
| Factory | 3 | Colossus | 300 | 200 | 6 | stock | **+ExtendedThermalLance** (free, +range) |
| Starport | 1 | CorsairMP | 150 | 100 | 2 | stock | EnergyStart 25 (mod) |
| Starport | 1 | Phoenix | 150 | 100 | 2 | stock | |
| Starport | 1 | Wraith | 150 | 150 | 2 | stock | **+WraithCloak** (free); EnergyStart 25 (mod) |
| Starport | 1 | VikingFighter | 150 | 75 | 2 | stock | voidmulti stock is 125/75 |
| Starport | 2 | Liberator | 150 | 150 | 3 | stock | voidmulti stock is 150/125 |
| Starport | 2 | Mutalisk | 100 | 100 | 2 | stock | cheapest in slot |
| Starport | 2 | DuskWing | 200 | 150 | 3 | **mod** | "buffed banshee"; cloak NOT granted |
| Starport | 2 | VoidRay | 250 | 150 | 3 | stock | |
| Starport | 3 | Raven | 100 | 200 | 2 | stock | EnergyStart 100 (mod) |
| Starport | 3 | Tempest | 300 | 200 | 4 | stock | voidmulti stock is 250/175 |
| Starport | 3 | Viper | 100 | 200 | 3 | stock | EnergyStart 100 (mod) |
| Starport | 3 | Battlecruiser | 400 | 300 | 6 | stock | most expensive unit |

## Flagged for review (your call — nothing changed)

1. **Zergling (Rax 1) — 25/0, 0.5 supply, stock.** Priced around 2-per-egg but produced singly → ~1/4 the cost of slot-mate Zealot. You already cut Marine from slot 1 for "scaling too well + super cheap" — Zergling has the same profile.
2. **HighTemplar (Rax 4) — 50/150, stock + free PsiStormTech.** 50 minerals for a unit with a free game-defining spell. Your own comment flags "might be too much."
3. **Goliath (Factory 2) — 150/50, stock.** Clear low outlier in a slot running 150–250 min / 100–150 gas (Immortal 250/100, Archon 225/150, WarHound 175/125). Looks underpriced for the tier.
4. **Mutalisk (Starport 2) — 100/100, stock.** ~Half the cost of VoidRay (250/150) / DuskWing (200/150). Big intra-slot spread.
5. **Ghost (Rax 4) — 150/150, stock, cloak+nuke disabled.** Paying full price for a stripped-down caster. (Note: WA-024 may re-enable nuke, which would justify the cost.)
6. **Battlecruiser (Starport 3) — 400/300/6, stock.** Big jump over slot-mates Raven/Viper (100/200). May be an intended apex, but worth a deliberate call vs. inherited default.
7. **Marine (Rax 2) — 50/25 + free ShieldWall.** Cheapest buffed unit in slot; you already bumped gas 0→25. Keep watching.

**Lower-priority:** gas-lopsided direct-produced casters (Sentry 50/100, Raven/Viper 100/200); Diamondback's heavy -4 supply in slot 2; and several units inherit Liberty/base costs that differ from current ladder (voidmulti) values — Ultralisk 300 vs 275, VikingFighter 150 vs 125, Liberator gas 150 vs 125, Tempest 300/200 vs 250/175, Raven gas 200 vs 150. Confirm that matches the intended base dependency.
