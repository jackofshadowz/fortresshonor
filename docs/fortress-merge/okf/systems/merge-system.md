---
type: game-mechanic
title: Merge System
description: Combining identical units (and buildings) to ascend discrete power tiers, the central tactile loop of the game.
resource: app://com.ttt.fortressmerge/systems/merge
tags: [merge, progression, units, tiers]
timestamp: 2026-06-21T00:00:00Z
---

# Concept

The defining interaction: drag one unit onto an identical unit to **merge** them into a single higher-tier unit. Merging is the primary way the player increases army power within a run. Buildings merge too (spawners level up via merge). Evidence: prefabs `MergeItemGroup`, `MergePossibilityPrefab`, `Level2Merge`/`Level3Merge`/`Level4Merge`, `InfoMerge`, `MergeDescription`, `BuildingMergeLVLUPStreamFX`; audio `MergeSound_1..4`; merge-related perks in [Perk System](perk-system.md).

# Merge Chains (from the `UnitType` enum)

Player units advance along typed ladders (see [Enums](/data-model/enums.md)):

* **Melee line:** `Peasant → Warrior → Knight`
* **Ranged line:** `Bowman → Archer → Marksman`
* **Cavalry line:** `Horseman → Chevalier`
* **Special/3D variants:** `Melee3D`, `Range3D`, plus `Farmer`, `AllyMage`, `AllySkeleton`.

Two orthogonal quality axes also exist:

* `Tier`: `Normal, S, SS, SSS` — a quality grade on a unit.
* `Rarity`: `Common, Uncommon, Rare, Epic, Legendary, Mythic` — card-level rarity (see [Card Data](/data-model/card-data.md)).

# Board / Placement

A grid of tower slots holds units/buildings (`Tower0,0`, `Tower1,0`, `Tower1,1`, with `L`/`T` corner/edge variants). The grid has a fixed capacity (`MaxPopIncreaseCPEffect` perk increases it), making merge-vs-keep a spatial decision.

# Related Economy Levers

* **Spawn multipliers** — `SpawnTwoUnitsChanceCPEffect`, `IncreaseDoubleSpawnChance`, `IncreaseTripleSpawnChance` increase units per spawn (more merge fodder).
* **Duplication** — `DuplicationIncreaseCPEffect`.
* **Gold on merge** — `GainGoldOnMergeCPEffect`, `OnTreeGoldCPEffect`; **free rerolls/units on merge** — `OnMergeFreeRerollCPEffect`, `OnMergeFreeTreeCPEffect`.
* **Resell** — `AddResellCostCPEffect`, `IncreaseStartingResellValueCPEffect` (sell units back for gold).

# Design Notes for Reimplementation

* Model units as `(line, tierIndex)`; a merge maps `(line, n) + (line, n) → (line, n+1)`.
* Keep ladders short and legible (3–4 steps shown here) so players read power at a glance.
* The "max population" cap is the core tension knob: it forces merging and makes board space valuable.

# Citations

[1] IL2CPP type model — `UnityType` enum, merge prefabs. See [methodology](/references/methodology.md).
