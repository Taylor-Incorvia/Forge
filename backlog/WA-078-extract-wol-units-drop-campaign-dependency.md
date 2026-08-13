---
id: WA-078
status: in-progress
size: L
phase: 1-game-readiness
priority: 40
---
# Extract WoL-dependent units into our own data, drop the Liberty (Campaign) dependency

## ✅ SPIKE VALIDATED (Goliath, 2026-08-12) — recipe proven
The Goliath was extracted (simplified — no multi-lock, see WA-079) and **renders + fights on VoidMulti alone, no campaign dependency**, in a throwaway staging mod. This confirms the core unknown: **asset files (`.m3`, textures) resolve from the game's shared CASC regardless of dependencies** — only catalog *data* is dependency-gated. Recipe is now mechanical; repeat for the remaining 6.

### The proven recipe (per unit)
1. **Pull raw XML from the reference dump**, not the editor's Duplicate (which mis-repoints refs). Source: `reference/campaigns/liberty.sc2campaign/base.sc2data/gamedata/*.xml`. **Keep ORIGINAL ids** — the target mod drops the campaign dep, so ids are free and every reference resolves with zero re-pointing.
2. **Include campaign-only objects; exclude shared ones.** An id defined in voidmulti/void/swarm/liberty(mod)/core is shared — leave it referenced. Only copy ids that exist ONLY in the campaign dump. (Follow the unit's ref graph: CUnit → weapons → effects → actors → models → turret → attach method → light → mover → sounds → buttons.)
3. **Trim cross-unit tangles** (shared campaign behaviors drag in other units — strip Spartan/Vulture/Scout-type refs; simplify per WA-079).
4. **ADD the explicit `.m3` model path — this is the one non-obvious step.** Campaign unit `CModel`s have **no `<Model>` path** (they bind via base-game asset data that does NOT follow into a non-campaign mod). Add `<Model value="Assets\Units\<Race>\<Unit>\<Unit>.m3"/>` — the retail convention (confirmed working for Goliath). Verify per unit (portrait/death models may need theirs too).
5. **Split into `GameData/*.xml`** (UnitData, WeaponData, EffectData, ModelData, ActorData, TurretData, AttachMethodData, LightData, MoverData, SoundData, ButtonData), each `<?xml?>`+`<Catalog>`-wrapped. Auto-loaded by convention (no manifest needed).
6. **Render-test** in a throwaway VoidMulti-only staging mod. Zerg-isolated test hook (mod is Terran-only): add a `LarvaTrain` Train entry + a Larva card button in a FREE slot (bottom-right / Row2 Col4) — morph a Larva → the unit. Confirm it renders/animates/attacks.

### Per-unit Wildcard overrides to re-apply (the currently-live CUnit overrides that sit on top of the campaign unit)
When the campaign dep is dropped, the extracted data BECOMES the base unit — so Wildcard's existing per-unit tweaks must be folded in. Check each unit's current `<CUnit id="…">` override in Wildcard `UnitData.xml`. Two known ones for Goliath (verify per unit):
- **`<Collide index="ForceField" value="1"/>` — REQUIRED on every WoL unit.** Campaign units ship WITHOUT force-field collision, so they phase through Force Fields (bug). This is the "same fix for each unit" Taylor applied.
- **Cost tweaks** (e.g., Goliath gas 50→**75**). Fold in whatever the current override sets.

### Wildcard-integration gotcha: dangling refs to dropped weapons
The **simplified** extraction drops the `*Upgraded` multi-lock weapon variants (e.g. `GoliathAUpgraded`, `GoliathGUpgraded`), but Wildcard features may reference them — e.g. the **`GoliathRange` upgrade** (`UpgradeData.xml`) has `EffectArray` refs to `GoliathAUpgraded`/`GoliathGUpgraded`. After pasting the simplified unit + dropping the dep, those refs dangle. **Trim them** (keep only the base-weapon refs). Sweep each unit's Wildcard upgrades/abilities for refs to anything the simplified extraction dropped.

### Remaining
- [x] Goliath (spike)
- [ ] 6 more WoL units (agent-assisted now that the pattern's proven; Ember reviews the XML)
- [ ] Paste all 7 into Wildcard Arena, then **drop the Liberty (Campaign) dependency** → resolves WA-076 (Cancel hotkey) as a side effect.

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
