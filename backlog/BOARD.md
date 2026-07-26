# Wildcard Arena — Board

Season 1. See [README](./README.md) for how we work. Say **"give me a ticket"** and I'll hand you the top `todo` item.

---

## 🔨 In progress
- [WA-035](./WA-035-lifesteal-upgrade.md) — Lifesteal **slice 1 (Marine, 15%)** merged (PR #8). Slice 2+ **parked** (decided lifesteal is less fun than the other upgrades) — resume if desired.
- [WA-038](./WA-038-tempest-remove-tectonic-destabilizers.md) — Tectonic Destabilizers (`TempestGroundAttackUpgrade`) passive button hidden via a Show-node requirement override (index-independent). **Merged to main (PR #2)** — one open AC: eyeball the Tempest card in-game to confirm it's gone + nothing else disturbed.
- [WA-014](./WA-014-verify-stalker-blink-range.md) — stalker blink range re-enabled in galaxy (root cause: first push missed the `PlacementRange` line; data complete since b40c4e6). **Awaiting production test** — roll it onto a Stalker, confirm range grows.

---

## ✅ Ready to grab (`todo`)
Groomed, scoped, has acceptance criteria. **⭐ = current top priority** (editor caching → Planetary → the Faction modal). Pull from the top.

| ID | Ticket | Size | Phase |
|----|--------|------|-------|
| ⭐ [WA-045](./WA-045-editor-caching-settings.md) | Investigate if editor Documents-prefs settings fix the caching pain (stop the stale-data scares) | S | 1 |
| ⭐ [WA-061](./WA-061-orbital-to-planetary-upgrade.md) | Orbital → Planetary Fortress upgrade — costly, footprint-limited anti-drop static defense | M | 1 |
| ⭐ [WA-039](./WA-039-arsenal-paper-design.md) | "Your Faction" modal — paper design + info spec (design-first; epic WA-001; **may rename** from "Your Arsenal") | S | 1 |
| [WA-056](./WA-056-projectile-count-upgrades.md) | Projectile-count upgrades — Phoenix (scalar ✅) + Liberator (array append ⚠️); home of the Valkyrie-Liberator idea | M | 1 |
| [WA-044](./WA-044-transfuse-targeting.md) | Transfuse any unit — bio gate is a heal-effect validator; needs editor merged-view | S | 1 |
| [WA-060](./WA-060-auto-mine-after-race-replace.md) | Auto-mine after race replacement — workers gather at 0:00 regardless of picked race | M | 1 |
| [WA-063](./WA-063-ultralisk-concussive-no-slow.md) | Ultralisk concussive applies no slow (KaiserBlades set override not merging?) — pulled from pool | M | 1 |
| [WA-064](./WA-064-neural-parasite-tube-lingers.md) | Neural Parasite tube doesn't disappear after effect ends (cosmetic) | S | 1 |
| [WA-036](./WA-036-overseer-hotkey.md) | Overseer hotkey won't render (cursed F_Blink class) | S | 1 |
| [WA-053](./WA-053-firebat-stalker-concussive.md) | **Stalker** concussive (Firebat half shipped; Stalker deferred — weapon-id risk) | S | 1 |
| [WA-059](./WA-059-ember-github-identity.md) | Give Ember its own GitHub identity so you can review/approve its PRs (workflow) | S | 1 |
| [WA-047](./WA-047-gameplay-clips.md) | Play + capture gameplay clips (every Reddit post needs one) | M | 4 (parallel) |
| [WA-065](./WA-065-replay-stats-tool.md) | Replay stats/analysis tool (sc2reader) — winner, rolls-from-chat, unit-death map, upgrades-used (someday) | M | 4 |
| [WA-066](./WA-066-hellion-flame-wall-upgrade.md) | Hellion "flame wall" upgrade — blue flame + big visually-matched splash (replaces removed Twin-Linked; needs actor work) | M | 1 |

---

## 🧊 Needs grooming (`backlog`)
Real work, but not ready to grab — needs a decision or a problem statement first.

| ID | Ticket | Size | Phase | Blocked on |
|----|--------|------|-------|------------|
| [WA-001](./WA-001-arsenal-modal.md) | "Your Faction" modal (**EPIC**) — design-first plan captured; grab WA-039 to start (**may rename** from "Your Arsenal") | L | 1 | Build phases (2–7) groom after the design lands |

---

## 🌟 Nice to have (ticketed)
_Lowest priority. Ordered bottom-up toward "never" (other races — see icebox)._
| ID | Ticket | Size | Phase |
|----|--------|------|-------|
| [WA-030](./WA-030-ability-telegraph.md) | Ability telegraph — icon/name above caster (upgrade abilities) | M | 1 |
| [WA-046](./WA-046-ultralisk-chitinous-visual.md) | Ultralisk looks different when it has Chitinous Plating (readability) | M | 1 |
| [WA-067](./WA-067-sentry-force-field-size-upgrade.md) | Sentry force-field size upgrade — one giant wall (grow the small Sentry pool) · **post-Faction-modal** | S | 1 |
| [WA-069](./WA-069-sentry-guardian-shield-shockwave.md) | Sentry Guardian Shield casts a mini shockwave (enemy-only = a `SearchFilters` fix) · **post-Faction-modal** | M | 1 |
| [WA-068](./WA-068-ravager-barracks-slot3-bile-upgrades.md) | Ravager → Barracks s3 + Corrosive Bile buff upgrades (splash/damage) · **post-Faction-modal** | M | 1 |

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
- [WA-024](./WA-024-ghost-academy-nuke.md) — Nuke **verified on prod**: full arm→calldown→detonate loop + Ghost Academy card button all work · 2026-07-23
- Balance pass — Irradiate 25→40, Disruption Web low-tier only (off rax s4 + starport s3), Zealot ✗zerglingattackspeed, Armory 150/50→150/75; Missile Pods confirmed 75 on prod (the 125 was Battle.net catalog lag) · 2026-07-23
- [WA-051](./WA-051-prepatch-economy.md) — Command Center back to 400 minerals (prepatch economy override) · 2026-07-22 (PR #15)
- [WA-054](./WA-054-liberator-transform-speed-upgrade.md) — Liberator Smart Servos: +50% move speed + faster Defender-Mode transform (both baked into the CUpgrade; the `InfoArray[0]` fix); transformationservos icon · 2026-07-22 (PR)
- [WA-057](./WA-057-phoenix-anion-pulse-crystals.md) — Phoenix rolls `PhoenixRangeUpgrade` (+2.5, displayed "Anion Pulse-Crystals"); purple beam confirmed on prod · 2026-07-22 (PR)
- Concussive extended — Firebat (WA-053 Firebat half) + Hellbat (transformed Hellion keeps it); Ultralisk cleave pulled → [WA-063](./WA-063-ultralisk-concussive-no-slow.md) · 2026-07-22
- [WA-062](./WA-062-autoturret-cast-range.md) — `F_BuildAutoTurret` cast range 2→5 so ground units can actually place it (rolled `F_` only; Raven native untouched) · 2026-07-22
- [WA-055](./WA-055-goliath-range-upgrade.md) — Goliath range upgrade (`GoliathRange`, +3 air / +1 ground, replaces generic Range for Goliath); in the Range family · 2026-07-20 (PR)
- [WA-058](./WA-058-battlecruiser-roll-yamato.md) — Battlecruiser can roll Yamato (0 native → 1 from the upgrade; tested); Yamato capped at 1 like Corrosive Bile · 2026-07-20 (PR)
- [WA-052](./WA-052-adjust-upgrade-caps.md) — Blink roll cap 1→2 (Corrosive Bile / casters / Concussive family stay at 1) · 2026-07-20
- [WA-034](./WA-034-concussive-shells-upgrade.md) — Concussive slow-on-attack on **13 units** (Marauder + 12 custom incl. Sentry); shared 2s / 70% slow, massive-immune, splash/bounce/cleave slow all targets · 2026-07-20 (PR #7/#9)
- [WA-049](./WA-049-per-upgrade-roll-caps.md) — Per-upgrade roll caps: global cap-aware assignment (smallest-pool-first, per-family counts, graceful fallback); cap 1 for Blink/Bile/casters/Concussive, 2 otherwise · 2026-07-20 (PR #9)
- [WA-050](./WA-050-upgrade-families.md) — Upgrade families (`upgradeFamilyHelpers`): per-unit variants (all `Concussive*`, `Lifesteal*`, the split Range upgrades) count as one for caps · 2026-07-20 (PR #9)
- [WA-048](./WA-048-colossus-thermal-lance-tooltip.md) — Added `Button/Tooltip/Colossus` (mirrors Marine/Hydralisk free-upgrade tooltips): flags that it starts with Extended Thermal Lance (+2 range, 9) · 2026-07-16
- [WA-043](./WA-043-unit-stats-reference-doc.md) — New `docs/audits/unit-stats.md`: merged per-unit stats (cost/supply/build/HP/shields/armor/speed/weapons+DPS) for all ~34 units, `(MOD)` flags + override summary + game-speed caveat · 2026-07-16
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
