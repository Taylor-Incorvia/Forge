# Hotkey Collision Audit (WA-005)

_Generated 2026-07-13. Review artifact — no data changed. Two rules: (1) units built from the same facility in different slots must not share a train hotkey; (2) a caster's default ability must not sit on `G` (rolled-upgrade ability buttons all live on G, so a native ability on G collides with a rolled one on the unit's card)._

## Rule 1 — train-hotkey collisions (within a facility)

**Barracks — 2 collisions:**
- ⚠️ **Letter `A`**: `Marine` (slot 2, forced `Marine2=A` in GameHotkeys.txt) collides with `Zergling`/`Zealot` (slot 1, alias→Marine=A). All appear on the Barracks card together.
- ⚠️ **Letter `E`**: `Firebat` (slot 3, forced `Firebat=E`) collides with `Hydralisk`/`Queen` (slot 2, alias→Reaper=E).

Both come from explicit `GameHotkeys.txt` entries that pull a unit onto a *different* slot's letter (their ButtonData `HotkeyAlias` would otherwise keep them separate). Fix = change those two letters so each Barracks slot has a distinct train key.

**Factory — none.** slot1=Hellion, slot2=SiegeTank/WidowMine (WarHound on S), slot3=Thor. Distinct.
**Starport — none.** slot1=Viking, slot2=Banshee, slot3=Raven/R. Distinct.

(Within-slot sharing is fine — units in the same slot are mutually exclusive.)

## Rule 2 — caster default ability on `G`

A native ability on `G` collides with any rolled `F_` ability (which the mod always places on G) once the unit rolls one. Precedents already fixed: GuardianShield→`V`, Phoenix GravitonBeam→`V`.

**Active violations:**
- ⚠️ **Sentry** — `ForceField=G`. Stock card (no override). *This is the same bug the GuardianShield move was meant to fix — GuardianShield got moved, ForceField was left on G.* Barracks slot 3.
- ⚠️ **Viper** — `BlindingCloud=G` **and** Abduct/`Yoink=G` (two on G). Starport slot 3.
- ⚠️ **Queen** — `Transfusion=G`. Barracks slot 2.

**Latent (unit not currently in a pool):**
- ScienceVessel — `Irradiate=G` (commented out of pools).

**Checked, clean:** Medic, Infestor, HighTemplar, Ghost, Phoenix, CorsairMP, Wraith, Raven (its card faces `AutoTurret`/`RavenShredderMissile`/`RavenScramblerMissile` differ from the G-listed ids — worth an in-game glance on the AutoTurret face, but by face-governs-hotkey it's not G), DefilerMP, ArbiterMP.

## → WA-033
All fixes are `GameHotkeys.txt` (and possibly `ButtonData.xml`) letter changes. ⚠️ Per `docs/static-prototype-attempt-1.md`, hotkey resolution in this mod has historically been finicky — change one letter, test, don't spiral.
