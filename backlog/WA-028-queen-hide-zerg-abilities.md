---
id: WA-028
status: done
size: M
phase: 1-game-readiness
priority: 24
---
# Hide Queen's burrow / spawn larva / creep tumor (editor-safe, for production)

## ✅ DONE (2026-07-12)
User removed Burrow / Spawn Larva / Creep Tumor from the Queen's card manually in the SC2 editor's data UI — and it worked locally. Consistent with the finding that card *removals* are in the locally-testable class (vs. added/unlocked ability buttons, which are production-only). Transfusion retained.

The Queen currently shows her full stock command card — including **Burrow**, **Spawn Larva**, and **Build Creep Tumor** — because we had to remove the card-strip to get Transfusion working. Fine for testing; not for production. Remove those three the right way.

## What actually went wrong in WA-019 (corrected)
`removed="1"` is NOT broken — the mod uses it ~30 times across `UnitData.xml` and it works. The Queen's Transfusion loss was almost certainly **wrong indices**: the card indices (5/6/8) were read from Liberty's *raw* Queen card, but the engine's *merged* card (Liberty + base-unit inheritance) has a different button order, so the removals hit Transfusion instead of the intended buttons. (The earlier `AbilArray` index removals compounded it.)

So the fix is to use the **correct merged-card indices**, not to avoid `removed="1"`. Determine the real indices from the editor's card view for the Queen (or test index sets fresh), remove exactly Burrow / Spawn Larva / Creep Tumor, and confirm Transfusion stays.

## Options to try (editor-safe)
- Define the Queen's `CardLayouts` **explicitly** with only the buttons we want (full set, no `removed` markers), if that overrides cleanly instead of merging.
- Or a data approach that doesn't touch the card array by index.
- Burrow is the priority: it actually works (morphs to `QueenBurrowed`, an alt-form) — leaving it means a hidden alt-form. Creep tumor / spawn larva are inert (no creep, no hatchery) so they're cosmetically ugly but harmless.

## Acceptance criteria
- [ ] Burrow, Spawn Larva, Build Creep Tumor gone from the Queen's card in production.
- [ ] Transfusion still present.
- [ ] Ideally verifiable locally (find a method the editor doesn't mangle) — else confirm on production.

## Notes
Current Queen override (`UnitData.xml`) is deliberately minimal (Speed 1.9 + gas only) with a comment explaining why. Stock card order: Move/Stop/Hold/Patrol/Attack (0-4), SpawnLarva (5), CreepTumor (6), Transfusion (7), BurrowDown (8).
