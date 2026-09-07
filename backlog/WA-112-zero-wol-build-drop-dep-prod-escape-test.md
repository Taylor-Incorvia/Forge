---
id: WA-112
status: todo
size: L
phase: 2-post-launch
priority: 2
---
# Zero-WoL-reference build: strip all WoL content, drop the dep, test escape on prod

Part of [[WA-108]]. Item 7. This is the removal *experiment* — the "does the bug vanish with the content
actually gone?" test that the earlier `wa-078` branch did NOT run (it kept the units, re-sourced).

## Do
1. On a dedicated branch, remove **all** references to WoL units + upgrades (use the WA-109/110 toggles
   to zero the pools, then also strip the command-card DATA / CUnit / CAbil / button entries — the toggles
   alone don't remove data, and the escape bug is about data presence).
2. Manually remove the Liberty (Campaign) dependency (Taylor does this in the editor).
3. Publish to the **private test mod** (dev mode on) and test the escape-key repro:
   enter on a custom profile → switch to Standard **in-game** → does Escape still cancel?

## Expectations (set correctly up front)
- **This build will NOT be functional** — the Starport Tech Reactor requires a WoL reference, so slot-3
  starport production breaks. That's fine; [[WA-113]] restores it. The escape test doesn't need a working
  Tech Reactor.
- **The bug may well persist.** Per [[WA-076]]'s strongest lead, the likely culprit is the core
  Barracks/Factory/Starport Cancel index shift — core cards that survive WoL removal. If [[WA-116]] wasn't
  done first and the bug persists here, that's the signal the cause is core-card, not WoL. Either outcome
  is informative:
  - Bug **gone** → WoL content (some card in it) was a contributor → proceed to WA-114 to pinpoint which.
  - Bug **persists** → cause is elsewhere (core cards); pivot escape-bug work to WA-116/WA-076 and keep
    the WoL removal for its own value (clean dep, Season-2 content).

## Acceptance
- [ ] A branch build with zero WoL references + no Liberty (Campaign) dep, published to the test mod.
- [ ] Escape repro run on that PUBLISHED build; result recorded here (gone / persists).
- [ ] Decision recorded: continue to WA-114 (bug moved) or redirect escape work (bug stayed).

## Notes
VERIFY ON PUBLISHED BUILDS ONLY — the editor Test Document faked hotkeys for days (see [[WA-076]]).
