---
id: WA-114
status: todo
size: L
phase: 2-post-launch
priority: 2
---
# Add WoL content back one commit at a time + bisect the escape-key culprit(s)

Part of [[WA-108]]. Items 9 + 10 (same workflow). Only meaningful if [[WA-112]] showed the bug is
WoL-content-related (bug gone with content removed). If WA-112's bug persisted, redirect to WA-116/WA-076.

## The discipline (Taylor's design — keep it strict)
Starting from the zero-WoL build ([[WA-112]]), add the WoL units/upgrades back **one per commit**, and:
- **Add lines only; change zero existing lines** (or as few as humanly possible). This is what makes the
  end state a clean cherry-pick: once the culprits are known, branch fresh and cherry-pick only the
  non-breaking commits.
- One coherent unit/upgrade (with its full ref subtree) per commit. Re-add the Tech Reactor first ([[WA-113]])
  so the build is functional, then the 7 units (Goliath, Wraith, Diamondback, DuskWing, Firebat, Medic,
  Vulture) and any WoL upgrades kept per [[WA-110]].
- Test the escape repro on a **published** build after each add-back; record pass/fail per commit here.

## Finding culprit(s) — mind the multiple-cause trap
`git bisect` assumes ONE good→bad transition; Taylor expects several. So:
- Prefer the **linear sweep** (test every add-back commit, record result) — with ~7 units + a few upgrades
  it's cheap and each result is unambiguous. This catches ALL culprits, not just the first.
- If you do use `git bisect`, it finds the first culprit only — fix/skip it, then bisect again. Repeat.
- Because commits are add-only, the final fix = a fresh branch cherry-picking every non-breaking commit
  (and re-doing the culprits correctly, informed by what the collision turned out to be).

## What "breaks it" probably looks like
Given [[WA-076]]'s lead, watch for a re-added unit whose command card **renumbers `LayoutButtons`** or
adds a button that collides with Cancel's slot/key on Standard re-resolution. Note the exact card entry
for each culprit — that's the data to fix.

## Acceptance
- [ ] Every WoL add-back is its own add-only commit with a recorded escape-repro result.
- [ ] The culprit set is identified (could be >1).
- [ ] A clean branch exists cherry-picking the safe commits + corrected versions of the culprits.
- [ ] File follow-up tickets for each distinct breakage found (epic item 11).
