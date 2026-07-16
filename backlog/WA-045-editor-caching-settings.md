---
id: WA-045
status: todo
size: S
phase: 1-game-readiness
priority: 24
---
# Investigate whether SC2 Editor Documents-prefs settings fix the caching pain

Ongoing pain: the editor **caches merged catalog / document state** so that some changes don't reflect in the local Test Document (the "publish to verify" class — added/unlock-ability buttons), and external file edits (galaxy/XML edited outside the editor) can get clobbered or ignored. Question: do any settings on **Preferences → Documents** help? Screenshot captured at `backlog/assets/WA-045-editor-documents-prefs.png`.

## Current settings (from the screenshot, 2026-07-16)
- **Optimize Saving For:** `Faster Saves` (vs `Smaller Files`)
- Automatically Save Documents: **off**
- Automatically Backup Documents When Saving: **on** → User Folder; Limit 5 backups; Don't backup more than once / hour
- **Automatically Reload Documents On External File Change:** **on**
- **Remember Extra Data Loaded With Documents:** **off**

## Hypotheses to test (one variable at a time; relaunch/re-open between)
1. **Automatically Reload Documents On External File Change (currently ON)** — most relevant to our workflow (Claude/text-tools edit files on disk while the map is open). Test both states:
   - Does ON reliably pull in external `.galaxy`/XML edits, or does the editor hold a cached copy and overwrite on next save?
   - Does OFF + manual "reload" behave more predictably? Figure out the safe save/reload order so external edits aren't lost.
2. **Optimize Saving For: Faster Saves → Smaller Files** — "Faster Saves" may write incrementally / keep cached blocks; "Smaller Files" likely does a full rewrite. Test whether Smaller Files forces a cleaner rebuild that reflects changes.
3. **Remember Extra Data Loaded With Documents (currently OFF)** — related to dependency/extra-data caching. Test whether toggling changes how merged base-catalog data (our deps) is re-read.

## Also check (not on this tab — note for a follow-up if promising)
- The **Test Document** preferences tab (left sidebar) is the likelier home of the "changes don't show until published" behavior — worth a look after the Documents-tab pass. If a setting there fixes the added-ability-button rendering, it would retire a lot of "publish to verify" friction.

## Acceptance criteria
- [ ] Test each of the 3 hypotheses above, one at a time; record what changed (did a previously-cached change now reflect locally?).
- [ ] Land on a recommended settings combo (or conclude "no change helps").
- [ ] If anything helps, update `docs/dev-testing.md` + the `reference-sc2-editor-testing-constraint` memory with the new known-good workflow.

## Notes
Low risk (just editor preferences, not mod data). Payoff is high if it shrinks the "can't test locally / must publish" loop that gates the caster work. Purely local investigation.
