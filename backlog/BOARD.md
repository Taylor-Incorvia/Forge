# Wildcard Arena — Board

Season 1. See [README](./README.md) for how we work. Say **"give me a ticket"** and I'll hand you the top `todo` item.

---

## 🔨 In progress
- [WA-038](./WA-038-tempest-remove-tectonic-destabilizers.md) — Tectonic Destabilizers (`TempestGroundAttackUpgrade`) passive button hidden via a Show-node requirement override (index-independent). **Merged to main (PR #2)** — one open AC: eyeball the Tempest card in-game to confirm it's gone + nothing else disturbed.
- [WA-014](./WA-014-verify-stalker-blink-range.md) — stalker blink range re-enabled in galaxy (root cause: first push missed the `PlacementRange` line; data complete since b40c4e6). **Awaiting production test** — roll it onto a Stalker, confirm range grows.

---

## ✅ Ready to grab (`todo`)
Groomed, scoped, has acceptance criteria. Pull from the top.

| ID | Ticket | Size | Phase |
|----|--------|------|-------|
| [WA-024](./WA-024-ghost-academy-nuke.md) | Enable nuke (Ghost Academy arms, Ghost fires) | M | 1 |
| [WA-044](./WA-044-transfuse-targeting.md) | Transfuse should heal (almost) any unit — kill the Biological target gate | S | 1 |
| [WA-045](./WA-045-editor-caching-settings.md) | Investigate if editor Documents-prefs settings fix the caching pain | S | 1 |
| [WA-036](./WA-036-overseer-hotkey.md) | Overseer hotkey won't render (cursed F_Blink class) | S | 1 |
| [WA-039](./WA-039-arsenal-paper-design.md) | Arsenal modal — paper design + info spec (design-first; epic WA-001) | S | 1 |
| [WA-048](./WA-048-colossus-thermal-lance-tooltip.md) | Tooltip: Colossus starts with Extended Thermal Lance (9 range) | S | 1 |
| [WA-043](./WA-043-unit-stats-reference-doc.md) | Doc: full per-unit stats reference (weapons/HP/armor/speed/build) | M | 1 |
| [WA-047](./WA-047-gameplay-clips.md) | Play + capture gameplay clips (every Reddit post needs one) | M | 4 (parallel) |

---

## 🧊 Needs grooming (`backlog`)
Real work, but not ready to grab — needs a decision or a problem statement first.

| ID | Ticket | Size | Phase | Blocked on |
|----|--------|------|-------|------------|
| [WA-001](./WA-001-arsenal-modal.md) | Your Arsenal modal (**EPIC**) — design-first plan captured; grab WA-039 to start | L | 1 | Build phases (2–7) groom after the design lands |
| [WA-034](./WA-034-concussive-shells-upgrade.md) | Slow-on-attack (Concussive) — per-unit upgrades | M | 1 | Pick units/slots (Marauder stock = quick slice 1) |
| [WA-035](./WA-035-lifesteal-upgrade.md) | Lifesteal — per-unit upgrades on non-shielded units | M | 1 | Pick unit subset + fraction (15/20%) |

---

## 🌟 Nice to have (ticketed)
_Lowest priority. Ordered bottom-up toward "never" (other races — see icebox)._
| ID | Ticket | Size | Phase |
|----|--------|------|-------|
| [WA-030](./WA-030-ability-telegraph.md) | Ability telegraph — icon/name above caster (upgrade abilities) | M | 1 |
| [WA-046](./WA-046-ultralisk-chitinous-visual.md) | Ultralisk looks different when it has Chitinous Plating (readability) | M | 1 |

---

## 📋 Later phases — not yet ticketed
We'll turn these into tickets when Phase 1 winds down. Listed so nothing's forgotten.

**Phase 2 — Documentation:** What is WA · How drafting works · Building slots · Unit list · Upgrade list · FAQ · Beginner guide · Patch notes _(unit/upgrade lists can reuse the matrices from WA-003/WA-004)_
**Phase 3 — Website:** Vue 3 + Vite + GitHub Pages · landing · how-to-play · docs · patch notes · Discord invite · tournament page · embedded YouTube
**Phase 4 — Marketing:** logo + palette · short trailer/hook video · Reddit/TeamLiquid/YouTube clips
**Phase 5 — Matchmaking:** Discord bot MVP (queue, pair, report, standings) · Queue Night format (Tue/Fri Arena)
**Phase 6 — Streaming:** GameHeart integration (supply/resources/workers observer overlay)
**Phase 7 — Tournament:** first Season 1 event · replay library · YouTube clips

**Nice-to-have (from Reddit feedback):** a genuinely-random "chaos" mode · Banelings in the pool · send replays to casters (Lowko/Uthermal) · replay organization

**🧊 Icebox (cut scope, kept for someday):** [The Forge](../docs/ideas/forge-mechanic.md) — the mod's namesake. A one-time-use "unstable forge" (unlocked by Armory/Fusion Core) that rolls wild upgrades: meta-upgrades on already-researched abilities, player-wide buffs, a 2nd upgrade stacked on a unit, or a 4th-slot mega/hero unit (Brutalisk / Loki BC) — then explodes. Cut for v1 (**not** shipping for Season 1), but still ranks **above** everything below.

**🚫 Not planned (priority floor):** Support for additional races. Frequently asked about; explicitly not happening — huge effort, and it would erode Wildcard Arena's core advantage: **per-unit balance instead of per-matchup** (too strong a unit → just nerf that unit; no need to keep three matchups simultaneously balanced like standard SC2). Ranks below even the icebox Forge.

---

## 🏁 Done
- [WA-006](./WA-006-update-mod-description.md) — Rewrote the in-client mod description (`DocInfo/DescLong`): hook-first, roll-and-adapt pitch, "not chaos" reassurance, Discord link; dropped the stale "PICK TERRAN (P/Z don't work)" line · 2026-07-16 (PR)
- [WA-023](./WA-023-chitinous-plating-researchable.md) — Ultralisk nerf: Chitinous Plating no longer free — now a rolled count-upgrade at the Armory (slot 3, +2 armor). Full recipe wired (galaxy/AbilData/ButtonData/GameStrings) · 2026-07-16
- [WA-027](./WA-027-missilepods-tier.md) — MissilePods → low-tier only (off Barracks s4 + Starport s3); Light bonus removed → flat 60 to all air (explicit `Light=0` so base +5 doesn't bleed through). Anti-air burst, 10 missiles × 6 · 2026-07-16
- [WA-025](./WA-025-transfuse-eligibility.md) — Transfuse eligibility: pulled off Starport s1 fliers (`NoneOf starport1`); now rolls on Sentry + Medic only. (Bio-target-gate question left for a future ticket) · 2026-07-16
- [WA-032](./WA-032-upgrade-pool-bugs.md) — Upgrade-pool bug fixes: D8Charge tag-case (`Thor`→`ThorAP`, `Warhound`→`WarHound`) so it reaches Thor/WarHound; Battlecruiser no longer rolls duplicate Yamato. Raven/Seeker + BC/Hyperjump verified as intended · 2026-07-16
- [WA-042](./WA-042-queen-build-time.md) — Barracks slot-2 retune: Queen build 40→32s + supply 2→3 (price kept 175/50), Marine build 17→15s. None of the three now especially cost/supply-efficient · 2026-07-16
- [WA-040](./WA-040-corrosive-bile-eligibility.md) — Corrosive Bile eligibility narrowed: `NoneOf` Marine/Hellion/Vulture (too cheap), Immortal/SiegeTank/Lurker (useless), Archon/VoidRay (too pricey); eligible span now Marauder→DuskWing. Doc regenerated · 2026-07-15
- [WA-041](./WA-041-duskwing-gate-cloak.md) — DuskWing cloak gated (nerf: now a roll, not free) + cloak-button Show/Use gating (hidden → grayed-while-researching → usable) on DuskWing & Wraith · 2026-07-15
- [WA-020](./WA-020-decide-cloak-behavior.md) — Cloak as a rollable upgrade: Wraith (cost→100/100, no start-cloak) + Ghost (card restored to stock) done; DuskWing gating → WA-041 · 2026-07-15
- [WA-031](./WA-031-range-indicator-not-updating.md) — Range indicator fix: Tempest/SiegeTank Range → catalog count upgrades (`TempestRange`/`SiegeTankRange`, +2.5 to all their weapons); ring now tracks the upgrade · 2026-07-14
- [WA-037](./WA-037-caster-native-abilities-on-g.md) — Caster natives reverted off G to stock defaults (ForceField→F, BlindingCloud→B, Transfusion→T, FungalGrowth→F); F_FungalGrowth verified still G · 2026-07-14
- [WA-033](./WA-033-hotkey-collisions-fix.md) — Hotkey collision sweep (in-game, Standard profile): addon builds→X; Factory Stalker→F/Lurker→R/Archon→A; Starport Phoenix→E/VoidRay→G/DuskWing→F/Corsair→E. Overseer split → WA-036 · 2026-07-14
- [WA-003](./WA-003-verify-unit-costs.md) — Cost audit → `docs/audits/unit-costs.md` (7 flagged: Zergling/HighTemplar/Goliath standouts) · 2026-07-13
- [WA-004](./WA-004-verify-upgrade-pools.md) — Upgrade matrix → `docs/audits/upgrade-matrix.md` (found bugs → WA-032) · 2026-07-13
- [WA-005](./WA-005-verify-hotkey-collisions.md) — Hotkey audit → `docs/audits/hotkey-collisions.md` (found collisions → WA-033) · 2026-07-13
- [WA-015](./WA-015-blink-changes-form.md) — Blink on burrowed Lurker / sieged Tank teleports then auto-unburrows/unsieges (`onBlinkUsed` + editor trigger) · 2026-07-13
- [WA-018](./WA-018-hybrid-caster-upgrade-pool.md) — Hybrid caster/fighter upgrade mechanism (`caster`/`pureCaster` split); Ghost/Phoenix/Corsair/Wraith + Queen hybrids · 2026-07-13
- [WA-029](./WA-029-archon-price.md) — Archon re-priced to 225/150 (was 150/250) · 2026-07-13
- [WA-026](./WA-026-stim-indicator-missing.md) — Stim flash now fires for `F_Stimpack` (any rolled-stim unit), via `StimpackStartImpact` actor override · 2026-07-12
- [WA-019](./WA-019-add-queen-barracks-slot2.md) — Queen added to Barracks slot 2 (150/50, speed 1.9, hybrid, Transfusion kept) · 2026-07-12
- [WA-028](./WA-028-queen-hide-zerg-abilities.md) — Queen: removed Burrow / Spawn Larva / Creep Tumor from card (kept Transfusion) · 2026-07-12
- [WA-016](./WA-016-marine-cost-and-combat-shield.md) — Marine 50/25 + starts with Combat Shield (`ShieldWall`) + tooltip · 2026-07-11
- [WA-017](./WA-017-hydralisk-cost-and-grooved-spines.md) — Hydralisk 100/50 + starts with Grooved Spines (`EvolveGroovedSpines`) + tooltip · 2026-07-11
- [WA-022](./WA-022-test-map.md) — Dev-mode test setup: `devMode` spawns all 6 buildings + 50k/50k (structures-only; unit-dump descoped) · 2026-07-11
- [WA-021](./WA-021-extract-base-game-catalogs.md) — Extracted all 6 base deps + trigger libs + UI into `reference/` (Claude now has stock-data lookup) · 2026-07-11
