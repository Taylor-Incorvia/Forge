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

**Income columns (%Min / %Gas):** *"if one production facility makes this unit non-stop, what share of a fully-staffed one-base economy does it eat?"* Basis: **16 mineral workers ≈ 925/min, 6 gas workers ≈ 325/min**. Computed at the **Suggested** build time (rows with no change = live values). A **gas % over 100%** means a single facility out-drains one base's gas — you can't build it back-to-back on one base's gas alone. The *relative* ordering between units is exact; only the absolute anchoring depends on those income figures.

### Barracks
| Unit | Cost (m/g) | Current | Suggested | %Min | %Gas | Why |
|------|-----------|---------|-----------|------|------|-----|
| Zergling | 25/0 | 5.6 (4.0) | **7 (5.0)** | 32% | — | ≈ ¼ Zealot (your rule) |
| Zealot | 100/0 | 21 (15.0) | **28 (20.0)** | 32% | — | key opener → ~74% stock (your anchor) |
| Marine | 50/25 | 15 (10.7) | **20 (14.3)** | 23% | 32% | opener → 80% stock (your anchor) |
| Hydralisk | 100/50 | 26 (18.6) | **40 (28.6)** | 23% | 32% | ≈ 2× Marine (your rule) — see note |
| Queen | 175/50 | 32 (22.9) | 32 (22.9) | 50% | 40% | keep — support, not a masser |
| Firebat | 100/50 | 21 (15.0) | 21 (15.0) | 43% | 62% | keep — slot-3 gas bio |
| Marauder | 100/25 | 28 (20.0) | 28 (20.0) | 32% | 23% | keep — already 93% |
| Sentry | 50/100 | 21 (15.0) | **26 (18.6)** | 17% | 99% | opener → ~80% stock |
| Medic | 75/50 | 26.6 (19.0) | 26.6 (19.0) | 26% | 49% | keep — support |
| Ghost | 150/125 | 40 (28.6) | 40 (28.6) | 34% | 81% | keep — already stock |
| Infestor | 50/150 | 37.8 (27.0) | 37.8 (27.0) | 12% | 103% | keep — caster, 76% |
| HighTemplar | 50/150 | 42 (30.0) | 42 (30.0) | 11% | 92% | keep — caster, 69% |

### Factory
| Unit | Cost (m/g) | Current | Suggested | %Min | %Gas | Why |
|------|-----------|---------|-----------|------|------|-----|
| Stalker | 125/50 | 18 (12.9) | **21.6 (15.4)** | 53% | 60% | +20% nudge — Factory unit, fine to stay fast (Gateway≈Barracks) |
| Vulture | 100/0 | 21 (15.0) | **26 (18.6)** | 35% | — | cheap opener; matches Hellion |
| Hellion | 100/0 | 21 (15.0) | **26 (18.6)** | 35% | — | opener → ~87% stock |
| Diamondback | 150/150 | 38 (27.1) | 38 (27.1) | 36% | 102% | keep |
| Immortal | 250/100 | 72.8 (52.0) | **55 (39.3)** | 41% | 47% | → stock (your call) |
| SiegeTank | 150/125 | 45 (32.1) | 45 (32.1) | 30% | 72% | keep — already stock |
| WarHound | 200/75 | 72.8 (52.0) | 72.8 (52.0) | 25% | 27% | keep — high for a supply-3 unit |
| Archon | 225/150 | 70 (50.0) | 70 (50.0) | 29% | 55% | keep — single canonical value |
| Lurker | 150/150 | 37.8 (27.0) | 37.8 (27.0) | 36% | 103% | keep |
| Goliath | 150/50 | 43 (30.7) | 43 (30.7) | 32% | 30% | keep |
| Thor | 300/150 | 67.2 (48.0) | **60 (42.9)** | 45% | 65% | → stock (your call) |
| Colossus | 300/200 | 67.2 (48.0) | 67.2 (48.0) | 41% | 77% | keep — already 90% |
| Ultralisk | 325/200 | 67.2 (48.0) | 67.2 (48.0) | 44% | 77% | keep — or 55 to match Thor/Immortal |

### Starport
| Unit | Cost (m/g) | Current | Suggested | %Min | %Gas | Why |
|------|-----------|---------|-----------|------|------|-----|
| Corsair | 150/100 | 30.8 (22.0) | 30.8 (22.0) | 44% | 84% | keep |
| Phoenix | 150/100 | 30.8 (22.0) | 30.8 (22.0) | 44% | 84% | keep — 88% |
| Wraith | 100/100 | 30.8 (22.0) | 30.8 (22.0) | 29% | 84% | keep |
| Viking | 125/75 | 42 (30.0) | 42 (30.0) | 27% | 46% | keep — stock |
| Liberator | 150/125 | 49 (35.0) | 49 (35.0) | 28% | 66% | keep — 82% |
| Mutalisk | 100/100 | 20 (14.3) | **28 (20.0)** | 32% | 92% | fastest flier at 61%; raise to ~85% |
| DuskWing | 200/150 | 39.2 (28.0) | 39.2 (28.0) | 46% | 99% | keep |
| VoidRay | 250/150 | 49 (35.0) | 49 (35.0) | 46% | 79% | keep — 81% |
| Raven | 75/150 | 48 (34.3) | 48 (34.3) | 14% | 81% | keep — stock |
| Tempest | 250/175 | 58.8 (42.0) | 58.8 (42.0) | 39% | 77% | keep — 78% |
| Viper | 75/200 | 39.2 (28.0) | 39.2 (28.0) | 17% | 132% | keep — 98% |
| Battlecruiser | 400/300 | 70 (50.0) | 70 (50.0) | 52% | 111% | keep — 78% |

**Note on Hydralisk:** the 2×-Marine rule lands it at 40 (~121% of stock), i.e. slightly *slower* than a stock Hydra. Defensible — it's a robust slot-2 generalist and this is anti-saturation — but if you'd rather keep it under stock, use Marine 18 → Hydra 36 (109%). Flagging so the number is a choice, not an accident.

**Reading the income columns:** most *mineral* %s land in the 25–50% band — a single facility never eats a whole base's minerals, which is exactly why one base can feed several production buildings (and why the opening-building count matters). *Gas* is the real bottleneck: everything at/over 100% gas (Infestor, Lurker, Diamondback, Viper, Battlecruiser — with Sentry, DuskWing right at the line) can't be produced back-to-back on a single base's gas. That's the natural brake on the gas-heavy tech units, working as intended.

This keeps the mod **faster than stock across the board** (the lone exception is Hydralisk, and only if you take the 2× rule literally), so transitions stay quick — PiG's "too long" feeling lives in the tech/upgrade gating, a separate knob.

**Do not tune blind** — math-backed hypothesis, but it needs replays. Highest-value follow-up: get replay analysis working (WA-065) so opening-building counts and first-army timings are measured, not eyeballed.
