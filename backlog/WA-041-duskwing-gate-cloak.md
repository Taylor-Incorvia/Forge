---
id: WA-041
status: todo
size: S
phase: 1-game-readiness
priority: 31
---
# Nerf DuskWing: make cloak a roll instead of free (gate its cloak ability)

## Why
The DuskWing is over-strong right now, and **free cloak is a big part of it**. It's been the deciding factor across multiple games; it's so obviously optimal that a brand-new player massed cloaked DuskWings unprompted, both games. The user sometimes refuses to use its cloak because it feels too strong. Making cloak a **roll** (not guaranteed) reins it in.

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
