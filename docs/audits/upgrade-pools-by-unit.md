# Upgrade pools by unit

Every unit's **rollable upgrade pool**, derived from the eligibility tags in `upgradeInitializers.galaxy` (see `docs/upgrade-eligibility-tags.md` for the rules). Snapshot 2026-07-15 (post WA-040) — regenerate after changing pools or tags. `[n]` = number of eligible upgrades.

**Unit combinations (units only): 290,304** = 2·3·4·3 · 3·7·3 · 4·4·4 across the 10 slots.

```
BARRACKS
 s1  Zergling      [5]  Blink, HotSRaptorCharge2, Stimpack, zerglingattackspeed, zerglingmovementspeed
 s1  Zealot        [5]  Blink, HotSRaptorCharge2, Charge, Stimpack, zerglingattackspeed
 s2  Hydralisk     [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s2  Marine        [5]  Blink, Range, Speed, Stimpack, LifestealMarine
 s2  Queen        [10]  CorsairMPDisruptionWeb, ForceField, GuardianShield, GravitonBeam, MissilePods, Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s3  Firebat       [5]  Blink, HotSRaptorCharge2, Speed, Stimpack, RavagerCorrosiveBile
 s3  Marauder      [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s3  Sentry        [3]  CorsairMPDisruptionWeb, GravitonBeam, MissilePods
 s3  Medic         [5]  CorsairMPDisruptionWeb, ForceField, GuardianShield, GravitonBeam, MissilePods
 s4  Ghost        [15]  BlindingCloud, BuildAutoTurret, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, NeuralParasite, ParasiticBomb, RavenScramblerMissile, SeekerMissile, Yoink, Blink, Range, Speed, Stimpack, PersonalCloaking
 s4  Infestor      [9]  ArbiterMPRecall, BlindingCloud, BuildAutoTurret, CorsairMPDisruptionWeb, Irradiate, ParasiticBomb, RavenScramblerMissile, SeekerMissile, Yoink
 s4  HighTemplar  [11]  ArbiterMPRecall, BlindingCloud, BuildAutoTurret, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, NeuralParasite, ParasiticBomb, RavenScramblerMissile, SeekerMissile, Yoink
FACTORY
 s1  Vulture       [4]  Blink, Range, Speed, Stimpack
 s1  Hellion       [5]  Blink, Speed, Stimpack, HighCapacityBarrels, TwinLinkedFlameThrowers
 s1  Stalker       [7]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, stalkerblinkcooldown, stalkerblinkrange
 s2  Diamondback   [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s2  Immortal      [5]  Blink, D8Charge, Range, Speed, Stimpack
 s2  SiegeTank     [4]  Blink, DiggingClaws, Stimpack, SiegeTankRange
 s2  WarHound      [6]  Blink, D8Charge, Range, Speed, Stimpack, RavagerCorrosiveBile
 s2  Archon        [3]  Blink, Range, Speed
 s2  LurkerMP      [4]  Blink, LurkerRange, DiggingClaws, Stimpack
 s2  Goliath       [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s3  ThorAP        [6]  Blink, D8Charge, Range, Speed, Stimpack, Yamato
 s3  Ultralisk     [7]  Blink, HotSRaptorCharge2, Range, Charge, Stimpack, Yamato, ChitinousPlating
 s3  Colossus      [6]  Blink, Range, Speed, Stimpack, Yamato, Hyperjump
STARPORT
 s1  CorsairMP    [10]  ForceField, GuardianShield, GravitonBeam, MissilePods, Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump
 s1  Phoenix      [10]  CorsairMPDisruptionWeb, ForceField, GuardianShield, MissilePods, Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump
 s1  Wraith       [12]  CorsairMPDisruptionWeb, ForceField, GuardianShield, GravitonBeam, MissilePods, Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, WraithCloak, Hyperjump
 s1  VikingFighter [6]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump
 s2  Liberator     [4]  Blink, Speed, Stimpack, RavagerCorrosiveBile
 s2  Mutalisk      [6]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump
 s2  DuskWing      [8]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, ClusterWarheads, BansheeCloak, Hyperjump
 s2  VoidRay       [3]  Blink, Range, Speed
 s3  Raven         [9]  ArbiterMPRecall, BlindingCloud, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, NeuralParasite, ParasiticBomb, SeekerMissile, Yoink
 s3  Tempest       [5]  Blink, Stimpack, Yamato, TempestRange, Hyperjump
 s3  Viper         [8]  ArbiterMPRecall, BuildAutoTurret, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, NeuralParasite, RavenScramblerMissile, SeekerMissile
 s3  Battlecruiser [4]  Blink, Range, Speed, Stimpack
```

_Note: current as of WA-032/WA-040. `RavagerCorrosiveBile` now excludes Marine / Hellion / Vulture / Immortal (too cheap or useless) and Archon / VoidRay / SiegeTank / LurkerMP (too expensive or useless). Also reflects Medic ✗Hyperjump and Ghost ✗Recall. WA-032: D8Charge now correctly reaches ThorAP + WarHound (tag-case fix); Battlecruiser no longer rolls Yamato (has it natively). WA-014: Stalker can roll stalkerblinkrange again. WA-025: Transfusion pulled off Starport s1 (Corsair/Phoenix/Wraith) — now Sentry/Medic only. WA-027: MissilePods now low-tier only — off Barracks s4 (Ghost/Infestor/HighTemplar) and Starport s3 (Raven/Viper). WA-023: Ultralisk can now roll ChitinousPlating (no longer a free start upgrade). WA-044: Transfusion pulled from the rollable pool entirely for V1 (bio-only gate makes it a bad roll) — Sentry/Medic no longer roll it; Queen keeps its native (bio-only) Transfusion._
