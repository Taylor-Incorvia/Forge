---
id: WA-102
status: done
size: M
phase: 2-post-launch
priority: 0
---
# Fix placeholder tooltip titles / ability names showing the raw string key

## Problem
Many upgrade/ability names show up in-game as the **raw string id** instead of a readable title — e.g. a tooltip header reads `GoliathRange2` or `Range2` or `D8Charge1` instead of "Goliath Range" / "Cluster Bomb". These are GameStrings entries whose **value equals the key** (self-referential placeholders that were never filled in), so SC2 renders the id. A player hovering an ability sees gibberish instead of what it does — a legibility failure ([[balance-upgrade-legibility]]), and it looks unfinished.

## Scope (as of 2026-09-06)
**56 self-referential placeholder entries** in `enUS.SC2Data/LocalizedData/GameStrings.txt` — find them with:
```
grep -nE '^(Abil/Name|Button/Name|Button/Tooltip)/([A-Za-z0-9_]+)=\2$' GameStrings.txt
```
Mostly `Abil/Name/<X>` for the count/stat upgrade abilities and F_ abilities — examples: `D8Charge1-4`, `F_Blink`, `F_Stimpack`, `HighCapacityBarrels1`, `WraithCloak1`, `TempestRange3`, `SiegeTankRange2`, `GoliathRange2`, `ChitinousPlating3`, `LifestealMarine2`, `PunisherGrenades3`, the whole `Concussive*` family, etc.

Also audit for **entirely missing keys** — an undefined string shows the full path (e.g. `Button/Name/Range2`), which is likely what Taylor saw ("button\buttonname\range2"). The research-button strings are `Button/Name/Research<upgrade><slot>` / `Button/Tooltip/Research<upgrade><slot>` for ability upgrades, and `<upgrade><slot>` for count/stat upgrades (see getUpgradeTooltipKey in factionIcons.galaxy for the exact key each upgrade resolves to — the faction modal reads the same keys, so fixing GameStrings fixes both the command card and the modal).

## Do
1. Give every placeholder a proper **display name** (Abil/Name + Button/Name) and, where missing, a **Button/Tooltip** describing the effect.
2. Fill any entirely-missing research-button strings.
3. Spot-check on the command card AND the Your Faction modal (same strings).

## Notes
Polish/legibility, not gameplay-breaking, but it's the kind of unfinished-looking thing that undercuts the "what did I roll / what does it do" clarity that's core to Wildcard. No XML-comment concern (GameStrings is a text file). Related: [[WA-048]] (one specific tooltip), [[balance-upgrade-legibility]].

## Priority note (2026-09-06)
PRIORITY 0 -- do now + PR: fill in the 56 placeholder tooltip titles/names.

## Done 2026-09-06 (PR pending)
Two parts:
1. Rewrote the 53 self-referential name entries (`Abil/Name/<id>` and a few `Button/Name`) that were rendering the raw id -- e.g. `GoliathRange2`, `ConcussiveVulture1`, `stalkerblinkrange1`, `D8Charge1`. (The 4 remaining key==value entries -- Charge, Irradiate, Transfusion, Stalker -- are real English words that render fine, left alone.) Also fixed the `Infernal Pre-Igniter1` trailing-digit typo and the ugly `F_Abduct`/`F_Yoink` names.
2. **Bigger find via the audit:** the research header the user saw ("button/buttonname/range2") was one symptom of a *systemic* gap -- ~130 research abilities had no `Abil/Name/<id><slot>` and ~130 research buttons had no `Button/Name/Research<id><slot>`, so every research hover showed a raw path. Added all 239 missing strings, names derived from each ability's existing display name where present (Force Field, Fungal Growth, Yamato Cannon, ...) and authored for the rest. Includes a few dead/unused research entries for completeness (harmless).

Still worth an in-editor spot-check on the command card + Your Faction modal, but the strings are all present and readable now.
