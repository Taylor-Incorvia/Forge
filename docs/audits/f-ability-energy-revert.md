# F_ ability energy costs vs stock — revert planning

Generated 2026-07-27, against `main`. Reference for the planned pass to move `F_` caster-spell energy costs **back toward stock**. Values pulled live from `ForgeModLowConfidence.SC2Mod/.../AbilData.xml`; stock values from `reference/` (retail LotV multiplayer where the ability exists there, HotS/campaign/co-op otherwise).

**Why revert now:** every caster now gets **+25% energy regen** when it researches its rolled spell (shipped, PR #19). That buff already improves *sustain* (casts over the game). The old per-cast discounts were the *previous* way to reward the tech — with regen doing that job, the discounts are largely redundant, and keeping spells at stock cost keeps single-fight *burst* honest. So: revert the discount, keep the regen.

**Your stated split:** revert the ones **in standard multiplayer** to stock; use judgment on the ones that **only exist in campaign/co-op** (often overpowered precisely because no MP opponent was ever balanced against being on the receiving end).

Only **active, energy-gated, pooled** `F_` spells are listed. Cooldown/life-gated `F_` abilities (Blink, Charge, D8/KD8 Charge, Hyperjump, Grapple, Yamato, Stimpack, Corrosive Bile, Tempest Blast) are excluded — energy cost doesn't govern them.

---

## Tier 1 — In standard SC2 multiplayer → revert to stock (clear)
Discounted below stock today; the units exist in retail/HotS MP, so stock is a known-fair number.

| `F_` spell | On (native + rollable) | Current | Stock | Change |
|---|---|---|---|---|
| **F_ForceField** | Sentry | 40 | **50** | +10 |
| **F_GuardianShield** | Sentry | 50 | **75** | +25 |
| **F_GravitonBeam** | Phoenix | 40 | **50** | +10 |
| **F_BuildAutoTurret** | Raven | 40 | **50** | +10 |
| **F_FungalGrowth** | Infestor | 50 | **75** | +25 |
| **F_NeuralParasite** | Infestor | 75 | **100** | +25 |
| **F_ParasiticBomb** | Viper | 100 | **125** | +25 |
| **F_Yoink** (Abduct) | Viper | 50 | **75** | +25 |

**F_BlindingCloud** (Viper, retail MP): already at stock **100** — no change.

---

## Tier 2 — Borderline (was in MP, cut from current LotV) → probably revert, your call
Existed in an earlier MP (HotS-era Raven) but isn't in the current live game.

| `F_` spell | On | Current | Stock (HotS) | Note |
|---|---|---|---|---|
| **F_SeekerMissile** | Raven | 100 | 125 | HotS Raven ability, removed from LotV. Stock 125 is a real MP-tested number → reverting to 125 is defensible. |
| **F_RavenScramblerMissile** | Raven | 75 | 75 | Already at stock. No change. |

---

## Tier 3 — Not in standard MP → your judgment (no "correct" stock to trust)
Campaign / co-op / SC1 spells. Their "stock" numbers were never balanced for competitive MP, so treat them as *reference, not target*. These are the ones you flagged as potentially overpowered "because nobody had to be on the receiving end."

| `F_` spell | On | Current | "Stock" | Note |
|---|---|---|---|---|
| **F_MissilePods** | rolled (Wraith etc.) | 75 | 125 (campaign) | You already reworked it (flat 60 to air, low-tier). 75 is a deliberate custom value — leave unless it feels off. |
| **F_CorsairMPDisruptionWeb** | Corsair + rolled | 50 | 100 (SC1/co-op) | Low-tier utility; you set 50 on purpose. Reverting to 100 would gut it at low tiers. Recommend keep, or a small bump. |
| **F_Irradiate** | rolled | 40 | 25 (campaign) | **Already ABOVE stock** — you deliberately raised 25→40 because 25 was "insane." Keep. Do not "revert" (that's a buff). |
| **F_ArbiterMPRecall** | rolled (s3/s4) | 125 | 100 (SC1/co-op) | **Already ABOVE stock** — deliberately, because Recall is a game-swinging army teleport. Keep above stock; reverting to 100 is a buff you don't want. |

---

## Inactive (pooled = NO right now — energy cost is moot until/unless re-enabled)
Listed so nobody "fixes" a cost on a spell that isn't in the game: **F_ArbiterMPStasisField** (removed — un-telegraphed army freeze), **F_DefensiveMatrix**, **F_OdinBarrage**, **F_LightningBomb**, **F_Transfusion** (WA-044), **F_PsionicShockwave**, **F_TempestDisruptionBlast**.

---

## Suggested edit set (Tier 1 only — the safe pass)
Eight edits in `AbilData.xml`, `<Vital index="Energy" value="...">` on each `CAbil…` id:
`F_ForceField` 40→50 · `F_GravitonBeam` 40→50 · `F_BuildAutoTurret` 40→50 · `F_GuardianShield` 50→75 · `F_FungalGrowth` 50→75 · `F_NeuralParasite` 75→100 · `F_ParasiticBomb` 100→125 · `F_Yoink` 50→75.

Leave Tier 3 as-is (all deliberate). Decide Tier 2 (`F_SeekerMissile` 100→125) by feel.
