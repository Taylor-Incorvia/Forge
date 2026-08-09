---
id: WA-075
status: todo
size: S
phase: 1-game-readiness
priority: 55
---
# Use a custom splash / preview image (currently a stock one)

## Why
The image shown above the mod's description is a **missile turret** — deliberately chosen from a list in the editor, but a stock game image. It's a **splash / preview image** (large — much bigger than an icon), the first thing a player sees next to "Wildcard Arena". For Season 1 presentation / branding we want a **custom** one. The only blocker is "how do you supply your own image rather than pick from the stock list." Cosmetic, not a launch blocker, but cheap polish with outsized first-impression value.

## Where it's configured (known)
- It's set in the editor's **Document Info modal** — the **same modal as the mod description**. The description copy (Name, DescLong, HowToPlay*) lives in `ForgeModLowConfidence.SC2Mod/DocumentHeader`, written through that modal and committed to the repo via editor saves.
- So the splash selection is almost certainly stored in `DocumentHeader` / `DocumentInfo` too → once we point it at a custom image, it should be **version-controllable** in the repo (no publish-only surprise).
- It's likely the **Arcade / Preview** section of Document Info (the big preview image + the how-to-play text live together). Stock options are a picker list; the chosen turret is a base-game asset (`.dds`), which is why nothing is imported into the mod today (a `find` for `.dds/.tga/.png` returns nothing).

## The actual question
Within that Document Info splash/preview field, how to point it at **our own image** instead of a stock list entry. Most likely path:
1. Create the splash art and export as **`.dds`** (match the required format/dimensions — verify against the stock preview specs; splash/preview is large, not icon-sized — get the exact size + aspect ratio, don't guess; `.dds` compression DXT1/DXT5 matters for transparency).
2. **Import** the `.dds` into the mod via the editor's **Import module** (this is what adds a custom asset to `ForgeModLowConfidence.SC2Mod`).
3. In the Document Info modal, set the preview/splash field to the **imported** asset instead of a stock one.

## To confirm
- **The crux unknown:** does that splash field let you browse to a custom/imported asset, or only offer the stock list? If stock-only, find the alternate mechanism (e.g. a dedicated custom-preview upload in the Publish dialog).
- **Exact dimensions + format** for the splash/preview (verify; it's the large preview, not a 76x76 icon).
- **Splash vs. thumbnail** — is "above the description" the same asset as the arcade/library thumbnail, or two separate images that both need setting?
- Whether the final reference lands in `DocumentHeader`/`DocumentInfo` (committable) or only in the Battle.net publish record.

## Deliverable
- A custom `.dds` splash imported into the mod and set as the preview (turret gone).
- The exact steps + specs written down (here or in `docs/`) so it's repeatable.
- Commit the imported asset + the `DocumentHeader`/`DocumentInfo` change.

## Notes
- Art: a "Forge"/anvil-or-ember mark fits the project identity (Ember, the Forge). Doesn't need to be fancy for S1 — anything custom beats a stock turret. ffmpeg is available for source conversion, though `.dds` export likely needs an image tool / editor plugin.
- Related: WA-006 (mod description — same modal) and the branding surfaces the website agent owns.
