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

*Catalog (standard-speed) seconds; real-game ≈ ÷1.4. Starting points to test, not final values. Changed values in **bold**.*

**Design logic:**
- **Raise the cheap early openers** toward ~70–85% of stock so one production building no longer saturates opening income — the lever that restores the "how many buildings before you expand/tech" branch.
- **Bring the two over-stock heavies down to stock** (Immortal 72.8 → 55, Thor 67.2 → 60) — your call; they were slower than stock for no reason.
- **Leave everything already at ~78–100% of stock alone** — the mod stays faster than stock across the board, so transitions still feel quick.
- **Relationship rules applied:** Zergling ≈ ¼ Zealot · Hydralisk ≈ 2× Marine.

### Barracks
| Unit | Current | Suggested | Stock | Why |
|------|---------|-----------|-------|-----|
| Zergling | 5.6 | **7** | 24 (pair) | ≈ ¼ Zealot (your rule) |
| Zealot | 21 | **28** | 38 | key opener → ~74% stock (your anchor) |
| Marine | 15 | **20** | 25 | opener → 80% stock (your anchor) |
| Hydralisk | 26 | **40** | 33 | ≈ 2× Marine (your rule) — lands slightly *above* stock; see note |
| Queen | 32 | 32 | 50 | keep — support, not a masser (64%) |
| Firebat | 21 | 21 | — | keep — slot-3 gas bio |
| Marauder | 28 | 28 | 30 | keep — already 93% |
| Sentry | 21 | **26** | 32.66 | opener → ~80% stock |
| Medic | 26.6 | 26.6 | — | keep — support |
| Ghost | 40 | 40 | 40 | keep — already stock |
| Infestor | 37.8 | 37.8 | 50 | keep — caster, 76% |
| HighTemplar | 42 | 42 | 60.66 | keep — caster, 69% |

### Factory
| Unit | Current | Suggested | Stock | Why |
|------|---------|-----------|-------|-----|
| Stalker | 18 | **21.6** | 38 | +20% nudge only — Gateway≈Barracks, not Factory (which costs more to build), so it's fine that it stays fast; stock not a target for a Factory unit |
| Vulture | 21 | **26** | — | cheap opener; keep in line with Hellion |
| Hellion | 21 | **26** | 30 | opener → ~87% stock |
| Diamondback | 38 | 38 | — | keep |
| Immortal | 72.8 | **55** | 55 | → stock (your call) |
| SiegeTank | 45 | 45 | 45 | keep — already stock |
| WarHound | 72.8 | 72.8 | — | keep — but high for a supply-3 unit; consider the Thor/Immortal tier |
| Archon | 70 | 70 | (merge) | keep — single canonical value now |
| Lurker | 37.8 | 37.8 | (morph) | keep |
| Goliath | 43 | 43 | — | keep |
| Thor | 67.2 | **60** | 60 | → stock (your call) |
| Colossus | 67.2 | 67.2 | 75 | keep — already 90% |
| Ultralisk | 67.2 | 67.2 | 55 | keep — but same logic as Thor/Immortal would put it at 55 (your call) |

### Starport
| Unit | Current | Suggested | Stock | Why |
|------|---------|-----------|-------|-----|
| Corsair | 30.8 | 30.8 | — | keep |
| Phoenix | 30.8 | 30.8 | 35 | keep — 88% |
| Wraith | 30.8 | 30.8 | — | keep |
| Viking | 42 | 42 | 42 | keep — stock |
| Liberator | 49 | 49 | 60 | keep — 82% |
| Mutalisk | 20 | **28** | 33 | fastest flier at 61%; raise to ~85% |
| DuskWing | 39.2 | 39.2 | (Banshee 60) | keep |
| VoidRay | 49 | 49 | 60.2 | keep — 81% |
| Raven | 48 | 48 | 48 | keep — stock |
| Tempest | 58.8 | 58.8 | 75 | keep — 78% |
| Viper | 39.2 | 39.2 | 40 | keep — 98% |
| Battlecruiser | 70 | 70 | 90 | keep — 78% |

**Note on Hydralisk:** the 2×-Marine rule lands it at 40 (~121% of stock), i.e. slightly *slower* than a stock Hydra. Defensible — it's a robust slot-2 generalist and this is anti-saturation — but if you'd rather keep it under stock, use Marine 18 → Hydra 36 (109%). Flagging so the number is a choice, not an accident.

This keeps the mod **faster than stock across the board** (the lone exception is Hydralisk, and only if you take the 2× rule literally), so transitions stay quick — PiG's "too long" feeling lives in the tech/upgrade gating, a separate knob.

**Do not tune blind** — math-backed hypothesis, but it needs replays. Highest-value follow-up: get replay analysis working (WA-065) so opening-building counts and first-army timings are measured, not eyeballed.
