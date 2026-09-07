---
id: WA-116
status: todo
size: S
phase: 2-post-launch
priority: 1
---
# Try the documented core-facility Cancel index fix FIRST (cheap escape-key gate)

Part of [[WA-108]], but sequenced BEFORE the big WoL work. My (Ember's) recommended addition to the epic.

## Why this exists
[[WA-076]]'s strongest, best-documented lead (2026-08-21 addendum) is NOT WoL content — it's that the
mod's **Barracks/Factory/Starport** command cards drop an add-on button and **renumber their
`LayoutButtons` indices**, so the Cancel/CancelBuilding overrides land on the wrong base entries →
duplicate Cancel commands → the binding collides when Standard re-resolves on an in-game profile switch.
Those are **core facilities that survive a full WoL removal.** So this small fix could resolve the escape
bug for ~1% of the WoL epic's effort — and if it works, the escape-bug justification for the whole epic
disappears (the WoL removal then proceeds purely for its own value: clean dep + Season-2 content).

WA-061 already proved this exact defect class in this repo (commit `ccf45cf`: a new card index inherited
`Type=Undefined`). WA-076 and WA-061 look like the same bug in different clothes.

## Do (from WA-076's "Suggested test", cheapest first)
1. **Barracks card only:** align the mod's `Cancel` / `CancelBuilding` `LayoutButtons` with the *base*
   indices (9/10/11, not the shifted 7/8/9) and restore `Type="AbilCmd"` and `Row="2"` on them.
2. Publish to the test mod. Enter on a custom profile → switch to Standard in-game → test Escape.
3. If Barracks alone changes it, repeat for Factory and Starport.
4. Faster smoke test first: if the Barracks card renders a Cancel button in the wrong slot (Row 0 instead
   of Row 2) while an add-on builds, that alone confirms the index shift without the profile switch.

## Regression check
Tech Lab still builds; add-on Halt still works; `CancelLast`/que5 on a queued unit works; deselect and
menu-Escape unaffected; the other cancels (CancelBuilding) intact.

## Acceptance
- [ ] On a PUBLISHED build: custom → Standard in-game, Cancel stays bound to Escape.
- [ ] No regression to the cancels above.
- [ ] Result recorded in [[WA-076]] + [[WA-108]] (fixed → deprioritize the escape motivation for the epic;
      not fixed → the cause really is elsewhere, proceed knowing WoL removal may not fix it).

## Notes
VERIFY ON PUBLISHED BUILDS ONLY (the editor Test Document faked hotkeys — see [[WA-076]]).

## Do the isolation tests first (2026-09-07)
Before hand-fixing indices, run the prod-anchored isolation plan in [[WA-076]] ("Investigation plan").
Test 1 (bare dep) + Test 2 (strip facility CardLayouts) tell you whether this WA-116 fix is even the right
lever, or whether the cause is the dependency / elsewhere. Editor tests don't count -- published only.
