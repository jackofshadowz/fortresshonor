---
type: data-schema
title: Unit & Enemy Roster
description: The full cast — player unit lines, hero roster, enemy families, and bosses — recovered from the UnitType enum plus prefab and animation-clip names.
resource: app://com.ttt.fortressmerge/data/roster
tags: [units, enemies, bosses, heroes, roster]
timestamp: 2026-06-21T00:00:00Z
---

# Player Units (merge lines)

* **Melee:** `Peasant → Warrior → Knight` (+ `Melee3D`, `BasicMeleeUnit2D/3D`)
* **Ranged:** `Bowman → Archer → Marksman` (+ `Range3D`, `BasicRangeUnit2D/3D`, `RoyalArcherUnit2D`)
* **Mage:** `BasicMageRangeUnit2D` (4 deck ranks: `UIMage1-4DeckBuildingPrefab`)
* **Cavalry:** `Horseman → Chevalier`
* **Misc/economy:** `Farmer`
* **Summoned allies:** `AllyMage`, `AllySkeleton`

# Heroes

Baker (Croissant Baker), Berserker, Commander, Electric Goddess, Elf Archer, Esmeralda, Paladin, Undertaker, King. Each: `Hero*MeleeUnit2D` prefab + `UIHero*Deck` + animation set. See [Hero System](/systems/hero-system.md).

# Enemies (regular)

Warrior, Archer, Mage, Horseman, Skeleton, Slime (`SlimeMelee`, `FireSlime`, slime range), Flying Monster, plus four faction families with full class kits (warrior/archer/horseman/mage each):

* **Zombies:** `ZombieWarrior`, `ZombieArcher`, `ZombieHorsemen`, `ZombieMage`
* **Cursed:** `CursedWarrior`, `CursedArcher`, `CursedHorsemen`, `CursedMage`
* **Orcs:** `OrcWarrior`, `OrcArcher`, `OrcHorsemen`, `OrcMage`
* Training: `DummyWarriorEnemy`

# Bosses

* **Melee/Giant:** `Melee2DGiantBoss` (Attack/Run/Death/Idle/SpecialAttack anim sets)
* **Golem family:** `GreenGolemBoss`, `GreyGolemBoss`, `SandGolemBoss`, `RedGolemBoss`, `BlueGolemBoss` (a.k.a. Rock/Ice/Lava/Obsidian/Electric — `Boss*GolemUnit2D`)
* **Slime:** `SlimeBoss`
* **Dragon:** `DragonBoss`
* **Demon:** `DemonBoss`
* **Medusa**, **Reaper**, **Horseman** bosses
* **Mimic chests:** `BossGoldMimic`, `BossGoldPurpleMimic`, `BossGoldRedMimic`, `BossIronBrownMimic`, `BossIronPurpleMimic` (treasure-chest enemies)

Bosses share a 4-state animation contract (`Attack`, `Run`, `Death`, `SpecialAttack`, some `Idle`) and a telegraphed alert (`BossAlertPrefab`).

# Design Notes for Reimplementation

* Define units as data rows keyed by `UnitType`; the merge line is `(family, tierIndex)`.
* Enemy **families share a kit** (warrior/archer/horseman/mage archetypes reskinned per faction) — one behavior set, many skins. Cheap roster expansion; copy this pattern.
* Bosses = an enemy with a `SpecialAttack` + telegraph + healthbar; a single boss controller parameterized by data covers all of them.

# Citations

[1] `UnitType` enum (`dump.cs`), prefab names, and animation-clip names from the Unity asset graph. See [methodology](/references/methodology.md).
