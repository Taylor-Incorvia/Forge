# Build Times — Wildcard Arena vs. Stock LotV

Snapshot **2026-08-02** · updated **2026-08-03** (added *Suggested build times*; Stalker/Archon dual-facility entries removed, PR #28 merged). Purpose: decide whether the mod's fast build times are *flattening the opening* — specifically the "how many production buildings before you expand/tech" branch that gives standard SC2 its opening variety.

> **Units.** All numbers are **catalog (standard-speed)** `Time` values — the raw XML attribute. The game runs on **"Faster" (≈ ÷1.4)**, so a *real-game* second column is included for intuition. **Ratios are speed-independent** (÷1.4 hits both sides equally), so "Mod % of stock" is the same whether you read catalog or real seconds.
>
> **Sources.** Mod = `AbilData.xml` `CAbilTrain` InfoArrays (BarracksTrain/FactoryTrain/StarportTrain), current as of this date. Stock = LotV multiplayer, resolved across the layered catalogs (voidmulti → void → swarm → liberty → core). Warp-in times noted where relevant.

---

## The headline

Build times are compressed **unevenly**, and the compression is worst exactly where the opening branch lives:

| Tier | Mod vs stock | Examples |
|------|-------------|----------|
| **Cheap early combat units** | **55–70% of stock** (much faster) | Zealot 55%, Stalker 47–66%, Marine 60%, Sentry 64%, Adept 60%, Hellion 70%, Mutalisk 61%, Queen 64% |
| **Mid units** | **~80–93%** | Colossus 90%, Marauder 93%, VoidRay 81%, Tempest 78%, Liberator 82%, Battlecruiser 78%, Hydralisk 79%, Infestor 76% |
| **Heavy / capital tech** | **90–132%** (at or *slower* than stock) | Immortal 132%, Thor 112%, Ultralisk 122%, Colossus 90% |

**The early game is where times are most compressed; the late game is stock-speed or slower.** That is the mechanical fingerprint of "flattened openings." The frontline units you open with pop ~twice as fast as stock, while the tech payoff units are unchanged — so the *early* decisions get squeezed flat while the *late* game doesn't feel faster (which also explains a "builds feel too long" complaint about teching — the two are not contradictory, they're opposite ends of the same curve).

---

## Why fast cheap units flatten the opening (the mechanism)

In standard SC2 the opening branches — 1 rax vs 2 rax vs 3 rax vs CC-first — because **one production building does NOT consume your whole mineral income.** Adding buildings converts more of your bank into army, at the cost of economy/tech. That tension *is* the decision.

If a single building's unit throughput already **saturates** your early income, a second building produces nothing until your income grows — so the decision disappears. Worked example (real-game seconds, ~12 workers ≈ **8 min/sec** income):

| | Build time (real) | Cost | Throughput per building | Share of 8 min/sec income |
|---|---|---|---|---|
| **Mod Zealot** | 15.0s | 100 | **6.7 min/sec** | **~83%** — one building nearly saturates you |
| **Stock Zealot** | 27.1s | 100 | 3.7 min/sec | ~46% — you comfortably run 2 gateways |

So in the mod, **one production building ≈ 1.8 stock buildings** of early throughput (your "1.5" estimate, if anything undersold). A second early building is close to wasted until ~20 workers — which is why building more than one barracks up front feels like it buys you nothing, and why the 2-gateway-into-4-units-into-expand identity gets muted.

---

## Protoss (rolls from Barracks / Factory / Starport)

| Unit | Mod cat (real) | Stock cat (real) | Mod % of stock | Notes |
|------|----------------|------------------|----------------|-------|
| Zealot | 21 (15.0) | 38 (27.1) | **55%** | stock warp-in 30.8 |
| Stalker | **18 (12.9)** | 38 (27.1) | **47%** | Factory-only; warp-in 30.8 |
| Sentry | 21 (15.0) | 32.66 (23.3) | **64%** | warp-in 30.8 |
| Adept | 25 (17.9) | 42 (30.0) | 60% | *not in any roll pool* (train entry only) |
| HighTemplar | 42 (30.0) | 60.66 (43.3) | 69% | warp-in 49 |
| Immortal | 72.8 (52.0) | 55 (39.3) | **132%** | *slower than stock* |
| Colossus | 67.2 (48.0) | 75 (53.6) | 90% | |
| Observer | 25 (17.9) | 25 (17.9) | 100% | *not in any roll pool* (utility) |
| Phoenix | 30.8 (22.0) | 35 (25.0) | 88% | |
| VoidRay | 49 (35.0) | 60.2 (43.0) | 81% | |
| Tempest | 58.8 (42.0) | 75 (53.6) | 78% | |
| Archon | 70 (50.0) | (2 templar + 12s merge) | — | **not comparable** — mod direct-trains it; stock is a merge |

## Terran

| Unit | Mod cat (real) | Stock cat (real) | Mod % of stock | Notes |
|------|----------------|------------------|----------------|-------|
| Marine | 15 (10.7) | 25 (17.9) | **60%** | |
| Marauder | 28 (20.0) | 30 (21.4) | 93% | |
| Ghost | 40 (28.6) | 40 (28.6) | 100% | identical (base fallback) |
| Hellion | 21 (15.0) | 30 (21.4) | **70%** | |
| SiegeTank | 45 (32.1) | 45 (32.1) | 100% | identical (base fallback) |
| Thor | 67.2 (48.0) | 60 (42.9) | **112%** | *slower than stock* |
| VikingFighter | 42 (30.0) | 42 (30.0) | 100% | identical (base fallback) |
| Liberator | 49 (35.0) | 60 (42.9) | 82% | |
| Raven | 48 (34.3) | 48 (34.3) | 100% | identical (base fallback) |
| Battlecruiser | 70 (50.0) | 90 (64.3) | 78% | |
| DuskWing | 39.2 (28.0) | (Banshee 60 / 42.9) | 65% | merc variant of Banshee |

*Campaign-only units with no LotV-MP equivalent (Vulture 21, Firebat 21, Medic 26.6, Diamondback 38, Goliath 43, WarHound 72.8, Wraith 30.8) are omitted from the comparison — no stock baseline.*

## Zerg (for completeness — see caveat)

> Zerg build times are **not decision-relevant for the mod**: stock Zerg makes many units simultaneously from larva, so the per-unit time doesn't map onto the mod's one-slot-at-a-time production. Included only for reference.

| Unit | Mod cat | Stock cat | Mod % of stock |
|------|---------|-----------|----------------|
| Hydralisk | 26 | 33 | 79% |
| Mutalisk | 20 | 33 | 61% |
| Roach | 25.2 | 27 | 93% |
| Infestor | 37.8 | 50 | 76% |
| Ultralisk | 67.2 | 55 | **122%** |
| Viper | 39.2 | 40 | 98% |
| Queen | 32 | 50 | 64% |
| Zergling | 5.6 (per unit) | 24 (per pair) | — (model differs) |
| Lurker | 37.8 (train) | 25.25 (morph) | — (model differs) |
| Overseer | 35 (train) | 16.67 (morph) | — (model differs) |

---

## Secondary finding: a unit's build time changes with which facility rolls it

Because build times live on each facility's train ability, the **same unit builds at different speeds depending on the random roll**:
- **Stalker:** was 18s from Factory, 25.2s from Barracks (a 40% swing)
- **Archon:** was 45s from Barracks, 70s from Factory

**Resolved — merged 2026-08-03 (PR #28):** each unit rolls from exactly one facility, so the second entry was always dead config. The stale `BarracksTrain` Stalker (25.2s) and Archon (45s) InfoArrays have been **removed**. Canonical times are the **Factory** values: **Stalker 18, Archon 70**.

---

## Suggested build times (v1 — to playtest)

*Build times as **catalog (real)** seconds — catalog is the raw XML value, real is in-game "Faster" (÷1.4). Starting points to test, not final values. Changed values in **bold**.*

**Design logic:**
- **Raise the cheap early openers** toward ~70–85% of stock so one production building no longer saturates opening income — the lever that restores the "how many buildings before you expand/tech" branch.
- **Bring the two over-stock heavies down to stock** (Immortal 72.8 → 55, Thor 67.2 → 60) — your call; they were slower than stock for no reason.
- **Leave everything already at ~78–100% of stock alone** — the mod stays faster than stock across the board, so transitions still feel quick.
- **Relationship rules applied:** Zergling ≈ ¼ Zealot · Hydralisk ≈ 2× Marine.

**Income columns (%Min / %Gas):** *"if one production facility makes this unit non-stop, what share of a fully-staffed one-base economy does it eat?"* Basis: **minerals ≈ 925/min (16 workers); gas ≈ 650/min (325 per geyser × 2 geysers, 6 workers)**. Computed at the **Suggested** build time (rows with no change = live values). The *relative* ordering between units is exact; only the absolute anchoring depends on those income figures.

### Barracks
| Unit | Cost (m/g) | Current | Suggested | %Min | %Gas | Why |
|------|-----------|---------|-----------|------|------|-----|
| Zergling | 25/0 | 5.6 (4.0) | **7 (5.0)** | 32% | — | ≈ ¼ Zealot (your rule) |
| Zealot | 100/0 | 21 (15.0) | **28 (20.0)** | 32% | — | key opener → ~74% stock (your anchor) |
| Marine | 50/25 | 15 (10.7) | **20 (14.3)** | 23% | 16% | opener → 80% stock (your anchor) |
| Hydralisk | 100/50 | 26 (18.6) | **40 (28.6)** | 23% | 16% | ≈ 2× Marine (your rule) — see note |
| Queen | 175/50 | 32 (22.9) | 32 (22.9) | 50% | 20% | keep — support, not a masser |
| Firebat | 100/50 | 21 (15.0) | 21 (15.0) | 43% | 31% | keep — slot-3 gas bio |
| Marauder | 100/25 | 28 (20.0) | 28 (20.0) | 32% | 12% | keep — already 93% |
| Sentry | 50/100 | 21 (15.0) | **26 (18.6)** | 17% | 50% | opener → ~80% stock |
| Medic | 75/50 | 26.6 (19.0) | 26.6 (19.0) | 26% | 24% | keep — support |
| Ghost | 150/125 | 40 (28.6) | 40 (28.6) | 34% | 40% | keep — already stock |
| Infestor | 50/150 | 37.8 (27.0) | 37.8 (27.0) | 12% | 51% | keep — caster, 76% |
| HighTemplar | 50/150 | 42 (30.0) | 42 (30.0) | 11% | 46% | keep — caster, 69% |

### Factory
| Unit | Cost (m/g) | Current | Suggested | %Min | %Gas | Why |
|------|-----------|---------|-----------|------|------|-----|
| Stalker | 125/50 | 18 (12.9) | **21.6 (15.4)** | 53% | 30% | +20% nudge — Factory unit, fine to stay fast (Gateway≈Barracks) |
| Vulture | 100/0 | 21 (15.0) | **26 (18.6)** | 35% | — | cheap opener; matches Hellion |
| Hellion | 100/0 | 21 (15.0) | **26 (18.6)** | 35% | — | opener → ~87% stock |
| Diamondback | 150/150 | 38 (27.1) | 38 (27.1) | 36% | 51% | keep |
| Immortal | 250/100 | 72.8 (52.0) | **55 (39.3)** | 41% | 24% | → stock (your call) |
| SiegeTank | 150/125 | 45 (32.1) | 45 (32.1) | 30% | 36% | keep — already stock |
| WarHound | 200/75 | 72.8 (52.0) | 72.8 (52.0) | 25% | 13% | keep — high for a supply-3 unit |
| Archon | 225/150 | 70 (50.0) | 70 (50.0) | 29% | 28% | keep — single canonical value |
| Lurker | 150/150 | 37.8 (27.0) | 37.8 (27.0) | 36% | 51% | keep |
| Goliath | 150/50 | 43 (30.7) | 43 (30.7) | 32% | 15% | keep |
| Thor | 300/150 | 67.2 (48.0) | **60 (42.9)** | 45% | 32% | → stock (your call) |
| Colossus | 300/200 | 67.2 (48.0) | 67.2 (48.0) | 41% | 38% | keep — already 90% |
| Ultralisk | 325/200 | 67.2 (48.0) | 67.2 (48.0) | 44% | 38% | keep — or 55 to match Thor/Immortal |

### Starport
| Unit | Cost (m/g) | Current | Suggested | %Min | %Gas | Why |
|------|-----------|---------|-----------|------|------|-----|
| Corsair | 150/100 | 30.8 (22.0) | 30.8 (22.0) | 44% | 42% | keep |
| Phoenix | 150/100 | 30.8 (22.0) | 30.8 (22.0) | 44% | 42% | keep — 88% |
| Wraith | 100/100 | 30.8 (22.0) | 30.8 (22.0) | 29% | 42% | keep |
| Viking | 125/75 | 42 (30.0) | 42 (30.0) | 27% | 23% | keep — stock |
| Liberator | 150/125 | 49 (35.0) | 49 (35.0) | 28% | 33% | keep — 82% |
| Mutalisk | 100/100 | 20 (14.3) | **28 (20.0)** | 32% | 46% | fastest flier at 61%; raise to ~85% |
| DuskWing | 200/150 | 39.2 (28.0) | 39.2 (28.0) | 46% | 49% | keep |
| VoidRay | 250/150 | 49 (35.0) | 49 (35.0) | 46% | 40% | keep — 81% |
| Raven | 75/150 | 48 (34.3) | 48 (34.3) | 14% | 40% | keep — stock |
| Tempest | 250/175 | 58.8 (42.0) | 58.8 (42.0) | 39% | 38% | keep — 78% |
| Viper | 75/200 | 39.2 (28.0) | 39.2 (28.0) | 17% | 66% | keep — 98% |
| Battlecruiser | 400/300 | 70 (50.0) | 70 (50.0) | 52% | 55% | keep — 78% |

**Note on Hydralisk:** the 2×-Marine rule lands it at 40 (~121% of stock), i.e. slightly *slower* than a stock Hydra. Defensible — it's a robust slot-2 generalist and this is anti-saturation — but if you'd rather keep it under stock, use Marine 18 → Hydra 36 (109%). Flagging so the number is a choice, not an accident.

**Reading the income columns:** one base comfortably sustains a single facility of *any* unit — mineral %s land in the 11–53% band, gas in 12–66%. So the question isn't "can I afford to build this back-to-back" (on one base, you always can); it's how many *parallel* facilities a base supports. Minerals cap that around 2–3 cheap-unit facilities; the gas-heavy units (Viper 66%, Battlecruiser 55%, Diamondback/Infestor/Lurker ~51%) are what push you toward a second base's gas once you want two of them running at once.

This keeps the mod **faster than stock across the board** (the lone exception is Hydralisk, and only if you take the 2× rule literally), so transitions stay quick — PiG's "too long" feeling lives in the tech/upgrade gating, a separate knob.

**Do not tune blind** — math-backed hypothesis, but it needs replays. Highest-value follow-up: get replay analysis working (WA-065) so opening-building counts and first-army timings are measured, not eyeballed.

---

## Stat score (raw combat value) — added 2026-08-04

A rough, transparent heuristic for *"how strong are this unit's raw combat stats?"* — meant to catch **A-move cost outliers**, not to be a balance verdict. It ignores abilities, bonus damage, micro, and role.

**Formula (per weapon):**

> **StatScore = ( 15 × DPS + effEHP ) × rangeFactor × speedFactor ÷ 100**
> - **effEHP** = (Health + Shields) × (1 + 0.05 × Armor)
> - **rangeFactor** = 1 + 0.12 × Range
> - **speedFactor** = 1 + 0.10 × (Speed − 2.25)   *(2.25 = standard ground speed)*

**Why this shape:** a **linear** blend on purpose — *not* DPS × EHP. A product makes big units look quadratically cost-efficient and buries the outliers; a linear blend keeps a fairly-priced unit near a *constant* score/cost, so the outliers are the rows that deviate. Weights are set so DPS and EHP contribute about equally for a typical unit (~10 DPS, ~150 EHP), i.e. **1 DPS ≈ 15 EHP**. Range and speed are multipliers because they're force-multipliers, not raw output. All constants are tunable — say the word if a weight feels off.

**Two columns, two questions.** **Score** (the linear blend) ÷ cost answers *"is this priced fairly / is it an outlier?"* — that's what **/Min** and **/Gas** divide. **CmbtPwr = DPS × effEHP ÷ 100** (the raw product, no range/speed factors) answers a *different* question: *"who wins a cost-neutral, even-supply clash?"* It's superlinear on purpose (2× stats → 4× power — a unit twice as good kills twice as fast *and* lives twice as long), so **don't** divide it by cost; read it as a raw fight-power ranking. SC2 prices much closer to the linear Score than to CmbtPwr, which is exactly why big units win cost-neutral fights (high CmbtPwr) yet aren't broken (supply, production time, splash-vulnerability, and focus-fire are the brakes cost can't show).

**Rules & caveats:**
- **Base DPS only** (no +vs-armored/light bonus) — so anti-armored/anti-light specialists (Marauder, Goliath-air, Immortal-in-practice) are *under*-scored vs their niche.
- **Multiple weapons → one score each** (Goliath grd/air, Siege Tank tank/sieged, Thor/BC/Tempest/Wraith). **/Min** and **/Gas** divide each weapon's score by the unit's *full* cost — so a versatile 2-weapon unit reads strong on both lines (that versatility *is* value).
- **Casters excluded** (Sentry, Medic, Infestor, High Templar, Raven, Viper). **Ghost, Queen, Corsair included** (real attacks) though they also have abilities.
- **Not captured:** abilities, mode-lock drawbacks (Liberator siege immobility, Tank siege setup, Lurker must burrow), shield regen, and versatility beyond the two booleans. Smell test, not a ruling.

### Barracks
| Unit | Weapon | Air | Grd | Splash | CmbtPwr | Score | /Min | /Gas |
|------|--------|:---:|:---:|:------:|--------:|------:|-----:|-----:|
| Zergling | Claws | – | Y | – | 2.5 | 1.5 | 0.062 | — |
| Zealot | Psi Blades | – | Y | – | 20.9 | 3.6 | 0.036 | — |
| Marine | Gauss | Y | Y | – | 3.1 | 2.4 | 0.048 | 0.096 |
| Hydralisk | Needle Spines | Y | Y | – | 13.1 | 4.9 | 0.049 | 0.098 |
| Queen | Talons (grd) | – | Y | – | 14.7 | 4.7 | 0.027 | 0.094 |
| Queen | Acid (air) | Y | – | – | 16.5 | 5.7 | 0.032 | 0.113 |
| Marauder | Punisher | – | Y | – | 8.8 | 4.0 | 0.040 | 0.159 |
| Ghost | C10 Rifle | Y | Y | – | 10.0 | 4.5 | 0.030 | 0.036 |
| Firebat | Flamethrower | – | Y | Y | 6.0 | 2.4 | 0.024 | 0.047 |

### Factory
| Unit | Weapon | Air | Grd | Splash | CmbtPwr | Score | /Min | /Gas |
|------|--------|:---:|:---:|:------:|--------:|------:|-----:|-----:|
| Vulture | Spikes | – | Y | – | 4.6 | 3.5 | 0.035 | — |
| Hellion | Infernal | – | Y | Y | 2.9 | 2.6 | 0.026 | — |
| Stalker | Particle | Y | Y | – | 11.8 | 5.0 | 0.040 | 0.101 |
| Diamondback | Railgun | – | Y | – | 21.0 | 6.6 | 0.044 | 0.044 |
| Immortal | Phase | – | Y | – | 39.4 | 8.6 | 0.035 | 0.086 |
| Siege Tank | 90mm (tank) | – | Y | – | 26.5 | 7.4 | 0.049 | 0.059 |
| Siege Tank | Crucio (sieged) | – | Y | Y | 24.4 | 9.8 | 0.065 | 0.078 |
| War Hound | Haywire | – | Y | – | 40.9 | 9.6 | 0.048 | 0.129 |
| Archon | Psi Shockwave | Y | Y | Y | 51.5 | 8.3 | 0.037 | 0.055 |
| Lurker | Spines | – | Y | Y | 19.9 | 7.6 | 0.051 | 0.051 |
| Goliath | Goliath (grd) | – | Y | – | 18.9 | 6.1 | 0.040 | 0.121 |
| Goliath | Goliath (air) | Y | – | – | 16.9 | 5.7 | 0.038 | 0.114 |
| Thor | Hammer (grd) | – | Y | – | 197.0 | 19.9 | 0.066 | 0.099 |
| Thor | Lance (air) | Y | – | – | 81.9 | 15.9 | 0.053 | 0.080 |
| Ultralisk | Kaiser Blades | – | Y | Y | 192.5 | 12.9 | 0.040 | 0.064 |
| Colossus | Thermal Lance | – | Y | Y | 48.9 | 11.8 | 0.039 | 0.059 |

### Starport
| Unit | Weapon | Air | Grd | Splash | CmbtPwr | Score | /Min | /Gas |
|------|--------|:---:|:---:|:------:|--------:|------:|-----:|-----:|
| Corsair | Neutron Flare | Y | – | Y | 20.0 | 5.9 | 0.039 | 0.059 |
| Phoenix | Ion Cannons | Y | – | – | 16.4 | 6.1 | 0.041 | 0.061 |
| Wraith | Laser (air) | Y | – | – | 11.2 | 4.8 | 0.048 | 0.048 |
| Wraith | Burst (grd) | – | Y | – | 6.6 | 3.9 | 0.039 | 0.039 |
| Viking | Lanzer (air) | Y | – | – | 13.5 | 6.2 | 0.050 | 0.083 |
| Liberator | AA missiles | Y | – | – | 10.1 | 4.3 | 0.029 | 0.034 |
| Liberator | Defender (grd) | – | Y | – | 84.4 | 19.7 | 0.132 | 0.158 |
| Mutalisk | Glaive | Y | Y | – | 7.1 | 3.3 | 0.033 | 0.033 |
| DuskWing | Banshee | – | Y | – | 50.4 | 11.0 | 0.055 | 0.073 |
| Void Ray | Swarm | Y | Y | – | 30.0 | 7.8 | 0.031 | 0.052 |
| Tempest | Tempest (air) | Y | – | – | 30.0 | 11.9 | 0.048 | 0.068 |
| Tempest | Tempest (grd) | – | Y | – | 39.9 | 11.3 | 0.045 | 0.064 |
| Battlecruiser | ATA (air) | Y | – | – | 140.4 | 16.0 | 0.040 | 0.053 |
| Battlecruiser | ATS (grd) | – | Y | – | 225.2 | 19.3 | 0.048 | 0.064 |

### Reading it
**Lead with /Min — minerals are the binding constraint in most games** (they fund workers, buildings, supply, and expansions; gas only funds unit-gas + upgrades, so it tends to bank). /Gas matters once you're actually gas-bound: late game, gas-heavy comps, or heavy upgrading.

- **By /Min** (excluding mode-locked Liberator-Defender): **Thor, sieged Siege Tank, Zergling, DuskWing** lead. Most single-role units cluster ~0.030–0.050 /Min; those poke above.
- **Goliath is *average* by /Min (0.040)** — its earlier "outlier" billing was purely /Gas (it's gas-cheap), which only helps when gas is tight. Its real edge is **versatility**: air+ground on one cheap-gas unit, so you never need a dedicated AA unit. Watch it for *that*, not for raw efficiency.
- **War Hound** sits a touch high on both (0.048 /Min, 0.129 /Gas) for 200/75 — a mild watch; ground-only, anti-mech-flavored.
- **Marauder's** 0.159 /Gas is an artifact: cheap gas (25) + base-DPS-only scoring (its value is +vs-armored, which the formula omits). Not a real outlier.
- **Liberator-Defender** tops /Min and /Gas but the score can't see it's **immobile and ground-only** in that mode. Discount heavily.

**CmbtPwr is the deathball lens** (cost-neutral clash): **Battlecruiser (225 grd / 140 air), Thor (197 grd), Ultralisk (193)** dominate — which is exactly why supply, production time, and focus-fire vulnerability are the balancing brakes, not price. Compare *that* column unit-to-unit for "who wins the fight"; compare the /Min column for "who's an efficiency outlier."

---

## Combat value per production-minute (by slot) — added 2026-08-04

The bridge between the two halves of this doc: **how much combat value one facility pumps out per minute** = score × 60 ÷ build-time(real). Two flavors:
- **Sc/min** — the *cost-aware* linear Score per minute (rewards cheap-fast units by design).
- **Cp/min** — the *raw fight-power* (DPS×EHP) per minute.

Computed at the **suggested** build times. **Compare within a slot** — same-slot units are roughly the same size, so no cost-normalizing is needed and a big gap = a build-time (or stat) outlier. Caveat: slots aren't *perfectly* uniform in cost (B1 Zergling 25 vs Zealot 100; B2 Marine 50 vs Queen 175), so within-slot reads cleanest in F1/F2/F3/S3 where costs are closer. And **base DPS only** — splash/bonus specialists (Colossus, Hellion, Firebat, Marauder, Lurker) are understated here.

### Barracks
| Slot | Unit | Weapon | Build (real) | Score | Sc/min | CmbtPwr | Cp/min |
|:----:|------|--------|-------------:|------:|-------:|--------:|-------:|
| B1 | Zergling | Claws | 5.0 | 1.5 | 18.6 | 2.5 | 30.2 |
| B1 | Zealot | Psi Blades | 20.0 | 3.6 | 10.8 | 20.9 | 62.8 |
| B2 | Hydralisk | Needle | 28.6 | 4.9 | 10.3 | 13.1 | 27.4 |
| B2 | Marine | Gauss | 14.3 | 2.4 | 10.1 | 3.1 | 13.2 |
| B2 | Queen | Talons (grd) | 22.9 | 4.7 | 12.3 | 14.7 | 38.6 |
| B2 | Queen | Acid (air) | 22.9 | 5.7 | 14.9 | 16.5 | 43.4 |
| B3 | Firebat | Flame | 15.0 | 2.4 | 9.4 | 6.0 | 23.9 |
| B3 | Marauder | Punisher | 20.0 | 4.0 | 12.0 | 8.8 | 26.4 |
| B4 | Ghost | C10 Rifle | 28.6 | 4.5 | 9.5 | 10.0 | 21.0 |

### Factory
| Slot | Unit | Weapon | Build (real) | Score | Sc/min | CmbtPwr | Cp/min |
|:----:|------|--------|-------------:|------:|-------:|--------:|-------:|
| F1 | Vulture | Spikes | 18.6 | 3.5 | 11.2 | 4.6 | 15.0 |
| F1 | Hellion | Infernal | 18.6 | 2.6 | 8.6 | 2.9 | 9.3 |
| F1 | Stalker | Particle | 15.4 | 5.0 | **19.5** | 11.8 | **45.7** |
| F2 | Diamondback | Railgun | 27.1 | 6.6 | 14.6 | 21.0 | 46.4 |
| F2 | Immortal | Phase | 39.3 | 8.6 | 13.2 | 39.4 | 60.1 |
| F2 | Siege Tank | 90mm (tank) | 32.1 | 7.4 | 13.7 | 26.5 | 49.4 |
| F2 | Siege Tank | Crucio (sieged) | 32.1 | 9.8 | 18.3 | 24.4 | 45.6 |
| F2 | War Hound | Haywire | 52.0 | 9.6 | 11.1 | 40.9 | 47.2 |
| F2 | Archon | Psi Shockwave | 50.0 | 8.3 | 9.9 | 51.5 | 61.8 |
| F2 | Lurker | Spines | 27.0 | 7.6 | 16.9 | 19.9 | 44.3 |
| F2 | Goliath | Goliath (grd) | 30.7 | 6.1 | 11.8 | 18.9 | 36.9 |
| F2 | Goliath | Goliath (air) | 30.7 | 5.7 | 11.2 | 16.9 | 32.9 |
| F3 | Thor | Hammer (grd) | 42.9 | 19.9 | **27.9** | 197.0 | **275.8** |
| F3 | Thor | Lance (air) | 42.9 | 15.9 | 22.3 | 81.9 | 114.7 |
| F3 | Ultralisk | Kaiser Blades | 48.0 | 12.9 | 16.1 | 192.5 | 240.6 |
| F3 | Colossus | Thermal Lance | 48.0 | 11.8 | 14.7 | 48.9 | 61.1 |

### Starport
| Slot | Unit | Weapon | Build (real) | Score | Sc/min | CmbtPwr | Cp/min |
|:----:|------|--------|-------------:|------:|-------:|--------:|-------:|
| S1 | Corsair | Neutron Flare | 22.0 | 5.9 | 16.0 | 20.0 | 54.6 |
| S1 | Phoenix | Ion Cannons | 22.0 | 6.1 | 16.6 | 16.4 | 44.7 |
| S1 | Wraith | Laser (air) | 22.0 | 4.8 | 13.0 | 11.2 | 30.5 |
| S1 | Wraith | Burst (grd) | 22.0 | 3.9 | 10.6 | 6.6 | 17.9 |
| S1 | Viking | Lanzer (air) | 30.0 | 6.2 | 12.4 | 13.5 | 27.0 |
| S2 | Liberator | AA missiles | 35.0 | 4.3 | 7.3 | 10.1 | 17.3 |
| S2 | Liberator | Defender (grd) | 35.0 | 19.7 | 33.8 | 84.4 | 144.7 |
| S2 | Mutalisk | Glaive | 20.0 | 3.3 | 9.8 | 7.1 | 21.2 |
| S2 | DuskWing | Banshee | 28.0 | 11.0 | **23.5** | 50.4 | **108.0** |
| S2 | Void Ray | Swarm | 35.0 | 7.8 | 13.3 | 30.0 | 51.4 |
| S3 | Tempest | Tempest (air) | 42.0 | 11.9 | 17.1 | 30.0 | 42.9 |
| S3 | Tempest | Tempest (grd) | 42.0 | 11.3 | 16.1 | 39.9 | 57.0 |
| S3 | Battlecruiser | ATA (air) | 50.0 | 16.0 | 19.2 | 140.4 | 168.5 |
| S3 | Battlecruiser | ATS (grd) | 50.0 | 19.3 | 23.2 | 225.2 | 270.2 |

### Within-slot flags
- **F1 — Stalker runs away with the slot.** ~**2×** its slotmates on both Sc/min (19.5 vs Vulture 11.2 / Hellion 8.6) and Cp/min (45.7 vs 15.0 / 9.3), *even after* the +20% build nerf. Either its build time has room to go higher, or it earns it as the gas-costed premium option. (Vulture's KD8 + speed and Hellion's splash + vs-Light aren't in these numbers, so they're a bit understated.)
- **F3 — Thor tops the slot and builds fastest.** Highest Sc/min (27.9) and Cp/min (275.8) of F3, at a 60 build vs Ultra/Colossus 67.2. Worth eyeing Thor's build time *up* (or Colossus's *down*). Colossus's line-splash value is understated here.
- **S2 — DuskWing is the "buffed Banshee" you flagged.** Sc/min 23.5 / Cp/min 108 lead the slot (ignoring mode-locked Liberator-Defender). Mutalisk is the slot's floor (9.8 / 21.2). 
- **Goliath is *not* a throughput outlier** — bottom of F2 on Cp/min (37/33). This reconfirms its edge is **versatility** (air+ground, cheap gas), not value-per-minute. Good sanity check against the earlier /Gas scare.

Use Sc/min to ask *"is this unit's build time fair for its cost-tier?"* and Cp/min to ask *"how fast does this facility crank raw army power?"* — both read best down a single slot.

### Current → suggested (only the units whose build time changes)
Everything else is unchanged, so its value/min is identical to the table above. (Sentry's build also changes 21→26 but it's excluded as a caster.)

| Slot | Unit | Weapon | Build (real) | Sc/min | Cp/min |
|:----:|------|--------|:------------:|:------:|:------:|
| B1 | Zergling | Claws | 4.0 → 5.0 | 23.2 → 18.6 | 37.8 → 30.2 |
| B1 | Zealot | Psi Blades | 15.0 → 20.0 | 14.5 → 10.8 | 83.8 → 62.8 |
| B2 | Marine | Gauss | 10.7 → 14.3 | 13.4 → 10.1 | 17.6 → 13.2 |
| B2 | Hydralisk | Needle | 18.6 → 28.6 | 15.9 → 10.3 | 42.2 → 27.4 |
| F1 | Stalker | Particle | 12.9 → 15.4 | 23.5 → 19.5 | 54.9 → 45.7 |
| F1 | Vulture | Spikes | 15.0 → 18.6 | 13.8 → 11.2 | 18.6 → 15.0 |
| F1 | Hellion | Infernal | 15.0 → 18.6 | 10.6 → 8.6 | 11.5 → 9.3 |
| F2 | Immortal | Phase | 52.0 → 39.3 | 10.0 → 13.2 | 45.4 → 60.1 |
| F3 | Thor | Hammer (grd) | 48.0 → 42.9 | 24.9 → 27.9 | 246.2 → 275.8 |
| F3 | Thor | Lance (air) | 48.0 → 42.9 | 19.9 → 22.3 | 102.4 → 114.7 |
| S2 | Mutalisk | Glaive | 14.3 → 20.0 | 13.7 → 9.8 | 29.7 → 21.2 |

**Two directions here — the second is a heads-up:**
- **Openers drop (intended).** All eight cheap openers lose value/min — that *is* the anti-saturation goal: slower openers mean one building no longer floods the map, restoring the opening-building decision.
- **Immortal & Thor *rise* — that's a throughput buff, not just a "fix."** "Bring them to stock" actually *shortens* their build (they were slower than stock), so their value/min goes **up** (Immortal 10.0 → 13.2 Sc/min; Thor 24.9 → 27.9). If the goal was just "stop them being weirdly slow," fine — but be aware it makes them come out *faster*, not merely feel cheaper. If you didn't want to speed up Thor/Immortal production, leave those two at their current times.
