---
id: WA-038
status: todo
size: S
phase: 1-game-readiness
priority: 46
---
# Remove the unresearchable "Tectonic Destabilizers" button from the Tempest card

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
- [ ] The Tectonic Destabilizers button no longer appears on the Tempest card; nothing else on the card is disturbed.
