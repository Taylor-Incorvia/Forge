---
id: WA-097
status: done
size: S
phase: 2-post-launch
priority: 45
---
# Remove the Vulture's reaper grenade (KD8 Charge) — hide via unsatisfiable requirement

## What
Stop the Vulture from having KD8 Charge (reaper grenade). It's an unexpected non-standard ability Taylor added, and it's illegible ("what just blew up my workers?"). Also part of rebalancing Factory slot 1 (Vulture is already strong without it).

## Why hide, not delete
KD8 Charge is a **static command-card button** on the Vulture's CUnit (`UnitData.xml`: `AbilArray index="3" Link="KD8Charge"` + `LayoutButtons index="6" Face="KD8Charge" ... Requirements=""`), NOT a rolled upgrade (the `addAbilityToUpgrade("KD8Charge",...)` is commented out). **Deleting a static card button poisons the editor command-card cache** (the stalker-blink/PF trap — see [[reference-command-card-button-type]]). So gate it with an always-false requirement instead; the button stays in the card but never shows and can't be used.

## Done — dedicated never-true requirement (decoupled from PF)
Mirrored the PFLockNever pattern with dedicated IDs so it does NOT share PF's requirement (reusing PFLockNeverReq would re-show the grenade if PF is ever un-hidden):
- `UpgradeData.xml`: dummy `CUpgrade id="KD8HideNever"` (never granted).
- `RequirementNodeData.xml`: `CountUpgradeKD8HideNever` (Count Link=KD8HideNever, CompleteOnly) + `GTECountUpgradeKD8HideNever1` (>= 1, always false).
- `RequirementData.xml`: `CRequirement id="KD8HideNeverReq"` with NodeArray Show **and** Use → the GTE.
- `UnitData.xml`: KD8Charge button `Requirements=""` → `Requirements="KD8HideNeverReq"`. AbilArray left intact (ability present but inaccessible).

Shipped via PR (not main). To bring the grenade back later: clear the button's Requirements.
