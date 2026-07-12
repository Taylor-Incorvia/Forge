# Wildcard Arena — Board

Season 1. See [README](./README.md) for how we work. Say **"give me a ticket"** and I'll hand you the top `todo` item.

---

## 🔨 In progress
_(nothing yet)_

---

## ⚡ Force multipliers — do these first if you can
| ID | Ticket | Size | Phase |
|----|--------|------|-------|
| [WA-022](./WA-022-test-map.md) | Dev-mode test setup (triggers-only, no .sc2map; instant unit/upgrade testing) | M | 1 |

---

## ✅ Ready to grab (`todo`)
Groomed, scoped, has acceptance criteria. Pull from the top.

| ID | Ticket | Size | Phase |
|----|--------|------|-------|
| [WA-003](./WA-003-verify-unit-costs.md) | Verify unit costs | M | 1 |
| [WA-004](./WA-004-verify-upgrade-pools.md) | Verify upgrade pools | M | 1 |
| [WA-005](./WA-005-verify-hotkey-collisions.md) | Verify hotkey collisions | M | 1 |
| [WA-006](./WA-006-update-mod-description.md) | Update mod description | S | 1 |
| [WA-023](./WA-023-chitinous-plating-researchable.md) | Chitinous Plating researchable (Ultralisk nerf) | S | 1 |
| [WA-014](./WA-014-verify-stalker-blink-range.md) | Re-enable + test stalker blink range | S | 1 |
| [WA-015](./WA-015-blink-changes-form.md) | Blink teleports then changes form | M | 1 |

### Barracks Slot 2 rework (was WA-002) — do WA-018 before WA-019/WA-020
| ID | Ticket | Size | Phase |
|----|--------|------|-------|
| [WA-016](./WA-016-marine-cost-and-combat-shield.md) | Marine: 50/25 + Combat Shield | S | 1 |
| [WA-017](./WA-017-hydralisk-cost-and-grooved-spines.md) | Hydralisk: 100/50 + Grooved Spines | S | 1 |
| [WA-018](./WA-018-hybrid-caster-upgrade-pool.md) | ⭐ Hybrid units roll both upgrade pools | L | 1 |
| [WA-019](./WA-019-add-queen-barracks-slot2.md) | Add Queen to Barracks Slot 2 _(needs WA-018)_ | M | 1 |
| [WA-020](./WA-020-decide-cloak-behavior.md) | Decide Wraith/DuskWing cloak behavior _(needs WA-018)_ | S | 1 |

---

## 🧊 Needs grooming (`backlog`)
Real work, but not ready to grab — needs a decision or a problem statement first.

| ID | Ticket | Size | Phase | Blocked on |
|----|--------|------|-------|------------|
| [WA-001](./WA-001-arsenal-modal.md) | Arsenal / Roll Explanation modal (SPIKE first) | L | 1 | Run the spike → docs + split into build tickets |

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

---

## 🏁 Done
- [WA-021](./WA-021-extract-base-game-catalogs.md) — Extracted all 6 base deps + trigger libs + UI into `reference/` (Claude now has stock-data lookup) · 2026-07-11
