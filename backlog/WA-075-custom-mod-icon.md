---
id: WA-075
status: todo
size: S
phase: 1-game-readiness
priority: 55
---
# Use a custom mod icon (currently a stock icon picked from the list)

## Why
The icon shown above the mod's description is a **missile turret** — deliberately chosen from the editor's built-in icon list, not a placeholder, but a stock game icon. For Season 1 presentation / branding (it's the first thing a player sees next to "Wildcard Arena"), we want a **custom** image instead. The blocker is purely "how do you supply your own icon rather than pick a stock one." Cosmetic, not a launch blocker, but cheap polish with outsized first-impression value.

## What we know
- The turret was **selected from a long list of built-in icons** in the editor (likely the standard button-icon set). Those stock icons are almost certainly `.dds` files that ship with the base game.
- Because it's a **stock** asset, nothing is imported into the mod — a `find` for `.dds/.tga/.png` under `ForgeModLowConfidence.SC2Mod/` returns nothing. A *custom* icon would be the first imported image asset.
- The selection persisted across sessions, so the icon reference is stored in the document (`DocumentInfo` / `DocumentHeader`) — meaning once we point it at a custom asset, it should be version-controllable in the repo.

## The actual question
How to make the icon field point at **our own image** instead of a stock list entry. Most likely path:
1. Create the icon image and export as **`.dds`** (match whatever format/dimensions the stock icons use — verify, don't guess; the button-icon set is small, e.g. ~76x76, but the "icon above the description" may be a larger preview — confirm which field the turret actually is).
2. **Import** the `.dds` into the mod via the editor's **Import module** (this is what adds a custom asset to `ForgeModLowConfidence.SC2Mod`).
3. In the same publish / Document Info dialog where the turret was picked, point the icon field at the **imported** asset instead of a stock one (confirm the field lets you browse to imported assets, vs. only offering the stock list — this is the crux unknown).

## To confirm
- **Which field / dialog** the icon lives in (the one where the turret was selected) and whether it accepts a custom/imported path or only the stock list.
- **Format + exact dimensions** required (verify against a stock icon's specs; `.dds` compression type — DXT1/DXT5 — matters for transparency).
- **Icon vs. preview** — is "above the description" the same asset as the arcade/library thumbnail, or two separate images?
- Whether the final reference is captured in `DocumentInfo` (committable) or only in the Battle.net publish record (must be re-set on publish).

## Deliverable
- A custom `.dds` icon imported into the mod and set as the icon (turret gone).
- The exact steps + specs written down (here or in `docs/`) so it's repeatable.
- Commit the imported asset + any `DocumentInfo` change if they live in the repo.

## Notes
- Art: a simple "Forge"/anvil-or-ember mark fits the project identity (Ember, the Forge). Doesn't need to be fancy for S1 — anything custom beats a stock turret. ffmpeg is available if a source image needs conversion, though `.dds` export may need an image tool / editor plugin.
- Related: WA-006 (mod description) and the branding surfaces the website agent owns.
