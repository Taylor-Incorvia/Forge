---
id: WA-074
status: done
size: M
phase: 1-game-readiness
priority: 5
---
# Scout roll-icons are explored-gated, not vision-gated (fog leak)

## ✅ RESOLVED (2026-08-11)
Fixed with the "ideal" zero-poll approach: **`TextTagSetFogVisibility(tagId, c_visTypeFog)`**
at tag creation (`scoutTags.galaxy`), which re-gates each tag on the fog (grey) layer so it
requires **current** vision instead of merely-explored ground. Confirmed in-game: icons show
**only while you have active vision** of the enemy structure and **disappear when you leave** —
no polling, no timer trigger. The fallback (maintained group + 0.5s poll) was not needed.
Merged in PR #38; the fallback PR #39 was closed. The devMode/AI test harness was stripped
before merge.

## 🚫 Season 1 launch blocker (original)
The scout-roll feature (WA-070) is a headline 3.0 selling point, and it currently
does something **weaker and jankier than advertised**. The 3.0 patch note promises
*"Fog-gated — you only see it while you have vision, so send a scout."* The code
does not deliver that. Fix before S1.

## Symptom (reported in play)
A player sees the **upgrade icons floating above an enemy tech structure while having
NO vision of that structure** — specifically after scouting *near* that base earlier,
without ever having had direct vision of the structure itself.

This is not a snapshot artifact. Buildings leave a **permanent snapshot** the instant
you first see them, so "saw it then lost the snapshot" cannot happen — if you'd ever
seen the structure you'd see it forever. The player is seeing icons for a building
**they genuinely never had vision of.**

## Root cause: text tags gate on EXPLORED terrain, unit snapshots gate on VISION
Two different fog rules:
- A **building model / snapshot** requires you to have had actual **vision of the
  object**. No vision of the object → nothing drawn, ever.
- A **fog-gated text tag** (`useFogofWar = true`) behaves like terrain/doodads: it shows
  as soon as the tag's **position has been explored once** (grey fog), and it does **not**
  track current, active vision.

So the tag's bar (ground explored) is lower than the building's bar (object seen). The
tell in the report — *"scouted the base but not quite the structure"* — fits the most
likely sequence exactly:
1. Early scout **explores the terrain** of the enemy base, including the tile the tech
   structure later sits on.
2. The structure is **built afterward** (or was just outside the scout's sight radius),
   so the player **never has vision of the object** → no snapshot → building invisible.
3. The tag is created when the structure is built; its fog test only asks *"is this spot
   explored?"* — yes → icons show, floating over a building the player has never seen.

**Design cost:** this makes scouting a **one-time unlock** of a base's *future* rolls
rather than an ongoing cost. That undercuts the whole scouting loop (and the "economic
clarity / read the game" thesis — see `docs/design-principles.md`). One early poke =
permanent roll-omniscience of everything that base ever builds.

## Native surface — CONFIRMED
All verified against `reference/mods/core.sc2mod/base.sc2data/triggerlibs/natives.galaxy`:
- `TextTagCreate(text, fontSize, point, heightOffset, inShow, useFogofWar, playergroup)` — line 3460. The 6th param is literally named `useFogofWar` upstream; current code passes `true`.
- `TextTagShow(int inTag, playergroup inPlayers, bool inShow)` — **line 3490. Per-player show via a playergroup — this is the gate lever.**
- `TextTagFogofWar(int inTag, bool inFog)` — line 3493 (toggle the create-time flag).
- `TextTagSetFogVisibility(int inTag, int inVisType)` — line 3494 (see Approach A).
- `VisIsVisibleForPlayer(int player, point inPos)` — **line 5207. Returns whether a point is CURRENTLY, ACTIVELY visible to a player — exactly the check we need** (point-based; a building's `UnitGetPosition` is fine, works for lifted buildings too).
- Vis-type constants: `c_visTypeMask = 0` (unexplored/black), `c_visTypeFog = 1` (explored/grey) — lines 5196-5197.
- **No `UnitIsVisible(unit, player)` native exists** — use `VisIsVisibleForPlayer(player, UnitGetPosition(u))` instead.

## Approach A — try first (may be a one-liner, no periodic trigger)
The bug is that the tag shows in the *explored/grey* layer. `TextTagSetFogVisibility`
picks which vis layer gates the tag. It's plausible that after `TextTagAttachToUnit`,
calling:
```galaxy
TextTagSetFogVisibility(tag, c_visTypeFog);   // vs the current mask-layer behavior
```
makes the tag hide in grey fog (require active vision). Semantics of this native aren't
documented, so **test empirically** — it's a 5-minute in-game check and if it works it
kills the whole periodic-trigger cost. Also worth toggling `TextTagFogofWar(tag, bool)`
to see if the runtime call differs from the create-time flag. If neither reliably hides
on vision-loss, go to Approach B.

## Approach B — reliable fix (manual per-player vision gate)
Stop trusting the built-in fog flag; gate the tag ourselves against live vision on a
periodic trigger.

### `scoutTags.galaxy` changes
1. **Create the tag ungated + hidden**, and track the unit in a group:
   ```galaxy
   unitgroup scoutTaggedBuildings = ...;   // module global, init empty in an init fn
   ```
   In `onProductionStructureCreated`, pass `inShow = false` and `useFogofWar = false`
   (our loop is the sole control), then `UnitGroupAdd(scoutTaggedBuildings, u)`.
2. **New periodic handler** (wired to an editor "Every 0.5 seconds" trigger →
   `Custom Script: updateScoutTagVisibility();`). C89: all locals declared at top.
   ```galaxy
   void updateScoutTagVisibility() {
       int gi; int gcount;
       int pi; int pcount;
       unit u; int tag; int owner; int p;
       playergroup enemies;
       bool vis;

       gcount = UnitGroupCount(scoutTaggedBuildings, c_unitCountAll);
       gi = 1;
       while (gi <= gcount) {
           u = UnitGroupUnit(scoutTaggedBuildings, gi);
           tag = FixedToInt(UnitGetCustomValue(u, scoutTagCustomIndex));
           owner = UnitGetOwner(u);
           enemies = PlayerGroupAlliance(c_playerGroupEnemy, owner);
           pcount = PlayerGroupCount(enemies);
           pi = 1;
           while (pi <= pcount) {
               p = PlayerGroupPlayer(enemies, pi);
               vis = VisIsVisibleForPlayer(p, UnitGetPosition(u));   // ACTIVE vision only
               TextTagShow(tag, PlayerGroupSingle(p), vis);
               pi += 1;
           }
           gi += 1;
       }
   }
   ```
   Per-player is required: different enemies have different vision, so we can't pass the
   whole enemy group with one bool — hence `PlayerGroupSingle(p)` per player.
3. **`onProductionStructureRemoved`**: also `UnitGroupRemove(scoutTaggedBuildings, u)`
   alongside the existing `TextTagDestroy`.

### Wiring (CLAUDE.md split — user does the editor side)
- New trigger: **Event** = Timer - Every 0.5 seconds of Game Time; **Action** =
  `Custom Script: updateScoutTagVisibility();`. (Existing create/remove triggers unchanged.)

### Cost / notes
- O(tagged buildings × enemy players) per tick — trivial for 1v1, fine for FFA. 0.5s
  cadence is imperceptible for a strategic overlay; raise to 0.25s if it feels laggy.
- Lift-off morph keeps the same unit handle, so group membership + `UnitGetPosition`
  track the flying form for free.
- The `2.0` height offset is applied twice (in `TextTagCreate` and `TextTagAttachToUnit`)
  — unrelated to this bug, but worth trimming one while we're in here.

## Also fix
- **Patch-notes wording** in `docs/patch-notes/2026-07-27.md` — once behavior matches,
  the "you only see it while you have vision" line becomes true. If we shipped 3.0 with
  the leak, note the fix in the S1 notes.

## Acceptance criteria
- [ ] Roll-icons appear **only while the viewing player currently has active vision** of the structure, and **disappear when vision is lost** (fog).
- [ ] Icons do **NOT** appear for a structure the player has merely explored the ground near but never had vision of (the reported bug).
- [ ] Behavior holds for a structure built *after* the area was scouted.
- [ ] Per-player correct in a 3+ player game (enemy A with vision sees it; enemy B without vision does not).
- [ ] No orphaned tags on structure death; no perf hitch from the periodic loop.
- [ ] Verified on a **published** build (vision/fog behavior won't show in the local Test Document).

## Related
Follow-up to **WA-070** (resolves the `useFogofWar` risk it explicitly deferred, line 48).
Design rationale: `docs/design-principles.md` → "Economic clarity."
