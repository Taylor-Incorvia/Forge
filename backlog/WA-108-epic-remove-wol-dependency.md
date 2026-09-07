---
id: WA-108
status: todo
size: XL
phase: 2-post-launch
priority: 2
---
# EPIC: Remove the WoL / Liberty-Campaign dependency (and use it to hunt the escape-key bug)

Parent tracker for the "get all Wings-of-Liberty content out of the dependency and into our own
data" effort. Gated on: post-Aug-31 changes test-deployed + Taylor in a quiet play/archive period.
Deliberate, messy, isolated-branch work — NOT interleaved with balance tweaks.

## ⚠️ READ THIS FIRST — the escape-key premise needs a caveat (from the evidence in [[WA-076]])
Taylor's driving hypothesis is "remove WoL → the escape-key bug goes away." Two facts from WA-076's
own elimination log complicate that, and should shape sequencing:

1. **Dropping the campaign *dependency* was already tested and did NOT fix it.** The `wa-078` branch
   extracted the WoL units into our data and dropped the dep; the bug **persisted** (WA-076 point 1).
   → The dependency *mechanism* is ruled out.
   **BUT:** that test *kept* the WoL units (re-sourced), so their command-card DATA was still present.
   Taylor's new plan **fully removes** the content — a genuinely different, stronger test. Worth doing.

2. **The strongest documented lead is NOT WoL content — it's the core facility cards.** The 2026-08-21
   addendum in WA-076 found the Barracks/Factory/Starport cards drop an add-on button and **renumber
   their `LayoutButtons` indices**, so Cancel/CancelBuilding overrides land on the wrong base entries →
   duplicate Cancel commands → collision when Standard re-resolves. Those are **core Terran facilities,
   not WoL units** — they'd survive a full WoL removal. If that lead is right, removing WoL won't fix it.

**Implication for sequencing:** do the **cheap** thing first ([[WA-116]] — the core-facility Cancel
index fix). It's a small change on a build you can publish; if it fixes the escape bug, the entire
"escape-bug" justification for this epic evaporates. Then run the WoL removal **on its own merits**
(drop a heavy dependency; unblock non-WoL Season-2 content) — which are strong regardless of the bug.

**Also:** the pool-exclusion toggles (WA-109/110) test *gameplay* ("is WoL content needed/fun in the
rolls"), NOT the escape bug — the bug is about command-card DATA being present, not about whether a
unit can roll. Don't expect the toggles to move the escape bug.

## The plan (Taylor's 12 items → tickets)
1. Fix `wa-082-cleanup-unused-references` merge conflict — ✅ DONE 2026-09-07 (see [[WA-082]]; resolved, pushed, NOT merged).
2. Remove unrollable WoL upgrade data (shockwave, disruption blast, HERC grapple, Odin barrage, …) → [[WA-111]].
3. `isWolDependent` unit tag → [[WA-109]].
4. Toggle to exclude WoL units from unit pools → [[WA-109]].
5. `isWolDependent` upgrade marking (clean approach — see WA-110) → [[WA-110]].
6. Toggle to exclude WoL upgrades from all upgrade pools → [[WA-110]].
7. Zero-WoL-reference build + manual dep drop + prod escape-key test → [[WA-112]].
8. Tech Reactor minimal re-implementation (7 hard requirements) → [[WA-113]].
9. Add WoL units/upgrades back one at a time, one commit each → [[WA-114]].
10. Bisect to find the culprit(s) → [[WA-114]].
11. Add more tickets based on what breaks → ongoing; file under this epic as found.
12. Add desired campaign units post-drop (Dragoon, etc.) → [[WA-115]].

Plus (my recommendation): [[WA-116]] — try the documented core-facility Cancel fix FIRST (cheap gate).

## Recommended sequencing
1. **[[WA-116]]** (cheap escape-bug test) — before anything big.
2. **[[WA-111]]** (delete unrollable WoL upgrade data) — pure cleanup, shrinks the surface.
3. **[[WA-109]] + [[WA-110]]** (tags + toggles) — gives clean single-switch removal for the play-side.
4. **[[WA-112]]** (zero-WoL build, drop dep, prod test) — the removal experiment. Non-functional build
   (no Tech Reactor) is expected and fine for the escape test.
5. **[[WA-113]]** (Tech Reactor minimal) — restores a functional build.
6. **[[WA-114]]** (add-back one-commit-each + bisect) — pinpoint the culprit(s).
7. **[[WA-115]]** (nice-to-have campaign units) — only after the dep is cleanly gone.

## Bisection note (technical)
`git bisect` assumes a **single** good→bad transition. Taylor expects **multiple** culprits — so classic
bisect will find only the *first* one. Better fits here:
- **Add-lines-only commits** (item 9) so a clean rebuild = cherry-pick the good ones. Taylor's own idea; keep it strict.
- For multiple causes: either (a) bisect → fix/skip the found culprit → bisect again, repeated; or
  (b) since each add-back is independently testable, just walk them linearly and record pass/fail per commit
  (a full O(n) sweep, but every result is unambiguous — no monotonicity assumption). With ~7 units + a
  handful of upgrades, the linear sweep is cheap and more honest than fighting bisect's single-transition model.

## Notes
Supersedes the stale "dependency IS the confirmed cause" framing in [[WA-078]]'s Why section (written
before the 2026-08-16 A/B that disproved it). [[WA-078]] remains the extraction *recipe* reference.
Related: [[WA-076]] (the bug), [[WA-082]] (dead-ref cleanup, done).
