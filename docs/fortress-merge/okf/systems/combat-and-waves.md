---
type: game-mechanic
title: Combat & Wave Defense
description: Auto-resolving combat where placed units fight scripted enemy waves attacking a home base with finite health.
resource: app://com.ttt.fortressmerge/systems/combat
tags: [combat, waves, tower-defense, auto-battler, base-health]
timestamp: 2026-06-21T00:00:00Z
---

# Concept

Combat is **auto-resolving**: the player does not directly control attacks. Placed/merged units acquire targets and fight automatically while waves of enemies advance toward the base. The player's agency is in *what* is on the board (merge decisions), *perk* choices, and *hero* usage — not per-unit micro.

# Wave Structure

Enemies arrive in discrete **waves** (`Wave`, `StartWave`/`EndWave`, `WaveCount`, `WaveNumberText`, `FirstEnemySpawn`). The next wave's composition is previewed (`WavePreviewPanel`, `WavePreviewEnemyItem`) so players can plan. Enemies enter via portals (`EnemiesPortalPrefab`) and follow checkpoints (`Checkpoint Hero Target - 1..5`) toward the base. Spawning is governed by `EnemySpawnManager`.

# Base / Fortress

The defended structure has health and takes damage when enemies break through (`BuildingDamaged`, `DamageFeedbackBuilding`). Healing exists via a "fountain" (`FountainHealBuildingCPEffect`, `FountainHealHeroBonusCPEffect`). The perk `HomeSizeDownCPEffect` implies the base footprint is itself a variable (smaller = harder to hit / different hitbox tradeoff).

# Combat Stats & Mechanics (from type model)

Attack behavior: `UBasicAttack`, `AttackAnimBehaviour`, projectiles (`MageProjectile`, `RockGolemProjectile`, `MageFireBallPerkProjectile`). Tunable combat dimensions exposed via perks and the buff system:

* Damage, attack speed, range, AOE radius/damage, crit chance, cooldown.
* Special effects: bash, bounce, lifesteal, thorns, explosion, fire (radius/duration), lightning, multi-projectile.
* See [Perk System](perk-system.md) and [Buff/Status System](buff-status-system.md) for the full lever set.

# Enemies & Bosses

A broad roster (warriors, archers, mages, horsemen, skeletons, slimes, zombies, cursed, orcs, flying monsters) and bosses with telegraphed special attacks (Demon, Dragon, Medusa, Reaper, Slime, the Golem family, and Mimic chests). Each boss has `Attack/Run/Death/SpecialAttack` animation sets and an alert telegraph (`BossAlertPrefab`, `BossHealthBar`). Full list: [Unit & Enemy Roster](/data-model/unit-roster.md).

# Skip / Speed

A `SkipIt` resource and `EndlessModeTicket`-style tickets let players skip or fast-forward content (`SkipTicketImage`, `SkipItConsumed`, reward `InfiniteSkipItTicket`).

# Design Notes for Reimplementation

* Represent a wave as an ordered list of `(enemyType, count, spawnDelay)` plus a wave-level difficulty scalar.
* Auto-targeting: nearest-enemy or first-in-path is sufficient and readable; expose range/AOE as data.
* Base health + wave preview together create the core "can I survive the next wave?" tension that merge timing answers.

# Citations

[1] Prefab/animation names and `UBasicAttack` from the asset graph and IL2CPP dump. See [methodology](/references/methodology.md).
