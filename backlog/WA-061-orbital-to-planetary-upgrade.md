---
id: WA-061
status: todo
size: M
phase: 1-game-readiness
priority: 40
---
# Orbital Command → Planetary Fortress upgrade — a self-limiting anti-drop static defense

## ⚠️ REOPENED 2026-08-20 → 🔧 ROOT CAUSE FOUND & FIXED 2026-08-21 (pending in-game verify)
**Real symptom (user-clarified):** the "Upgrade to Planetary Fortress" button never appeared on the Orbital command card *at all* — NOT a morph-execution failure; the button was simply invisible. (My earlier "engine morph-chain" theory was wrong — I'd assumed the button showed and the morph failed.)

**Root cause:** the morph `LayoutButtons` was added at **card index 8 — a NEW index the base Orbital card (indices 0–7, all `Type="AbilCmd"`) doesn't have.** Overriding an *existing* index merges unspecified fields (incl. `Type`) from the base button; a *new* index inherits nothing, so `Type` defaulted to `Undefined` → the button renders as nothing. Position was never the issue (Row1/Col0 is free; only Rally sits in Row 1). **This is the general gotcha behind "my command-card button won't show": a new `LayoutButtons` index must set `Type="AbilCmd"` explicitly.**

**Fix:** added `Type="AbilCmd"` to the index-8 LayoutButtons (committed to main). One attribute.

**Verify on next Test Document** (data card buttons DO render locally): (1) the `P` Planetary button now appears on the Orbital card, and (2) clicking it actually morphs to a Planetary. If the button shows but the morph won't execute, that's a separate follow-up — but the ability def (`CAbilMorph`, target PlanetaryFortress, `HasNoCargo`) is complete, so it most likely just works now. Flip status to `done` once both are confirmed.

**Design note (keep):** CC auto-morphs to Orbital for free on completion → no plain-CC state exists, so Orbital→Planetary is the only path (a CC→Planetary branch is impossible). Keep it a command-card button — a standalone SCV-built Planetary is explicitly NOT wanted (it breaks the "feels like normal multiplayer, no muscle-memory change" goal). The [[WA-081]] `Food=15` on PlanetaryFortress is correct and ready.

## ✅ ~~RESOLVED~~ (PR #22, merged 2026-08-12) — superseded by the reopen above
Shipped: `MorphOrbitalToPlanetary` ability + Orbital command-card button (hotkey **P**, Row 1 / Col 0), Planetary Fortress at **750 minerals**. Hotkey P verified free mod-wide and the card slot verified clean (no shadowed buttons) before merge.

## STATUS (2026-07-27) — implemented on a branch, UNMERGED, blocked on prod verification
Full data implementation is done on branch **`wa-061-planetary-fortress`** / **PR #22** (not merged). Do NOT assume it works — the morph itself has never been verified in-game.

**What's implemented (5 files, data-only, no galaxy):**
- `AbilData.xml` — new `MorphOrbitalToPlanetary` (`CAbilMorph`, `InfoArray Unit="PlanetaryFortress"`, `Requirements=""`, cloned field-for-field from stock `UpgradeToPlanetaryFortress` found in `reference/mods/liberty.sc2mod`). Plus the footgun fix: `UpgradeToOrbital`'s `AutoCastValidatorArray` swapped from `IsCasterCommandCenterOrPlanetaryFortress` → `IsCasterCommandCenter` (CC-only) so a finished Planetary won't auto-revert to Orbital.
- `UnitData.xml` — `OrbitalCommand` gets `AbilArray Link="MorphOrbitalToPlanetary"` + a card button; new `PlanetaryFortress` cost override (750 minerals).
- `ButtonData.xml` — `MorphOrbitalToPlanetary` button (planetaryfortress icon, hotkey **P**).
- `GameStrings.txt` / `GameHotkeys.txt` — name/tooltip + hotkey P.

**Where it's stuck: the button does not render in the local editor Test Document.**
- First placement was `Column="2"`, which collided with the stock merged Orbital card's Scanner Sweep (bottom row: MULE col0 / SupplyDrop col1 / **ScannerSweep col2** / LiftOff col3). Moved to an empty cell **`Row="1" Column="0"`** (commit `ce86d05`) — still not showing locally.
- **Leading theory:** this is the known command-card caching / "publish-to-verify" gotcha (see CLAUDE.md dev-testing note + [[WA-045]]). Command-card buttons for added abilities/morphs frequently DON'T render in the local Test Document but DO on a published build; the editor also caches card state badly after one test. So "not showing locally" is *expected* and not proof the placement is wrong.
- Secondary theory (less likely): the morph ability is being culled because `PlanetaryFortress` isn't resolving in the merged catalog — would need the editor merged-view to rule out.

**To resume:** publish the branch to Battle.net and check the Orbital card in-game. Then verify, in order:
1. **Button appears** on the Orbital card (Row 1 Col 0, hotkey P). If it does, the local no-show was just the caching gotcha.
2. **Morph works** — press it → Orbital becomes a functioning Planetary (cannon fires, high armor, can't lift).
3. **Footgun holds** — the finished Planetary STAYS a Planetary (no auto-revert), AND fresh Command Centers still auto-upgrade to Orbital.
4. **Cost** — confirm whether 750 charges the full amount or the difference (750−400), then tune. This is the main balance knob.

Confidence: medium. Footgun fix is high-confidence (uses the confirmed stock CC-only validator); the morph mechanics are the real risk (built without the editor merged-view).



## Why
Drops are hard to answer without static defense — I lost units to a Medivac shuttling Ultras/etc. around my bases. But I **hate** cheap spammable static defense (turrets/cannons/spines): it costs **no supply**, so a supply-capped player with spare minerals just carpets the base with it. Planetary Fortress is the exception worth allowing:
- It can be priced **expensive enough** that people don't carpet with it.
- Its **footprint is large** and it sits on the resource node — you physically can't fit many at one base. In real games it's **rare to see >2 Planetaries per base**, which is the self-limiting property I want.

So: let a player upgrade their **Orbital Command → Planetary Fortress** as the deliberate, costly, footprint-limited answer to drops. **Not required before the first prod push.**

## Current state (investigated)
- The mod **auto-upgrades every Command Center → Orbital Command** at game start. `UpgradeToOrbital` (`AbilData.xml:2210`) is a `CAbilMorph` with `<Flags index="AutoCast" value="1"/>` + `AutoCastValidatorArray = IsCasterCommandCenterOrPlanetaryFortress`, `AutoCastRange 1000`. So the base you actually play is an **Orbital**, not a CC — the upgrade path has to hang off the Orbital, not the CC.
- `OrbitalCommand` CUnit (`UnitData.xml:391`): costs 400 minerals; command card index 6 = `SupplyDrop` (Column 1), index 7 = `ChronoBoostEnergyCost` (Column 0). Those are the two bottom-row buttons currently occupied.
- The mod has **no `PlanetaryFortress` CUnit override and no `UpgradeToPlanetaryFortress` morph** of its own. Both exist in the **stock multiplayer dependency** (this mod is built on Void multiplayer — Ravager/Lurker/etc. confirm it), so the ids `PlanetaryFortress` and `UpgradeToPlanetaryFortress` are available to reference/override. (They're absent from `reference/` because that folder only carries the *liberty campaign* extract, which has no Planetary — expected, not a blocker.)

## ⚠️ Critical gotcha — the auto-Orbital morph will eat the Planetary
`UpgradeToOrbital`'s autocast validator is `IsCasterCommandCenterOrPlanetaryFortress` — it **fires on Planetary Fortresses too**. With autocast + range 1000, the instant you finish a Planetary it would **auto-morph straight back to an Orbital**, making Planetary impossible to keep.
**Fix:** change `UpgradeToOrbital`'s `AutoCastValidatorArray` to a **CommandCenter-only** validator (e.g. `IsCasterCommandCenter`) so it only auto-upgrades fresh CCs and leaves Planetaries alone. Verify the exact stock validator id in the editor merged view before committing.

## Fix approach (data-only, no galaxy expected)
1. **Bring the morph onto the Orbital's card.** Reference the stock `UpgradeToPlanetaryFortress` morph (or clone it to a mod id) and add its button to `OrbitalCommand`'s `CardLayouts` at a **free slot** — indices 6 and 7 are taken (SupplyDrop / ChronoBoost); the Planetary morph button needs its own cell that doesn't collide (candidate: index 8 / a free bottom-row column). Confirm the hotkey doesn't clash with SupplyDrop or ChronoBoost.
2. **Let an Orbital be a valid caster.** Stock `UpgradeToPlanetaryFortress` expects a `CommandCenter` caster. Point its caster validator at `OrbitalCommand` (or drop the caster-type gate) so the morph is legal from an Orbital. This is the crux — the whole feature is "Orbital → Planetary," which stock SC2 does not allow (stock is CC → *either* Orbital *or* Planetary).
3. **Price it high** to discourage spam — override `PlanetaryFortress` `CostResource` (stock is 150/150 morph cost on top of the CC). Pick a deliberately steep number; the point is that it's a considered investment, not a carpet. (Open: exact price — TBD by playtest. Start high, e.g. 550+ minerals total, and tune.)
4. **Drop the stock tech requirement.** Stock `UpgradeToPlanetaryFortress` requires an **Engineering Bay**. This mod doesn't put Eng Bays in the player's hands, so remove/replace that requirement (`Requirements=""`) or the button will never light up. Verify in merged view what the stock requirement id is.
5. **Keep the Planetary's stock combat identity** (cannon weapon, high armor/HP, no lift-off) — that's exactly the static-defense value. Only override cost + caster/requirement gating; leave weapon/armor stock unless playtest says otherwise.

## Open questions / decide during build
1. **Exact price** — needs playtest. Must be high enough to be self-limiting but not so high nobody builds it. Tune against the 8-worker / 400-CC economy.
2. **Does morphing Orbital → Planetary correctly consume the Orbital** (energy/abilities lost) and leave a normal Planetary, or does the merge leave stray Orbital abilities on the card? Check the resulting card in-game.
3. **Reversibility** — should a Planetary be able to go back to Orbital manually (lift-off is gone once Planetary)? Stock says no; probably leave it one-way. But confirm the `UpgradeToOrbital` validator change (gotcha above) didn't accidentally strand a player who wants to convert back.
4. **Footprint / placement** — Planetary is 3x3 like the CC/Orbital, so it should drop in place with no re-placement. Confirm no placement error on morph.
5. **AI / race-replacement interaction** — non-Terran pickers get an Orbital via the swap path too; make sure the Planetary morph is available to them identically (it hangs off Orbital, so it should be automatic).

## Acceptance criteria
- [ ] An Orbital Command has a **Planetary Fortress** button on its command card (no hotkey/layout collision with SupplyDrop / ChronoBoost).
- [ ] Pressing it morphs the Orbital → a functioning Planetary Fortress (cannon fires, high armor, can't lift).
- [ ] The Planetary **stays** a Planetary (the auto-Orbital morph no longer reverts it).
- [ ] Cost is the deliberately-steep tuned value; fresh CCs still auto-upgrade to Orbital as before.
- [ ] Terran and non-Terran picks both get the upgrade path.

## Notes
Design intent: Planetary is the *only* static defense I want to enable, precisely because expense + footprint make it self-limiting (rarely >2 per base) — unlike supply-free spammable turrets/cannons/spines. Data-only if it holds together; the caster-validator swap on `UpgradeToOrbital` is the one change with a real footgun (see gotcha). Sibling to the base-structure setup; no galaxy expected. Related: [[WA-051]] (400-mineral CC — same base-economy tuning surface).
