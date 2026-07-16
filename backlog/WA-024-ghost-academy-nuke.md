---
id: WA-024
status: in-review
size: M
phase: 1-game-readiness
priority: 20
---
# Enable nuke: Ghost Academy builds it, Ghost uses it

## 🔨 Diagnosed + fixed 2026-07-16 (PR — needs in-game confirm)
**Root cause (H2 — requirement fails): CONFIRMED.** The mod's `TrainNuke` override (the `ArmSiloWithNuke` gate) has a `Use` node that AND's three operands, and operand 1 was a **stale stock leftover requiring a completed/lifted Factory**. Its `Show` node only checks "a Ghost was rolled," so the **arm button appears but stays greyed** for any Ghost-roller who hasn't built a Factory → silo never arms → no nuke. Exactly the "shows but dead" symptom.

**Fix:** removed that Factory operand from the `CRequirementAnd` in `RequirementNodeData.xml` (the Use node of `TrainNuke`). Remaining gates: *no nuke already at the silo* AND *a Ghost has been rolled* (`barracks4ghostupg`) — the intended loop.

**Ruled out (no edits needed):**
- **H3 (`TacNukeStrike` stripped): RULED OUT.** The mod's Ghost block makes no `AbilArray` edits (only repositions the CloakOff button); `TacNukeStrike` + the `NukeCalldown` button survive, gated by `HaveNuke` (inherited, passes once armed).
- **H1 (arm button stripped): RULED OUT.** The mod's `AbilArray index="3" removed` drops *GhostAcademyResearch*, not `ArmSiloWithNuke` (index 2); the `NukeArm` button isn't removed either.

**Design questions resolved:** only players who rolled a Ghost can arm (operand 3 gates on `barracks4ghostupg`), so "everyone has a Ghost Academy" isn't a problem. No Tech Lab needed — the mod's override already replaced the stock Tech-Lab requirement; this just drops the leftover Factory check.

**Card collision found 2026-07-16 (in-game test — arm button didn't show):** stock `NukeArm` (card index 3) is at **Row 0 Col 0**, and the mod's trigger-injected rolled-upgrade research buttons occupy **Row 0 Cols 0–3** — so an upgrade button covered it. **Attempted fix:** repositioned `NukeArm` to the free **Row 2 Col 0** in the `GhostAcademy` `CardLayouts` (`UnitData.xml`).

**Re-test still showed no arm button locally (2026-07-16).** Two remaining hypotheses:
1. **Local Test Document card cache** — the change is correct but the local editor isn't re-rendering the Ghost Academy card (the "publish to verify" class). Would work on a published build. → tied to **[WA-045](./WA-045-editor-caching-settings.md)**; retest locally after messing with the Test-Document editor settings.
2. **Trigger rebuilds the card** — the card-injection trigger reconstructs the Ghost Academy command card at runtime and drops static `LayoutButtons`, so the reposition never takes effect (even on prod). If prod also shows no button, the arm button must be added via the card-injection **trigger** (galaxy), not static `CardLayouts`.

**Deferred to a production test** (per user, 2026-07-16): the Ghost rolls ~1 in 3, so it's easy to hit on prod without rigging RNG. The **requirement fix** (Factory operand removed) and the **card reposition** are both committed in PR #4. Verify on the next deploy; if the button still doesn't render, pursue hypothesis 2 (trigger-side button add). Revisit local testability after WA-045.

Now that Ghosts are in the pool (Barracks slot 4), wire up nukes: the Ghost Academy should be able to arm a nuke, and a Ghost should be able to call it down. First step is diagnosing *why* it doesn't work today.

## Why
Ghosts without nuke feel incomplete, and a telegraphed nuke is exactly the kind of dramatic wildcard moment the mod wants. This closes the "ghosts can't nuke" gap noted in the code comments.

## Stock IDs (confirmed from reference/)
- **`ArmSiloWithNuke`** (`CAbilArmMagazine`) — the Ghost Academy ability that builds a `Nuke` into its silo.
- **`TacNukeStrike`** (`CAbilEffectTarget`) — the Ghost's Tactical Nuclear Strike ability.
- **`Nuke`** — the magazine unit.
- Stock requirements: `HaveGhostAcademy`, `TrainNuke`, `HaveNuke`, plus `AndCountUnitBarracksTechLab...GhostAcademy...` (stock nuke needs a Barracks **Tech Lab** + Ghost Academy).
- Note: the mod's own data does **not** reference nuke at all — so this is stock behavior being blocked by the mod's setup, not a custom override gone wrong.

## Likely root cause (to confirm during diagnosis)
The **Ghost Academy is repurposed as the Barracks upgrade facility** in this mod (`getUpgradeFacilityFromProductionFacility`: rax → GhostAcademy). Its command card is driven by the rolled-upgrade research buttons. So the most probable blockers:
1. The stock `ArmSiloWithNuke` button isn't on the Ghost Academy's command card (displaced by research buttons), or its card slot / hotkey collides with the research-button layout.
2. The stock nuke **requirement chain fails** — it wants a Barracks Tech Lab + Ghost Academy, and the mod's repurposed buildings may not satisfy `TrainNuke` / `HaveGhostAcademy` the way stock expects.
3. The Ghost's `TacNukeStrike` ability may have been stripped/left off its command card (the code comments mention ghosts that "can't nuke").

## Acceptance criteria
- [x] Diagnose which of the above is actually blocking it (start here — don't assume). → **H2, the Factory operand in TrainNuke's Use node.**
- [x] Ghost Academy can arm a nuke (`ArmSiloWithNuke` available, requirements satisfiable in the mod). → requirement fixed (pending in-game confirm the button enables).
- [x] Ghost has `TacNukeStrike` on its command card. → verified intact (native `NukeCalldown` button, `HaveNuke` gate).
- [ ] Full loop works in a test game: arm silo → ghost calls down nuke → it detonates. _(published-test caveat — see above)_

## Design considerations (flag, don't block)
- Every player has a Ghost Academy (it's the Barracks upgrade facility), so *anyone* could arm a silo — but only players who rolled a Ghost can actually use it. Decide if that's fine.
- Does the mod use Tech Labs? If the stock nuke requirement wants one, either satisfy it or simplify the requirement to just Ghost Academy.
- Balance/telegraph: nukes are strong; the red-dot warning is the counterplay. Keep it visible.

## Notes
Ghost = Barracks slot 4 (`initialize.galaxy`). Command-card slot pressure on the Ghost Academy is the crux — it's doing double duty as an upgrade facility. `reference/` has the full stock nuke wiring if you need to compare against a clean setup.
