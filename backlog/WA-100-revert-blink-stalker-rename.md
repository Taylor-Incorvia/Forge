---
id: WA-100
status: done
size: S
phase: 2-post-launch
priority: 50
---
# Revert WA-091 — rename "Blink Stalker" back to "Stalker"

## What
Reverted the UI name from "Blink Stalker" back to **"Stalker"**:
- `Unit/Name/Stalker` = Stalker (was "Blink Stalker")
- `Button/Name/Stalker` = Stalker (was "Blink Stalker")

## Why
WA-091 renamed it to "Blink Stalker" because it had native blink. The [[WA-096]] rework removes native blink (it's now a rolled upgrade), so "Blink Stalker" is no longer accurate — it's a Stalker that *might* roll blink. Back to "Stalker."

## Sequencing
Ships in the same PR as WA-096 (native-blink removal) — the name is only correct once blink is non-native.

## Done
Both GameStrings entries reverted, in the stalker-rework PR.
