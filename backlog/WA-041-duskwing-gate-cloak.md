---
id: WA-041
status: in-progress
size: S
phase: 1-game-readiness
priority: 31
---
# Nerf DuskWing: make cloak a roll instead of free (gate its cloak ability)

## ✅ Implemented 2026-07-15 (pending in-game test)
Overrode `DuskWingBansheeCloakingField` in `AbilData.xml` — added `Requirements="UseCloakingField"` to the `On` command (re-declaring `DefaultButtonFace="CloakOnBanshee"` + `Flags ToSelection`, since CmdButtonArray overrides replace the whole entry per docs §2). Confirmed `UseCloakingField` counts the `BansheeCloak` upgrade, which the rolled `BansheeCloak` count upgrade grants. So a fresh DuskWing starts **un**cloaked; cloak unlocks only after rolling + researching it.
**Test:** build a DuskWing → confirm no cloak by default → roll + research `BansheeCloak` (Fusion Core s2) → confirm cloak now works. Also closes WA-020's dead-roll gap (the DuskWing `BansheeCloak` roll now does something).

## Why
The DuskWing is over-strong right now, and **free cloak is a big part of it**. It's been the deciding factor across multiple games; it's so obviously optimal that a brand-new player massed cloaked DuskWings unprompted, both games. The user sometimes refuses to use its cloak because it feels too strong. Making cloak a **roll** (not guaranteed) reins it in.

## Refinement (2026-07-15): cloak button Show/Use gating
The cloak-on button showed (grayed) on every DuskWing/Wraith even without cloak — bad clarity. Added Show+Use requirements so it's **hidden until cloak research is queued, grayed while researching, enabled when complete**:
- `Forge_BansheeCloakShowUse` / `Forge_WraithCloakShowUse` (RequirementData): Show = `QueuedOrBetter`, Use = `CompleteOnly`.
- Applied to `DuskWingBansheeCloakingField` + `WraithCloak` ability On commands (AbilData overrides).

**Gap vs the exact ideal** ("grayed the instant it's rolled"): "rolled" is stored as a galaxy DataTable string, not a data-checkable upgrade, so a `CRequirement` can't see it — it grays when research is *queued*, not rolled. The exact ideal would need a **marker upgrade granted on roll** (the reverted static-pre-declaration territory) → separate ticket if wanted.
**Alternative** for "fully hidden until researched, no grayed state": change the Show node `QueuedOrBetter → CompleteOnly`.

## Root cause (found during WA-020 testing)
The DuskWing is the **campaign** unit, using its own cloak ability **`DuskWingBansheeCloakingField`**, whose `On` command has **no research requirement** → free cloak. It does NOT use the MP `BansheeCloak` ability — which is why the `BansheeCloak` count upgrade (added in WA-020) currently does nothing for it.

## Approach
Gate `DuskWingBansheeCloakingField` so cloak needs the roll:
- Override `DuskWingBansheeCloakingField` in the mod's `AbilData.xml` to add a research requirement on its `On` command — e.g. `Requirements="UseCloakingField"` (verify in the reference that this is the requirement the stock `BansheeCloak` upgrade satisfies).
- The **existing** `BansheeCloak` count upgrade (already in the DuskWing's pool) then becomes the roll that unlocks cloak.
- Result: DuskWing starts **un**cloaked; cloak only after rolling + researching `BansheeCloak`.

## Acceptance criteria
- [ ] A freshly built DuskWing cannot cloak by default.
- [ ] After rolling + researching the cloak upgrade, it can.
- [ ] The cloak-on button hides until researched (the requirement should handle this).

## Notes
Deliberate nerf; also makes the `BansheeCloak` roll meaningful instead of inert. Verify the requirement id (`UseCloakingField`) during build. Overriding a campaign ability in `AbilData.xml` is a normal catalog override (no CUnit CardLayouts risk here).
