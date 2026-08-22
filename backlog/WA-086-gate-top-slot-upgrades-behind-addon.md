---
id: WA-086
status: todo
size: M
phase: 2-polish
priority: 65
---
# Gate top-slot UPGRADE research behind the corresponding add-on

## What
Currently the **add-on gates the top-slot UNIT** (Tech Lab → Barracks slot 4, Reactor → Factory slot 3, Tech Reactor → Starport slot 3), but the **upgrade** for that top-slot unit can be researched at the upgrade facility with no add-on. Extend the gate so the top-slot *upgrade* also requires the add-on:

| Top slot | Add-on required | Upgrade researched at | Research column |
|---|---|---|---|
| Barracks slot 4 | **Tech Lab** | Ghost Academy | slot 4 → column 3 |
| Factory slot 3 | **Reactor** | Armory | slot 3 → column 2 |
| Starport slot 3 | **Tech Reactor** | Fusion Core | slot 3 → column 2 |

## Why
The Barracks-4 / Factory-3 / Starport-3 units may prove too strong for how accessible their upgrades are. The add-on already gates the *unit*; gating its *upgrade* too adds a real tempo/tech cost and keeps parity ("you paid the add-on to field it AND to upgrade it"). **Conditional — only build this if those slots actually over-perform in the meta. Not for S1 launch.**

## Why the normal way doesn't work
The mod grants research abilities via the **GrantAbility / `UnitAbilityAdd` API**, not static command cards — which means **standard ability `Requirements` are completely ignored** (confirmed: `InfoArray.Requirements` is silently dropped, and several other gating mechanisms fail — see `docs/completed-research-button-attempts.md`). So we can't just slap a "HaveTechLab" requirement on the research button.

## The key enabling insight (why this is now feasible)
The completed-research-lockout work failed because removing/swapping a dynamically-added ability **repacks the command card and shifts every later button left** (click slot N → slot N+1 actually researches). **But this nerf targets the FINAL research slot of each facility** — nothing sits to its right, so there's nothing to shift. Add/replace at the last slot is safe.

## Design (REQUIRED — the greyed placeholder is NOT optional)
**Why the placeholder is mandatory:** the upgrade system's whole legibility rests on **positional parity** — Ghost Academy slot N is the upgrade for Barracks slot N; Armory slot N ↔ Factory slot N; Fusion Core slot N ↔ Starport slot N. A greyed slot 4 keeps all four slots visible so that mapping reads at a glance from turn 1. Leaving the slot **blank** until the add-on breaks that parity ("is there even a slot 4? which unit does it map to?") and undermines what makes the system make sense. The Your Faction modal helps, but the in-facility 1:1 parity is its own thing. So: placeholder always shown, swapped for the real button on unlock — **never a blank slot.**

1. **At game start**, grant a **do-nothing ability with a greyed-out-looking button** to the final research slot of Ghost Academy / Armory / Fusion Core. Add it **last** (after the slot 1–3 real research abilities) so it occupies the rightmost/final card slot.
2. **On add-on completion** (Tech Lab / Reactor / Tech Reactor finishes), **swap** the placeholder for the real research ability (remove placeholder → add real). Safe because the placeholder is the last button — removing it shifts nothing to its left (slots 1–3 stay put), and the real ability re-appends into the same final slot. They're *different* ability ids, so the "can't re-add the same ability in one frame" gotcha (attempts doc Approach 2) doesn't apply.
3. Grant to **all** of that player's facilities of that type, and wire it into `applyAbilityListToUnit` so facilities built *later* also get the correct (locked/unlocked) version based on whether the add-on already exists.

## If the same-frame swap misbehaves
The fallback is **NOT** a blank slot (rejected — it breaks the slot parity above, which is the point of the whole feature). Instead: split the remove/add across a `Wait(0, c_timeGame)` frame yield, or use any swap that keeps the placeholder visible until the instant the real button replaces it. The greyed slot must always be present pre-unlock.

## Dependencies
- A **greyed-out placeholder button icon** (the "locked" look). Real research icons already exist.
- Reliable "add-on completed" and "facility created" trigger hooks (the mod already reacts to unit creation via `applyAbilityListToUnit`).

## Acceptance
- [ ] Top-slot upgrade (rax4 / factory3 / starport3) is **not researchable until** the matching add-on (Tech Lab / Reactor / Tech Reactor) is built.
- [ ] After the add-on completes, the real research button appears in the correct final slot and researches the correct upgrade (no shift/misfire onto a neighbor).
- [ ] Applies to all existing facilities of that type AND ones built later; per-player scoped (one player's add-on doesn't unlock another's).
- [ ] Verify on a **published** build (command-card behavior).

## Notes
Only worth doing if the top slots over-perform. Related: `docs/completed-research-button-attempts.md` (full list of gating mechanisms that DON'T work on dynamically-added abilities), CLAUDE.md add-on/slot rules, [[reference-command-card-button-type.md]] (new `LayoutButtons` need `Type="AbilCmd"`).
