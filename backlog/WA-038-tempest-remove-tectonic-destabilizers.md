---
id: WA-038
status: in-review
size: S
phase: 1-game-readiness
priority: 46
---
# Remove the unresearchable "Tectonic Destabilizers" button from the Tempest card

## 🔨 Fixed 2026-07-16 (PR, needs local visual confirm)
Identified the button: "Tectonic Destabilizers" = the **`TempestGroundAttackUpgrade`** passive button (confirmed via `Button/Name/TempestGroundAttackUpgrade=Tectonic Destabilizers` in voidmulti GameStrings).

**Fix chosen — requirement, not CardLayouts.** The stock `HaveTempestGroundAttackUpgrade` requirement (voidmulti) has only a `Use` node, so the passive button always *shows* (greyed) even though the arena has no Fleet Beacon path. Overrode it in the mod's `RequirementData.xml` to add a `Show` node = `CountUpgradeTempestGroundAttackUpgradeCompleteOnly788113278` (the upgrade never completes here → button hidden).

**Why not the CardLayouts index removal the ticket suggested:** the button is appended across three catalog layers (core → void → voidmulti) with no explicit index, so its final merged array index can't be computed reliably from static files — and getting it wrong risks nuking the wrong button + the docs-#5 sparse-array hazard. The requirement approach is index-independent, touches no CardLayouts, and reuses the exact Show/Use pattern from WA-041's cloak buttons.

**Needs:** quick local check — hover the Tempest card, confirm Tectonic Destabilizers is gone and Disruption Blast / move-stop-hold-attack all remain. (Locally testable; the rolled `F_TempestDisruptionBlast` on G is a separate button, unaffected.)

## Why
The Tempest's command card shows a **Tectonic Destabilizers** button, but there's no Fleet Beacon research path in the arena to enable it — so it's a dead/confusing button. Remove it.

## Findings
- The **Tempest is 100% stock in the mod** — there is **no `<CUnit id="Tempest">`** in `UnitData.xml`, and "Tectonic Destabilizers" appears **nowhere** in the mod's data. The button comes entirely from the base game.
- The stock Tempest card (reference `void.sc2mod`) carries a **`LightningBomb`** ability button (`Face="LightningBomb" AbilCmd="LightningBomb,Execute"`) — this is the likely culprit (the display name "Tectonic Destabilizers" / "Disruption Blast" maps to an internal id that isn't literally "Tectonic"). **Confirm the exact button in-game** (hover the one labeled Tectonic Destabilizers, note its ability) before removing.

## Fix
Add a minimal `<CUnit id="Tempest">` override with a `CardLayouts` entry that **removes just that button** (`<LayoutButtons index="N" removed="1"/>`), OR drop the ability from the Tempest's `AbilArray`. Keep it surgical.
- ⚠️ Per `docs/static-prototype-attempt-1.md` §5: edit `CardLayouts.LayoutButtons` in **XML only** — the editor re-normalizes/destroys sparse LayoutButtons arrays. Don't open the Tempest in the editor data UI after.

## Note
The rolled `F_TempestDisruptionBlast` (=G) is a separate upgrade button and is unaffected — this is only about the native unresearchable button on the base Tempest card.

## Acceptance
- [ ] The Tectonic Destabilizers button no longer appears on the Tempest card; nothing else on the card is disturbed. _(local visual confirm pending)_
