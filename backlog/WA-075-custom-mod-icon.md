---
id: WA-075
status: todo
size: S
phase: 1-game-readiness
priority: 55
---
# Look into a custom mod icon (currently a missile turret)

## Why
In the client, the icon shown above the mod's description is a **missile turret** — a stock placeholder, not chosen. For Season 1 presentation / branding (this is the first thing a player sees next to "Wildcard Arena"), a **custom icon** would look far more legit. Cosmetic, not a blocker, but cheap polish with outsized first-impression value.

## What we know
- The mod has **no custom image assets checked in** — `find` for `.dds/.tga/.png` under `ForgeModLowConfidence.SC2Mod/` returns nothing. So there is no custom icon today; the turret is a default.
- Mod metadata (Name, DescLong, HowToPlay, Website) lives in `DocumentHeader` / `DocumentInfo`. The icon/preview is most likely **publish-time metadata set in the editor**, not a normal repo asset — confirm whether it's stored in the document or only in the Battle.net publish record.

## To investigate
1. **Where the turret comes from** — is it a true default (no preview set), or is something referencing a stock turret icon? Open the editor's publishing / map (document) info and check the Preview Image / Icon field.
2. **How to set a custom one** — most likely: Editor → File → Publish (or Document Info) → set a custom **Preview Image / Arcade Icon**. Determine:
   - Required **format** (`.dds`/`.tga`?) and **dimensions** (arcade preview/icon has specific specs — verify; don't guess).
   - Whether the image must be **imported into the mod** (Import module) and referenced, or uploaded separately at publish time.
   - Whether the setting is captured in the repo (so it survives / is version-controlled) or lives only in the publish dialog.
3. **Icon vs preview** — clarify if "above the description" is the same asset as the arcade/library thumbnail, or two separate images that both may need setting.

## Deliverable
- A custom icon image (source + exported format), imported/set so the turret is gone.
- Note the exact steps + specs in this ticket (or `docs/`) so it's repeatable.
- If the setting can be committed to the repo, do so; if it's publish-only metadata, document that it must be re-set on publish.

## Notes
- Art: a simple "Forge"/anvil-or-ember mark fits the project's identity (Ember, the Forge). Doesn't need to be fancy for S1 — anything intentional beats the turret.
- Related: WA-006 (mod description), and the branding surfaces the website agent owns.
