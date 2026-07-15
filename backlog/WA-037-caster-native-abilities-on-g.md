---
id: WA-037
status: done
size: S
phase: 1-game-readiness
priority: 29
---
# Caster native abilities sitting on hotkey G (collide with rolled F_ abilities)

## ✅ Done 2026-07-14 (verified in-game)
Rule (user): don't change a caster hotkey from its stock default unless the stock default is itself G. Looked up every stock default in `reference/mods/*/enus.sc2data/localizeddata/gamehotkeys.txt` — all four confirmed problems have **non-G** stock defaults, so they just revert to stock (set to the stock value rather than deleting, to avoid the "shows nothing" failure mode):
- `ForceField` G → **F** (stock)
- `BlindingCloud` G → **B** (stock)
- `Transfusion` G → **T** (stock)
- `FungalGrowth` G → **F** (stock)
- `GuardianShield` — stock IS G, already moved to `V` earlier (correct).
- **Raven fine:** card face is `AutoTurret` (stock `T`); the `BuildAutoTurret=G` entry is a leftover on a non-card id. No change.
- **Viper Abduct:** verified on `D` in-game (stock) — `Yoink=G` was a harmless leftover on a non-card id. No change.
- **F_ rolled version unaffected:** forced High Templar to roll FungalGrowth → `F_FungalGrowth` still lands on `G` (via `G=G` fallback). Confirms moving a native off G to its stock letter doesn't touch the rolled `F_` twin.

---

## Why
This is the **caster half of the hotkey-collision work** — WA-033 shipped only the train-slot collisions + addon consolidation. `G` is reserved for rolled upgrade abilities (every `F_*` is on G). Several casters have a **native** ability *also* parked on G (a leftover from before the `F_`-duplicate approach existed). So any time that caster rolls an F_ ability, its native G-ability and the rolled G-ability collide on the same card.

## The collisions (verified in current `GameHotkeys.txt`, 2026-07-14)
Each native ability lives on exactly ONE unit, and its `F_<name>` rolled twin is a **separate** button that stays on G — so moving the native off G is safe and doesn't touch the rolled version. Precedent: `GuardianShield` was already moved `G→V` this way.

| Unit | Native ability | GameHotkeys entry | Suggested → | Verify against (same card) |
|---|---|---|---|---|
| Sentry | Force Field | `ForceField=G` | **F** | GuardianShield=`V`, Hallucination |
| Viper | Blinding Cloud | `BlindingCloud=G` | **B** | ParasiticBomb, Consume |
| Viper | Abduct (Yoink) | `Yoink=G` | **Y** | ParasiticBomb, Consume, BlindingCloud→B |
| Queen | Transfusion | `Transfusion=G` | **T** | (only ability on card — trivially safe) |
| Infestor | Fungal Growth | `FungalGrowth=G` | **F** | NeuralParasite, AmorphousArmorcloud |
| Raven ⚠️ | Build Auto-Turret | `BuildAutoTurret=G` | **T** | ShredderMissile, ScramblerMissile |

**Confirmed FINE (no change):** Ghost, High Templar, Phoenix, Void Ray, Medic, Hellion.

## ⚠️ Raven has the Lurker-style face-id trap
The Raven's turret button uses **face `AutoTurret`** but the hotkey entry is keyed **`BuildAutoTurret`** (`UnitData` line: `Face="AutoTurret" AbilCmd="BuildAutoTurret,Execute"`). Same mismatch that fooled the Lurker — changing `BuildAutoTurret=G` may not move the displayed key. Verify in-game which id actually controls it (may need `Button/Hotkey/AutoTurret=T` instead). Raven wasn't in the user's original list, so **confirm it's actually on G in-game** before fixing.

## How to verify each fix
Roll ANY F_ ability onto the caster (devMode force) and confirm the native ability and the rolled F_ ability no longer share G — and that the new letter doesn't collide with another ability already on that unit's card (the "verify against" column).

## Notes
Suggested letters are mnemonic starting points (F/B/Y/T), not guarantees — the intra-card check is the real gate, and you know your cards. These are `GameHotkeys.txt` value changes `G → letter` (set via the editor, or direct like `LurkerMP`). Related: [[reference-hotkey-resolution]], WA-033 (train half, done).
