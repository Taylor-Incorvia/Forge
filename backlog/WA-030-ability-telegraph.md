---
id: WA-030
status: backlog
size: M
phase: 1-game-readiness
tier: nice-to-have
priority: 40
---
# Ability telegraph — icon/name above the caster on an upgrade-granted ability (nice-to-have)

**Nice-to-have.** Legibility feature: when a unit casts an ability it gained from an upgrade (Force Field on a Marine, Recall on an Infestor, etc.), float an indicator above the caster's head so everyone — especially the opponent — can see *who* cast it and *what* it is. Generalizes the stim-indicator win (WA-026) into a proper "ability telegraph."

## Feasibility (investigated) — MEDIUM, and locally testable
The right tool is the **TextTag** system (world-space UI attached to a unit), NOT actor models (a 2D icon isn't a world model). Key natives:
- `TextTagCreate` + `TextTagAttachToUnit(tag, unit, heightOffset)` — pins it above the caster.
- `TextTagSetBackgroundImage(tag, iconPath, tiled)` — shows the ability's **icon**.
- `TextTagSetText` — or just the ability **name**.
- `TextTagSetVelocity`/`SetGravity` (float up), `TextTagFogofWar`/`Show` (who sees it), `TextTagDestroy` (cleanup ~1.5–2s).

Runtime galaxy → works in local Test Document (not a command-card button).

## Build
1. Trigger (wired in editor): "Any unit uses an ability" → calls a galaxy fn.
2. `showAbilityIndicator(caster, abilityId)`: create tag → set icon/name → attach above caster → float + fade → auto-destroy.
3. Gate to **upgrade-granted abilities only** (mod already tracks the per-unit granted-ability lists), and reuse the `F_` button icons for the image (hardcoded `ability→icon` map, or `CatalogFieldValueGet` at runtime).

## Two tiers
- **MVP (easy):** floating ability **name** ("Force Field!") — no icon mapping. Ship this first.
- **Full (medium):** ability **icon** via `TextTagSetBackgroundImage` + `F_` button icon paths.

## Decisions to make
- Who sees it? (recommend: anyone with vision of the caster — the point is the opponent learning.)
- Which abilities? (recommend: all upgrade-granted casts.)
- Fog-of-war behavior; duration; spam cap in big fights.

## Caveats
- Confirm the "unit uses ability" event fires for the `F_` abilities.
- Mod-trigger split: you wire the event, Claude writes the galaxy fn.
