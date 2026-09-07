---
id: WA-113
status: todo
size: M
phase: 2-post-launch
priority: 2
---
# Tech Reactor: minimal, self-contained re-implementation (no WoL dependency)

Part of [[WA-108]]. Item 8. The Tech Reactor is the load-bearing WoL-dependent add-on — the earlier
`wa-078` branch broke it badly (couldn't build one; in dev mode a pre-placed one had a **constantly
shaking / seizure-like idle animation**; "Unable to create duplicate entry" / "Unable to find parent
F_TechReactor" errors from the insert-before extraction leaving duplicate ids).

## Why it exists at all (scope-limiter)
The Tech Reactor exists ONLY because the mod needs a **third unique add-on** — one per unique production
facility (Barracks→Tech Lab, Factory→Reactor, Starport→Tech Reactor). See CLAUDE.md "Add-ons GATE the
top slot." So we do NOT need to port the full campaign Tech Reactor. We need a minimal add-on that is
visually distinct and behaves per the requirements below.

## Hard requirements (Taylor's a–g)
- (a) Constructible as an add-on **to a Starport**.
- (b) **No effect if a Factory or Barracks lifts and lands on it** (must not gate their slots / must not attach usefully).
- (c) Looks **distinctly different from a Tech Lab**.
- (d) **Does not visually seizure** (the idle-animation shake bug must not return — likely the model/actor
  binding or a duplicate actor from the bad extraction; get the model path + a single clean actor right).
- (e) **Unlocks Starport slot-3 production.**
- (f) **Does NOT enable doubled production** (it's a slot-gate, not a stock reactor — see CLAUDE.md).
- (g) **Same hotkey as the build-Tech-Lab hotkey** (the consolidated add-on `X`; one add-on button per
  facility, no collision with train buttons).

## Approach (Taylor's first attempt, 2026-09-07): duplicate the Tech Lab as the starting point
The Tech Lab already does **most** of what's needed (constructible add-on, gates a slot, correct
add-on behavior, no doubled production). So start by **duplicating the Tech Lab** into a new unit
(new id, distinct from Tech Lab), then change only what differs:
- **Model / actor** → a tech-reactor look (requirement c), with a single clean actor + correct `.m3`
  path so the idle doesn't shake (requirement d — the shake was the WoL extraction's duplicate/bad actor).
- **Attach to Starport** instead of Barracks; ensure it does nothing useful for a Factory/Barracks that
  lands on it (requirements a, b).
- **Gate Starport slot 3** (requirement e); keep it a slot-gate, not a stock reactor — no doubled
  production (requirement f).
- **Build hotkey** = the Tech Lab add-on hotkey (requirement g) — inherited for free if duplicated, just
  confirm no card collision.

This sidesteps the campaign object graph entirely (which is what caused the duplicate-id / "Unable to
find parent F_TechReactor" / seizure-idle problems). If duplicating the Tech Lab proves it needs more
than "a few property + model changes," fall back to a from-scratch minimal add-on mirroring the
Tech Lab / Reactor wiring.

## Acceptance
- [ ] Can build a Tech Reactor on a Starport; can't when it's not a Starport add-on.
- [ ] Idle animation is stable (no shake).
- [ ] Starport slot-3 units become buildable only with it attached; no doubled production.
- [ ] Build hotkey matches the Tech Lab add-on hotkey; no card collision.
- [ ] No WoL/campaign reference remains for the add-on.
