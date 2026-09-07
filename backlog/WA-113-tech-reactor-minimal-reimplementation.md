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

## Approach
Build it as its own clean unit (its own id, distinct from Tech Lab), minimal data — enough to construct,
attach to a Starport, look like a tech-reactor, and gate slot 3. Don't inherit the campaign object graph
that caused the duplicate-id / shake problems. Cross-check against how the Tech Lab / Reactor add-ons are
wired in the mod today (they're the working pattern to mirror).

## Acceptance
- [ ] Can build a Tech Reactor on a Starport; can't when it's not a Starport add-on.
- [ ] Idle animation is stable (no shake).
- [ ] Starport slot-3 units become buildable only with it attached; no doubled production.
- [ ] Build hotkey matches the Tech Lab add-on hotkey; no card collision.
- [ ] No WoL/campaign reference remains for the add-on.
