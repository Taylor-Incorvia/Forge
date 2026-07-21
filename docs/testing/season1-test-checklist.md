# Season 1 — big test-round checklist

Covers the open PRs about to be merged **and** merged-but-unverified work. Legend: **⚠️** = most likely to have an issue, worth extra attention. **📦** = can only be confirmed on a **published** build (added-ability command-card buttons don't render in the local Test Document). Jot results inline.

Snapshot 2026-07-21. Regenerate as PRs land / new work merges.

---

## A. Dev-setup sanity (do first — enables everything else)
_My recent devMode changes; verify before trusting the rest of the run._
- [ ] ⚠️ Add-ons attach — Barracks→Tech Lab, Factory→Reactor, Starport→Tech Reactor, and **top-slot units are buildable**. (If a top slot won't build → Ember switches to the tech-tree-cheat bypass.)
- [ ] +200 supply holds (no depot-building needed).
- [ ] Production buildings spawn **on land** near the CC (not the sea).

## B. Economy
- [ ] Command Center costs **400 minerals** (WA-051, PR #15).

## C. Roll caps & pool health (WA-049 / WA-050 / WA-052 / WA-058)
- [ ] No **blank** upgrade slots, ever.
- [ ] ⚠️ Archon / Void Ray / Sentry never **stranded** (tightest pools).
- [ ] Blink can land on up to **2** units, not 3+ (WA-052).
- [ ] Corrosive Bile capped at **1**. Yamato capped at **1**. Each caster spell capped at **1**.
- [ ] Concussive (whole family) capped at **1** — only one of your units gets any concussive.
- [ ] Speed / Range / Stimpack cap at 2.

## D. Concussive slow (WA-034 — 14 units)
- [ ] Baseline: ~**2s at 70%** (target crawls at 30% speed); **no debuff icon** on the target.
- [ ] **Massive units immune** across the board (Thor, Ultralisk, Colossus, BC, Carrier…).
- [ ] Single-target slow works: Marauder✓, Void Ray✓, Vulture, Zealot, Zergling, Diamondback, Wraith (air + ground), Sentry.
- [ ] ⚠️ Splash/line/cleave/bounce slow **ALL affected targets** (the risky set):
  - [ ] Archon (splash — custom-authored search)
  - [ ] Ultralisk (cleave — search may be inactive)
  - [ ] Mutalisk (3-hop bounce)
  - [ ] Colossus (line)
  - [ ] Hellion (cone)
  - [ ] Firebat (cone, WA-053 PR #14)

## E. New per-unit upgrades (the 4 upgrade PRs)
- [ ] Goliath range (WA-055 #11) — Factory s2; **+3.5 air / +1.5 ground**; range indicator + real range grow on both weapons.
- [ ] ⚠️📦 Phoenix Anion Pulse-Crystals (WA-057 #12) — Starport s1; +2.5 range; **beam turns purple**; generic Range no longer offered to Phoenix.
- [ ] ⚠️ Liberator Smart Servos (WA-054 #13) — Starport s2; **+50% move speed** AND **faster Defender-Mode transform**; **exactly one** research button (watch for a stray duplicate "Speed" button); transform actually speeds up (Unit-keyed `InfoArray` reference works).
- [ ] Firebat concussive (WA-053 #14) — Barracks s3; slows the flame cone; no Hellion interference.

## F. Never worked locally — confirm on the published/prod build 📦
- [ ] Nuke (WA-024) — Ghost Academy builds it; Ghost arms silo → calldown → detonate; **nuke button renders on the Ghost's card**.
- [ ] Stalker blink range (WA-014) — roll Blink onto a Stalker; range + indicator grow.
- [ ] Tempest (WA-038) — the dead "Tectonic Destabilizers" button is gone from its card.

## G. Rolled caster/ability buttons render 📦
- [ ] When a unit rolls an **added ability** (F_Blink, ForceField, Fungal, Seeker, Yamato, etc.), the button **shows on the published build** (doesn't render in the local Test Document).

---

**Highest-attention:** the concussive splash trio (Archon / Ultralisk / Mutalisk), the Phoenix purple beam, the Liberator single-vs-double button, and the nuke.
