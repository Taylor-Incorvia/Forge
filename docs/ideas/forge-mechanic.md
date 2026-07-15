# The Forge (the mod's namesake — cut from v1, kept for someday)

> This is the idea the whole project is named after. "Forge" / "Forge RTS" was the original working title before "Wildcard Arena," which is why the repo, the mod folder lineage, and a few vestigial changes to the Forge structure exist even though you currently can't build a Forge. It was cut to ship — not because it's bad. Writing it down so it's safe to set down.

## The pitch
Once you finish an **Armory** or **Fusion Core**, you unlock the ability to build a **Forge** — a structure that rolls **randomized upgrades that could be literally anything**, well beyond the normal per-slot pools. It's a wildcard on top of the wildcard.

## The twist: one-time-use, then it explodes
The Forge is **single-use**. The moment it finishes researching an upgrade (or building its unit), it **detonates in a big, obvious explosion**. Framing candidates: **Unstable Forge / Nuclear Forge / Mystery Forge**. The explosion is a feature — it's loud, readable, and makes each Forge a real decision.

## What a Forge could roll
- **Meta-upgrades** — improve an upgrade you've *already* researched. E.g. you have Blink on your Zerglings → the Forge offers "reduce Blink cooldown" or "increase Blink distance."
- **Player-wide buffs** — e.g. lifesteal on *all* your units at once (cf. the shared-lifesteal idea in [WA-035], which came from this).
- **A second upgrade on a single unit** — stack another upgrade onto a unit that's normally capped. e.g. a unit already has Blink → now also research Stimpack onto it.
- **The 4th slot = a mega/hero unit** — idea: the Forge rolls three upgrades into slots 1–3, and the 4th slot is instead **"build one singular hero/mega unit."** Envisioned: a **Brutalisk** or a **Loki Battlecruiser**. One-off, powerful, memorable.

## Why it was cut
Scope. This was brainstormed *before* actually building SC2 mods and discovering how painful the data/editor workflow is. It's a large, cross-cutting system (new structure, a whole second randomization layer, meta-upgrade detection, hero units, the explode-on-use lifecycle). Right call to cut for v1; revisit if the mod grows legs.

## If revived, likely dependencies / seeds already in the codebase
- Vestigial Forge-structure edits already in the data.
- The per-slot randomization + tag system generalizes to a Forge pool.
- Player-wide upgrade grants already exist (the count-upgrade path).
