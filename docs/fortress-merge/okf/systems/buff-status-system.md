---
type: game-mechanic
title: Buff / Status Effect System
description: A typed status-effect system (BuffId) covering crowd control, damage-over-time, and offensive/defensive buffs, applied to units and enemies.
resource: app://com.ttt.fortressmerge/systems/buffs
tags: [buffs, status-effects, crowd-control, dot, BuffId]
timestamp: 2026-06-21T00:00:00Z
---

# Concept

A general status-effect framework keyed by the `BuffId` enum, driven by `BuffGlobalSettings` (`Dictionary<BuffId, BuffTemplate>`) and `BuffTemplate` definitions. Buffs are applied by units, perks, heroes, and enemies. The numeric ID ranges are grouped by category (the tens/hundreds digit encodes the family).

# BuffId Catalog (recovered enum)

## Crowd Control (100s)
* `Slow` (100), `Stun` (101), `Freeze` (102), `Glue` (103)

## Damage over Time (200s)
* `Burn` (200), `Poison` (201), `Bleed` (202)

## Offensive Buffs (300s)
* `AtkDmgBuff` (300), `AttackSpeedBuff` (301), `CriticalBuff` (303), `DrunkBuff` (304)

## Defensive Buffs (400s)
* `HealOverTime` (400), `Shield` (401), `Invincibility` (402)

## Mobility (500s)
* `SpeedBuff` (500), `Sprint` (501)

## Range / Area (600s)
* `RangeBuff` (600), `AOERadiusBuff` (601), `AOEDamageBuff` (602)

# Relationship to Perks and Heroes

Many [perks](perk-system.md) and hero abilities apply these buffs (e.g., `BashChanceCPEffect` → `Stun`; `WardrumHealOverTimeCPEffect` → `HealOverTime`; `WardrumEnemyKnockbackCPEffect` → knockback). Bosses apply CC/DoT to the player's units. The `DrunkBuff` ties to the `2DWarriorDrunkAnimation` / Baker hero theme.

# Design Notes for Reimplementation

* Model a buff as `{ id, magnitude, duration, stacking-rule, tick-interval }` in a `BuffTemplate` table keyed by an enum — exactly as the source does.
* Use **numeric ID ranges by family** (CC / DoT / offense / defense / mobility / area) so new buffs slot in without enum churn and tooling can group them.
* A single status engine that both player units and enemies share keeps combat code small and symmetric.

# Citations

[1] `BuffId` enum and `BuffGlobalSettings`/`BuffTemplate` from `dump.cs`. See [methodology](/references/methodology.md).
