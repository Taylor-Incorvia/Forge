---
id: WA-024
status: todo
size: M
phase: 1-game-readiness
priority: 20
---
# Enable nuke: Ghost Academy builds it, Ghost uses it

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
- [ ] Diagnose which of the above is actually blocking it (start here — don't assume).
- [ ] Ghost Academy can arm a nuke (`ArmSiloWithNuke` available, requirements satisfiable in the mod).
- [ ] Ghost has `TacNukeStrike` on its command card, with a hotkey that doesn't collide (see WA-005 hotkey rules — the Ghost Academy card is already busy with research buttons).
- [ ] Full loop works in a test game: arm silo → ghost calls down nuke → it detonates.

## Design considerations (flag, don't block)
- Every player has a Ghost Academy (it's the Barracks upgrade facility), so *anyone* could arm a silo — but only players who rolled a Ghost can actually use it. Decide if that's fine.
- Does the mod use Tech Labs? If the stock nuke requirement wants one, either satisfy it or simplify the requirement to just Ghost Academy.
- Balance/telegraph: nukes are strong; the red-dot warning is the counterplay. Keep it visible.

## Notes
Ghost = Barracks slot 4 (`initialize.galaxy`). Command-card slot pressure on the Ghost Academy is the crux — it's doing double duty as an upgrade facility. `reference/` has the full stock nuke wiring if you need to compare against a clean setup.
