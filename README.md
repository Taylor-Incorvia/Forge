# Wildcard Arena

**A StarCraft II arcade mod where you don't pick your army — you're dealt one, and you win by adapting to the hand.**

Every game, your production buildings roll **randomized units** into their build slots and your tech buildings roll **randomized upgrades** from tag-filtered pools. No standard build order, no matchup to memorize, no optimal opening to grind — just the faction you were dealt and the decisions you make with it.

## Why it's different

Wildcard Arena is a deliberate bet *against* two RTS conventions — maximal player agency and minimal randomness — in order to kill the problems those conventions create:

- **Depth without a memorization wall.** Skill comes from *reading and adapting* to your roll, not from having memorized 40 units and every matchup.
- **Per-unit balance, not per-matchup balance.** A unit that's too strong just gets nerfed — there's no need to keep three matchups simultaneously fair.
- **No stale meta.** There's no single "correct" build, so the game can't be solved down to one four-year-old opening.

## How it plays

- **Production buildings** — Barracks, Factory, Starport — roll random units (from across all three races) into their slots. You build whatever you rolled.
- **Tech buildings** — Ghost Academy, Armory, Fusion Core — roll random upgrades onto the units you have.
- **Add-ons gate the top slot** of each production building — an add-on *unlocks a building's highest slot* rather than doubling production (this is intentionally **not** stock reactor behavior).
- Expand, tech, and out-decide your opponent with a faction neither of you designed.

## Play it

Search **"Wildcard Arena"** on the StarCraft II Arcade. Community, patch notes, and tournament info: **[wildcardarena.com](https://wildcardarena.com)** *(in progress)*.

## How it's built

Pure StarCraft II Galaxy Editor — no external engine:

- **[`ForgeModLowConfidence.SC2Mod/Base.SC2Data/TriggerLibs/`](ForgeModLowConfidence.SC2Mod/Base.SC2Data/TriggerLibs/)** — the Galaxy trigger libraries that power the mod: per-slot unit randomization, tag-filtered upgrade pools, dynamic ability/behavior/upgrade granting, and UI (the "Your Faction" panel, enemy scout tags).
- **[`ForgeModLowConfidence.SC2Mod/Base.SC2Data/GameData/`](ForgeModLowConfidence.SC2Mod/Base.SC2Data/GameData/)** — the XML catalog data (units, abilities, upgrades, buttons, requirements).
- **[`docs/`](docs/)** — design docs, balance audits, and patch notes. Good entry points: [design principles](docs/design-principles.md), the [build-time & stat analysis](docs/audits/build-times-vs-stock.md), and [patch notes](docs/patch-notes/).
- **[`backlog/`](backlog/)** — the public work board.

## Status

Season 1, actively developed. Latest changes live in [`docs/patch-notes/`](docs/patch-notes/).

## Notes

Built in the StarCraft II Editor with an AI-assisted development workflow. *StarCraft II* and its assets are © Blizzard Entertainment; this is an unaffiliated community mod. Original code and documentation © Taylor Incorvia.
