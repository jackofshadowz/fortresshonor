---
type: game-structure
title: Game Modes
description: Four gameplay modes built on one combat core — campaign, endless, boss fight, and a two-phase hero-battle deckbuilder — each with its own manager and ticket currency.
resource: app://com.ttt.fortressmerge/systems/modes
tags: [modes, campaign, endless, boss, hero-battle]
timestamp: 2026-06-21T00:00:00Z
---

# Concept

All modes reuse the [merge](merge-system.md) + [combat/wave](combat-and-waves.md) core; each is a `RoundManager` subclass with a matching `EnvironmentManager` and entry button loader. Modes are gated by ticket resources (see [Economy](economy-currencies.md)).

# Modes

## Campaign / Main (`RoundManager`)
Leveled stage progression (`main_level`, `current_level`, `Loading Screen LVL1`, themed backgrounds like `BackgroundLevelOrcs`). The default progression spine; difficulty ramps per level (`difficulty_offset_start_level`).

## Endless (`EndlessModeRoundManager` + `EndlessModePBManager`)
Survival / personal-best chase ("PB" = personal best). Gated by `EndlessModeTicket`. Leaderboard tie-in (`Ranking`, `RankText`).

## Boss Fight (`BossFightModeRoundManager` + `BossFightModeEnvironmentManager`)
Dedicated boss encounters with a progression bar (`BossModeProgressionBarPrefab`, `BossModeEndGameScreen`). Gated by `BossFightModeTicket`. Boss roster: Demon, Dragon, Medusa, Reaper, Slime, Golem family, Mimic chests.

## Hero Battle (`HeroBattleModeRoundManager`)
A **two-phase deckbuilder**: a build phase then a fight phase (`HeroBattleModeBuildPhaseCamera` / `...FightPhaseCamera`, `HeroBattleChoosePopup`, `HeroBattleEmptyHeroSlot`, `HeroBattleEndGameScreen`). Gated by `HeroBattleModeTicket`. Deck-driven around the hero roster.

## TLE variants
Each mode has a Time-Limited-Event reskin (`TLEEndlessMode*`, `TLEBossFight*`, `TLEHeroBattle*`). See [LiveOps & Events](liveops-events.md).

# Meta Screens

`MenuSceneManager`, `NavigationManager`/`NavigableScenesManager`, the AFK Village idle screen (`AfkVillageTouchManager`, `AfkVillageSettings.villageUpgrade`, ~40 `AFKVillageUnitPrefab`), and an enemy bestiary (`CollectionEnemies`, `EnemiesCollectionButton`).

# Design Notes for Reimplementation

* Build **one** combat core and a `RoundManager` base; modes are configuration + a thin subclass. This is the game's biggest content-efficiency lever.
* MVP = Campaign only. Endless/Boss/Hero-Battle are post-MVP multipliers; the ticket economy throttles their play and creates sinks.
* The AFK village is an optional idle/retention layer — defer past MVP.

# Citations

[1] `*RoundManager`/`*EnvironmentManager`/`*ButtonLoader` classes, mode tickets, hero-battle cameras from `dump.cs` + asset graph. See [methodology](/references/methodology.md).
