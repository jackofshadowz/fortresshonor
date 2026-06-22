---
type: game-mechanic
title: Building System
description: Placeable buildings that spawn units, defend, or provide economy; buildings level up via merge and have per-type settings.
resource: app://com.ttt.fortressmerge/systems/buildings
tags: [buildings, spawners, economy, merge]
timestamp: 2026-06-21T00:00:00Z
---

# Concept

Buildings are `CardType.Building` placeables on the board. Each type has its own settings class (recovered): `BankBuildingSettings`, `BannerBuildingSettings` (has `PriceDecrease`), `BarricadeBuildingSettings`, `CatapultBuildingSettings`, plus generic `BuildingSettings`, `BuildingLevelData`, `BuildingPoolData`, `BuildingCardSettings`, `BuildingCollectionSaveData`.

# Building Types (from settings classes + prefabs)

* **Spawners** — `ArcherSpawnerBuilding`, `MageSpawnerBuilding`, `HeroSpawnBuilding` produce units over time; `ArcherBuilding`, `MageBuilding` plus `*BuildingShadow`.
* **Banner** — the summon/gacha building (`BannerBuildingSettings.PriceDecrease`); ties to `IncreaseSummoningPoolCPEffect` and `UnitsBanner`. See [Chests & Gacha](chests-and-gacha.md).
* **Bank** — economy building (`BankBuildingSettings`) — gold storage/generation.
* **Barricade** — defensive wall (`BarricadeBuildingSettings`, `BarricadeLVL3` prefabs) — blocks/absorbs enemies.
* **Catapult** — offensive structure (`CatapultBuildingSettings`).

# Leveling

Buildings **merge to level up** (`BuildingMergeLVLUPStreamFX`, `BuildingLVLUPStreamFX`, `BuildingLevelData{level, unlocked, collected}`), mirroring the unit [merge system](merge-system.md). Higher levels improve spawn rate / effect.

# Design Notes for Reimplementation

* Use a shared `BuildingSettings` base with per-type subclasses for the few that need extra fields (banner price-decrease, bank rate, barricade HP, catapult damage).
* Spawners are the *source* side of the merge economy; their spawn interval + count are key pacing levers (and interact with spawn-multiplier perks).
* `BuildingPoolData` implies object pooling — relevant for performance with many units.

# Citations

[1] `*BuildingSettings`, `BuildingLevelData`, spawner prefabs from `dump.cs` + asset graph. See [methodology](/references/methodology.md).
