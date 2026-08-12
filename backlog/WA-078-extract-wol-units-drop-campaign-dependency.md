---
id: WA-078
status: todo
size: L
phase: 1-game-readiness
priority: 40
---
# Extract WoL-dependent units into our own data, drop the Liberty (Campaign) dependency

## Why
Wildcard Arena depends on **Liberty (Campaign)** for ~7 Wings-of-Liberty units. That dependency costs us three things:
1. **It breaks the Cancel/Escape hotkey (WA-076).** A campaign dependency forces a campaign hotkey context where Cancel ships **unbound** on the Standard profile. Confirmed root cause — can't be fixed while the dep is present, and can't remove the dep while units reference it. This project is the real fix for WA-076.
2. **It's a heavy dependency** we don't otherwise want.
3. **It blocks the pattern for adding non-WoL content later** (Season 2 ideas — Wraithwalker, the Colossus fire-trail upgrade, etc.). Proving this extraction is the proof-of-concept for pulling **any** unit from **any** source into our own data.

Good time to do it: content is frozen for Season 1, so the roster is stable.

## The approach (Taylor's — proven at small scale via the HotS "Leap Test" mod)
Do **NOT** make Wildcard Arena depend on a new mod. Instead use a staging mod as a known-good source, then copy the XML across:
1. **Staging mod.** Create a separate mod (like `Leap Test.SC2Mod`) that HAS the Liberty (Campaign) dependency.
2. **Duplicate with references.** In the editor, duplicate each target unit with "copy referenced entries" so the whole subtree (CUnit → weapons → abilities → behaviors → buttons → requirements → turrets → actors → models → effects) lands in the staging mod's **own** catalog, no longer referencing the dependency.
3. **Prove self-containment.** Swap the staging mod's dependency from Liberty (Campaign) to **VoidMulti**. If the units still work (art + abilities), the extracted data is self-contained.
4. **Copy the XML into Wildcard Arena.** Paste the resulting catalog XML from the staging mod's data files directly into Wildcard Arena's (`UnitData`, `WeaponData`, `AbilData`, `BehaviorData`, `ButtonData`, `ActorData`, `RequirementData`, `EffectData`, …). This is what worked for the leap ability.
5. **Drop the dependency.** Once all 7 units live in Wildcard Arena's data, remove the Liberty (Campaign) dependency → Cancel hotkey fixed.

## Roster (~7 units — confirm against the editor's dependency "Used By" report)
From the report, WoL-dependent units include: **Goliath, Diamondback, Predator, Science Vessel, Medic, Wraith, Firebat** (double-check Vulture, Lurker campaign effects, and Kelmorian Miner components). **The "Used By" report is the authoritative checklist** — every referenced entry must be copied, not just the CUnit.

## Workflow / sequencing
- **Start with Goliath as a spike** (heavy: weapons, turret, behaviors, and `GoliathRange` references it). The spike validates the whole theory:
  1. Model, textures, portrait, sounds render after dropping the dep.
  2. Abilities/upgrades work.
  3. **Cancel now binds to Escape** — confirms the WA-076 assumption we never got to test.
  Also: measure how long one unit takes → real estimate for the other six.
- Then one unit at a time. Once the pattern is proven on a few (manually, with AI help), a separate agent can likely do the rest with little intervention. Taylor reviews; **Ember reviews the final XML** and/or does the copy-paste into Wildcard Arena.

## Watch-outs
- **Keep original IDs.** Since we're dropping the dep, the original IDs (`Goliath`, `Diamondback`, …) free up — reuse them for the copies so every galaxy initializer / slot pool / upgrade reference keeps working untouched (no re-pointing).
- **Art resolution is the one real risk.** WoL unit models/textures live in the base game's shared CASC storage (everyone owns the campaign), so paths should resolve without the dep — but a campaign-only portrait or team-color variant could go missing. The Goliath spike is where we find out.
- **Completeness.** A missing referenced sub-entry (a weapon effect, a requirement, an actor) = a broken unit or a lingering campaign reference. Cross-check every unit against its "Used By" report.
- **XML house rules.** No `<!-- -->` comments in the mod's XML (editor mangles them; `--` breaks parsing). Watch catalog-merge semantics (overrides merge field-by-field).
- **Non-destructive.** Work on a branch; keep the staging mod as the reusable source of truth (like `leaptestcopy.zip`).

## Acceptance criteria
- [ ] Goliath spike: extracted into own data, dep-free in the staging mod, art + abilities intact, and (in a Wildcard build with the dep dropped) **Escape cancels**.
- [ ] All 7 units extracted, XML copied into Wildcard Arena, no remaining Liberty (Campaign) references.
- [ ] Liberty (Campaign) dependency removed from Wildcard Arena.
- [ ] Cancel/Escape works out of the box on Standard (closes WA-076).
- [ ] No unit visually or functionally regressed (spot-check each).

## Notes
Precedent: the HotS "Leap Test" mod (`C:\Program Files (x86)\StarCraft II\Mods\Leap Test.SC2Mod`) — same duplicate-then-swap-dependency pattern, proven for the leap ability. **Resolves WA-076.** Enables post-S1 non-WoL content (Season 2 units; see the standalone-game vision doc).
