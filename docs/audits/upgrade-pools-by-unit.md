# Upgrade pools by unit

Every unit's **rollable upgrade pool**, derived from the eligibility tags in `upgradeInitializers.galaxy` (see `docs/upgrade-eligibility-tags.md` for the rules). Snapshot 2026-07-22 (post WA-034/WA-049 + WA-052–WA-058, WA-063) — regenerate after changing pools or tags. `[n]` = number of eligible upgrades (pool membership). WA-049 caps then limit how many of a player's units may actually **roll** the same upgrade in one game — see note below.

**Unit combinations (units only): 290,304** = 2·3·4·3 · 3·7·3 · 4·4·4 across the 10 slots.

```
BARRACKS
 s1  Zergling      [6]  Blink, HotSRaptorCharge2, Stimpack, zerglingattackspeed, zerglingmovementspeed, ConcussiveZergling
 s1  Zealot        [6]  Blink, HotSRaptorCharge2, Charge, Stimpack, zerglingattackspeed, ConcussiveZealot
 s2  Hydralisk     [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s2  Marine        [5]  Blink, Range, Speed, Stimpack, LifestealMarine
 s2  Queen        [10]  CorsairMPDisruptionWeb, ForceField, GuardianShield, GravitonBeam, MissilePods, Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s3  Firebat       [6]  Blink, HotSRaptorCharge2, Speed, Stimpack, RavagerCorrosiveBile, ConcussiveFirebat
 s3  Marauder      [6]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, PunisherGrenades
 s3  Sentry        [4]  CorsairMPDisruptionWeb, GravitonBeam, MissilePods, ConcussiveSentry
 s3  Medic         [5]  CorsairMPDisruptionWeb, ForceField, GuardianShield, GravitonBeam, MissilePods
 s4  Ghost        [15]  BlindingCloud, BuildAutoTurret, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, NeuralParasite, ParasiticBomb, RavenScramblerMissile, SeekerMissile, Yoink, Blink, Range, Speed, Stimpack, PersonalCloaking
 s4  Infestor      [9]  ArbiterMPRecall, BlindingCloud, BuildAutoTurret, CorsairMPDisruptionWeb, Irradiate, ParasiticBomb, RavenScramblerMissile, SeekerMissile, Yoink
 s4  HighTemplar  [11]  ArbiterMPRecall, BlindingCloud, BuildAutoTurret, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, NeuralParasite, ParasiticBomb, RavenScramblerMissile, SeekerMissile, Yoink
FACTORY
 s1  Vulture       [5]  Blink, Range, Speed, Stimpack, ConcussiveVulture
 s1  Hellion       [6]  Blink, Speed, Stimpack, HighCapacityBarrels, TwinLinkedFlameThrowers, ConcussiveHellion
 s1  Stalker       [7]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, stalkerblinkcooldown, stalkerblinkrange
 s2  Diamondback   [6]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, ConcussiveDiamondback
 s2  Immortal      [5]  Blink, D8Charge, Range, Speed, Stimpack
 s2  SiegeTank     [4]  Blink, DiggingClaws, Stimpack, SiegeTankRange
 s2  WarHound      [6]  Blink, D8Charge, Range, Speed, Stimpack, RavagerCorrosiveBile
 s2  Archon        [4]  Blink, Range, Speed, ConcussiveArchon
 s2  LurkerMP      [4]  Blink, LurkerRange, DiggingClaws, Stimpack
 s2  Goliath       [5]  Blink, GoliathRange, Speed, Stimpack, RavagerCorrosiveBile
 s3  ThorAP        [6]  Blink, D8Charge, Range, Speed, Stimpack, Yamato
 s3  Ultralisk     [7]  Blink, HotSRaptorCharge2, Range, Charge, Stimpack, Yamato, ChitinousPlating
 s3  Colossus      [7]  Blink, Range, Speed, Stimpack, Yamato, Hyperjump, ConcussiveColossus
STARPORT
 s1  CorsairMP    [10]  ForceField, GuardianShield, GravitonBeam, MissilePods, Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump
 s1  Phoenix      [10]  CorsairMPDisruptionWeb, ForceField, GuardianShield, MissilePods, Blink, PhoenixRangeUpgrade, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump
 s1  Wraith       [13]  CorsairMPDisruptionWeb, ForceField, GuardianShield, GravitonBeam, MissilePods, Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, WraithCloak, Hyperjump, ConcussiveWraith
 s1  VikingFighter [6]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump
 s2  Liberator     [4]  Blink, LiberatorSiegeSpeed, Stimpack, RavagerCorrosiveBile
 s2  Mutalisk      [7]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump, ConcussiveMutalisk
 s2  DuskWing      [8]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, ClusterWarheads, BansheeCloak, Hyperjump
 s2  VoidRay       [4]  Blink, Range, Speed, ConcussiveVoidRay
 s3  Raven         [9]  ArbiterMPRecall, BlindingCloud, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, NeuralParasite, ParasiticBomb, SeekerMissile, Yoink
 s3  Tempest       [5]  Blink, Stimpack, Yamato, TempestRange, Hyperjump
 s3  Viper         [8]  ArbiterMPRecall, BuildAutoTurret, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, NeuralParasite, RavenScramblerMissile, SeekerMissile
 s3  Battlecruiser [5]  Blink, Range, Speed, Stimpack, Yamato
```

## Smallest pools — candidates for a bespoke upgrade
The `[4]` units have the thinnest, most generic pools, so a purpose-built upgrade lands hardest here — aim for something that changes how the unit **micros**, and stays quick to build:

**`[4]`: Sentry · Archon · LurkerMP · SiegeTank · Liberator · VoidRay**
**`[5]`: Hydralisk · Marine · Medic · Immortal · Goliath · Vulture · Tempest · Battlecruiser**

Liberator ideas floated (post-first-push): +1 shot on the anti-air attack, or a much larger AA-splash "Valkyrie mode" (SC1 Valkyrie was the mutalisk-clump counter — bigger splash + higher damage than the SC2 Liberator). See [[WA-056]].

---

_**WA-034 (concussive):** 13 units can roll a **Concussive** slow-on-attack upgrade — Marauder (`PunisherGrenades`) plus `Concussive*` for Zergling, Zealot, Sentry, Vulture, Hellion, Firebat, Diamondback, Archon, Colossus, Wraith, Mutalisk, VoidRay. Hellion's concussive also applies in its **Hellbat** (transformed) form. **Ultralisk pulled** — its `KaiserBlades` injection applies no slow; see [WA-063](../../backlog/WA-063-ultralisk-concussive-no-slow.md). **WA-049 caps:** all Concussive variants share one family capped at **1**; `RavagerCorrosiveBile` / `Yamato` / `Hyperjump` / every caster spell also capped at **1**; Blink / Speed / Range / Stimpack and everything else at **2**. Caps limit rolls, not pool membership, so the `[n]` counts above are unchanged by caps._

_**Recent (2026-07-21/22):** WA-052 Blink cap 1→2 · WA-055 Goliath rolls `GoliathRange` (off generic Range) · WA-057 Phoenix rolls `PhoenixRangeUpgrade` (displayed "Anion Pulse-Crystals", off Range) · WA-054 Liberator rolls `LiberatorSiegeSpeed` (off Speed) · WA-053 Firebat rolls `ConcussiveFirebat` · WA-058 Battlecruiser rolls Yamato (0 native → 1 from the upgrade) · Hyperjump capped at 1 · WA-063 Ultralisk concussive pulled from pool._

_Note: current as of WA-032/WA-040. `RavagerCorrosiveBile` excludes Marine / Hellion / Vulture / Immortal (too cheap or useless) and Archon / VoidRay / SiegeTank / LurkerMP (too expensive or useless). Also reflects Medic ✗Hyperjump and Ghost ✗Recall. WA-032: D8Charge reaches ThorAP + WarHound (tag-case fix). WA-014: Stalker can roll stalkerblinkrange again. WA-025: Transfusion pulled off Starport s1. WA-027: MissilePods low-tier only — off Barracks s4 and Starport s3. WA-023: Ultralisk rolls ChitinousPlating (no longer a free start upgrade). WA-044: Transfusion pulled from the rollable pool entirely for V1; Queen keeps its native (bio-only) Transfusion._
