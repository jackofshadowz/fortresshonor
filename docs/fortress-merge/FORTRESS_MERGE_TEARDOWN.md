# Fortress Merge — Mechanics Teardown

**Package:** `com.ttt.fortressmerge` · **Version:** 0.7.1 · **Engine:** Unity (IL2CPP, type-trees stripped)
**Source:** APKPure base split, SHA-256 `b32b252bc4cb93005d3adec83389c88b5aeba2695acee315439e976b60512499`
**Method:** Static decompile in a sandboxed Docker container (never executed). Findings derived from MonoBehaviour class names, prefab/GameObject names, animation clips, and audio cues. **No assets or code copied** — this is design analysis only.

> Limitation: exact tuning numbers (merge costs, damage, wave tables, drop rates) are **not** in this APK. They live either in the stripped IL2CPP binary (`libil2cpp.so`, in a separate arch split we didn't pull) or are fetched at runtime via `FGRemoteConfigManager` (server-side live-ops tuning). What follows is the complete *mechanics design*, not the balance constants.

## Genre
A **merge + tower-defense auto-battler roguelite**. You merge units on a grid to build an auto-fighting army that defends your base against waves of enemies, picking roguelite perks between rounds. Heavy live-ops + hyper-casual monetization.

## Core loop
1. **Spawner buildings** generate units over time (`ArcherSpawnerBuilding`, `MageSpawnerBuilding`, `HeroSpawnBuilding`, `ArcherBuilding`, `MageBuilding`).
2. **Merge** identical units to climb tiers — `LVL1`…`LVL6`, `Level2Merge`/`Level3Merge`/`Level4Merge`, `MergeItemGroup`, `MergePossibilityPrefab`, `InfoMerge`. Buildings also merge/upgrade (`BuildingMergeLVLUPStreamFX`).
3. Units **auto-fight waves** of enemies attacking the base (`Wave`, `StartWave`/`EndWave`, `WaveCount`, `WavePreviewPanel`, `EnemySpawnManager`, `EnemiesPortalPrefab`). Tower grid: `Tower0,0`/`Tower1,0`/`Tower1,1` with L/T variants.
4. Base/fortress has health (`FountainHealBuildingCPEffect`, `HomeSizeDownCPEffect`, `BuildingDamaged`, `DamageFeedbackBuilding`).
5. **Between rounds, choose perks** (the roguelite layer — see CPEffects).
6. Win/clear → rewards → next level; or lose when base falls.

## Units (yours)
- Basic: `BasicMeleeUnit2D`, `BasicRangeUnit2D`, `BasicMageRangeUnit2D`, `BasicMeleeUnit3D`/`BasicRangeUnit3D` (3D variants), `FlyingMonsterRangeUnit2D`, `RoyalArcherUnit2D`.
- Deck building tiers: `UIArcher1-4DeckBuildingPrefab`, `UIMage1-4DeckBuildingPrefab` (4 ranks each of archer/mage lines).

## Heroes (special hero units — collection + progression)
Roster: Baker (Croissant Baker), Berserker, Commander, Electric Goddess, Elf Archer, Esmeralda, Paladin, Undertaker, King.
- Decks per hero (`UIHero*Deck`), gear inventory + blueprints (`HeroTabIGearsnventoryCategory`, `HeroTabBlueprintsCategory`), unlock/level gating (`LockedHero`, `HeroMaxedText`).
- **Revive economy**: heroes die and are revived for a cost (`ButtonHeroRevivePrefab`, `FreeHeroReviveCPEffect`, `HeroReviveCostReductionCPEffect`).
- Cosmetic surfing particles (Claire, Elfe, Emile, Paladin).

## Enemies + bosses
- Enemies: Warrior, Archer, Mage, Horseman, Skeleton, Slime (melee/range), Zombie (archer/mage), Orc (Warrior/Archer/Mage/Horseman), Cursed Archer/Mage, Flying Monster, Dummy.
- **Bestiary** meta (`CollectionEnemies`, `EnemiesCollectionButton`, `EnemiesCardFrame`).
- Bosses: Demon, Dragon, Medusa, Reaper, Slime, Melee, and **Golem family** (Rock/Ice/Lava/Obsidian/Electric) + **Mimic chests** (Gold/Iron × Brown/Purple/Red) with special attacks + alert telegraphs (`BossAlertPrefab`, `BossHealthBar`).

## Game modes
1. **Campaign / standard** (`RoundManager`) — leveled stages (`Loading Screen LVL1`, `BackgroundLevelOrcs`).
2. **Endless** (`EndlessModeRoundManager` + `EndlessModePBManager`) — survival / personal-best chase.
3. **Boss Fight** (`BossFightModeRoundManager`) — dedicated boss encounters with progression bar.
4. **Hero Battle** (`HeroBattleModeRoundManager`) — **two-phase: build phase then fight phase** (`HeroBattleModeBuildPhaseCamera` / `...FightPhaseCamera`), deck-driven.
5. **Time-Limited Events (TLE / live-ops)**: Easter, Valentine's, St. Patrick's, Undertaker, Trampoline Madness — each reskins core modes (`TLEEndlessMode*`, `TLEBossFight*`, `TLEHeroBattle*`) and pairs with limited-time offers.

## Meta systems
- **AFK Village** idle screen (`AfkVillageTouchManager`, ~40× `AFKVillageUnitPrefab`, `AFKVillageGolemPrefab`) — units mill about; idle/tap rewards.
- Hero collection/upgrade tab; enemy bestiary; ranking/leaderboard (`Ranking`, `RankText`).
- Tutorial system (`TutHand`, `HeroTutorialManager`, drag/drop tuto hands).

## Roguelite perks — the `*CPEffect` system (run modifiers, drafted mid-run)
**Offense:** `CritChanceCPEffect`, `AOERadiusIncreaseCPEffect`, `AtkRangeIncreaseCPEffect`, `CooldownReductionCPEffect`, `DoubleProjectileCPEffect`, `DoubleLightningCPEffect`, `ExplosionDamageBonusCPEffect`, `IncreaseFireRadius/DurationCPEffect`, `BashChanceCPEffect`, `BounceCountBonusCPEffect`, `ThornCPEffect`, `LifestealCPEffect`, `ThrowPerCooldownIncreaseCPEffect`, `HPIncreaseCPEffect`, `UnitSpeedIncreaseCPEffect`.
**Economy/tempo:** `GainGoldOnMergeCPEffect`, `DoubleGoldProductionCPEffect`, `OnTreeGoldCPEffect`, `OnMergeFreeRerollCPEffect`, `OnMergeFreeTreeCPEffect`, `AddResellCostCPEffect`, `IncreaseStartingResellValueCPEffect`, `BannerRerollCostReductionCPEffect`, `IncreaseSummoningPoolCPEffect`, `MaxPopIncreaseCPEffect`, `DuplicationIncreaseCPEffect`, `XPGainBonusCPEffect`, `TokenGainBonusCPEffect`.
**Spawn:** `SpawnTwoUnitsChanceCPEffect`, `IncreaseDoubleSpawnChance`, `IncreaseTripleSpawnChance`.
**Hero/support:** `FreeHeroReviveCPEffect`, `HeroReviveCostReductionCPEffect`, `FountainHealHeroBonusCPEffect`, `SupplyCooldownReductionBonusCPEffect`, `SupplyProjectileBonusCPEffect`.
**Wardrum set** (synergy theme): `WardrumAttackSpeedCPEffect`, `WardrumEnemyKnockbackCPEffect`, `WardrumHealOverTimeCPEffect`.

## Economy / currencies
- **Gold/Coins** — soft, from merging + production (`Coins2/4/5/7`, gold-on-merge perks).
- **Gems** — premium (`GemsImage`).
- **Tokens** — event/meta currency.
- **Tickets / Skip tickets** — skip waves/levels (`SkipTicketImage`, `SkipItConsumed`).
- **Summoning pool / Units Banner** — gacha-style unit acquisition (`UnitsBanner`, `IncreaseSummoningPoolCPEffect`).
- In-run levers: **resell** units, **reroll** offers, **max population** cap.

## Monetization stack (for awareness, not to copy)
- Rewarded video everywhere (`RVManager`, ads-on/off toggles, `Image_NoAds`).
- IAP + Time-Limited Offers (`IAPManager`, `TimeLimitedOffersManager`, seasonal offers).
- Ad mediation: AppLovin MAX, IronSource, AdMob/Google, Meta Audience Network, Moloco, Mintegral, Fyber. Attribution: Adjust. Ad-quality: AppHarbr. Analytics: GameAnalytics + Firebase. Server tuning: Firebase Remote Config.

## Takeaways for Fortress of Honor (design, not assets)
- The **merge-grid → auto-defend-waves** spine is the hook; perks add the roguelite replay layer.
- **One mode shippable first** (campaign), with Endless/Boss/Hero-Battle as later content multipliers off the same combat core.
- **Perks are cheap content**: each is a small stat/economy modifier — dozens of them create build variety with little new art.
- **Heroes** are the collection/progression spine and the natural premium-monetization anchor.
- Balance is **server-driven** — they tune via remote config, not app updates. Worth designing your configs as data (JSON) from day one.
