# Upgrade pools by unit

Every unit's **rollable upgrade pool**, derived from the eligibility tags in `upgradeInitializers.galaxy` (see `docs/upgrade-eligibility-tags.md` for the rules). Snapshot 2026-07-15 — regenerate after changing pools or tags. `[n]` = number of eligible upgrades.

**Unit combinations (units only): 290,304** = 2·3·4·3 · 3·7·3 · 4·4·4 across the 10 slots.

```
BARRACKS
 s1  Zergling      [5]  Blink, HotSRaptorCharge2, Stimpack, zerglingattackspeed, zerglingmovementspeed
 s1  Zealot        [5]  Blink, HotSRaptorCharge2, Charge, Stimpack, zerglingattackspeed
 s2  Hydralisk     [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s2  Marine        [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s2  Queen        [10]  CorsairMPDisruptionWeb, ForceField, GuardianShield, GravitonBeam, MissilePods, Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s3  Firebat       [5]  Blink, HotSRaptorCharge2, Speed, Stimpack, RavagerCorrosiveBile
 s3  Marauder      [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s3  Sentry        [4]  CorsairMPDisruptionWeb, GravitonBeam, MissilePods, Transfusion
 s3  Medic         [7]  CorsairMPDisruptionWeb, ForceField, GuardianShield, GravitonBeam, MissilePods, Transfusion, Hyperjump
 s4  Ghost        [17]  ArbiterMPRecall, BlindingCloud, BuildAutoTurret, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, MissilePods, NeuralParasite, ParasiticBomb, RavenScramblerMissile, SeekerMissile, Yoink, Blink, Range, Speed, Stimpack, PersonalCloaking
 s4  Infestor     [10]  ArbiterMPRecall, BlindingCloud, BuildAutoTurret, CorsairMPDisruptionWeb, Irradiate, MissilePods, ParasiticBomb, RavenScramblerMissile, SeekerMissile, Yoink
 s4  HighTemplar  [12]  ArbiterMPRecall, BlindingCloud, BuildAutoTurret, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, MissilePods, NeuralParasite, ParasiticBomb, RavenScramblerMissile, SeekerMissile, Yoink
FACTORY
 s1  Vulture       [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s1  Hellion       [6]  Blink, Speed, Stimpack, RavagerCorrosiveBile, HighCapacityBarrels, TwinLinkedFlameThrowers
 s1  Stalker       [6]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, stalkerblinkcooldown
 s2  Diamondback   [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s2  Immortal      [6]  Blink, D8Charge, Range, Speed, Stimpack, RavagerCorrosiveBile
 s2  SiegeTank     [5]  Blink, DiggingClaws, Stimpack, RavagerCorrosiveBile, SiegeTankRange
 s2  WarHound      [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s2  Archon        [4]  Blink, Range, Speed, RavagerCorrosiveBile
 s2  LurkerMP      [5]  Blink, LurkerRange, DiggingClaws, Stimpack, RavagerCorrosiveBile
 s2  Goliath       [5]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile
 s3  ThorAP        [5]  Blink, Range, Speed, Stimpack, Yamato
 s3  Ultralisk     [6]  Blink, HotSRaptorCharge2, Range, Charge, Stimpack, Yamato
 s3  Colossus      [6]  Blink, Range, Speed, Stimpack, Yamato, Hyperjump
STARPORT
 s1  CorsairMP    [11]  ForceField, GuardianShield, GravitonBeam, MissilePods, Transfusion, Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump
 s1  Phoenix      [11]  CorsairMPDisruptionWeb, ForceField, GuardianShield, MissilePods, Transfusion, Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump
 s1  Wraith       [13]  CorsairMPDisruptionWeb, ForceField, GuardianShield, GravitonBeam, MissilePods, Transfusion, Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, WraithCloak, Hyperjump
 s1  VikingFighter [6]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump
 s2  Liberator     [4]  Blink, Speed, Stimpack, RavagerCorrosiveBile
 s2  Mutalisk      [6]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, Hyperjump
 s2  DuskWing      [8]  Blink, Range, Speed, Stimpack, RavagerCorrosiveBile, ClusterWarheads, BansheeCloak, Hyperjump
 s2  VoidRay       [4]  Blink, Range, Speed, RavagerCorrosiveBile
 s3  Raven        [10]  ArbiterMPRecall, BlindingCloud, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, MissilePods, NeuralParasite, ParasiticBomb, SeekerMissile, Yoink
 s3  Tempest       [5]  Blink, Stimpack, Yamato, TempestRange, Hyperjump
 s3  Viper         [9]  ArbiterMPRecall, BuildAutoTurret, CorsairMPDisruptionWeb, FungalGrowth, Irradiate, MissilePods, NeuralParasite, RavenScramblerMissile, SeekerMissile
 s3  Battlecruiser [5]  Blink, Range, Speed, Stimpack, Yamato
```

_Note: reflects the state at snapshot. Recent edits (Medic ✗Hyperjump, Ghost ✗Recall) and the corrosive-bile restriction (WA-040) are not yet applied above — regenerate after those land._
