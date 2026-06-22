---
type: game-mechanic
title: Perk System (CPEffect)
description: A roguelite draft layer of ~40 stackable run modifiers (CardPassiveEffect / CPEffect) chosen between rounds, modifying offense, economy, and spawns.
resource: app://com.ttt.fortressmerge/systems/perks
tags: [roguelite, perks, draft, cpeffect, modifiers]
timestamp: 2026-06-21T00:00:00Z
---

# Concept

Between rounds the player drafts **perks** — small stackable modifiers implemented as classes ending in `CPEffect` and all implementing the `ICardPassiveEffect` interface. Each perk runs through a uniform contract:

```
void ExecuteEffect(float value, PassiveContext context)
```

The **magnitude is data-driven** (`value` comes from `CardPassiveValues { CardPassiveSetting cardPassiveSetting; float value; }`), which is why specific numbers are server-tunable rather than hardcoded. The UI: `ShowHidePerksButtonController`, `GameScenePassives`, `UnitModifierGroup`.

# Perk Catalog (recovered class names)

## Offense
* `CritChanceCPEffect` — crit chance
* `AOERadiusIncreaseCPEffect` — area radius
* `AtkRangeIncreaseCPEffect` — attack range
* `CooldownReductionCPEffect` — attack cooldown
* `DoubleProjectileCPEffect`, `DoubleLightningCPEffect` — extra projectiles/bolts
* `ExplosionDamageBonusCPEffect` — explosion damage
* `IncreaseFireRadiusCPEffect`, `IncreaseFireDurationCPEffect` — fire DoT shaping
* `BashChanceCPEffect` — bash (stun-on-hit) chance
* `BounceCountBonusCPEffect` — projectile bounces
* `ThornCPEffect` — reflect damage
* `LifestealCPEffect` — heal on damage
* `ThrowPerCooldownIncreaseCPEffect` — throw rate
* `HPIncreaseCPEffect` — unit HP
* `UnitSpeedIncreaseCPEffect` — movement/attack speed

## Economy / Tempo
* `GainGoldOnMergeCPEffect`, `OnTreeGoldCPEffect`, `DoubleGoldProductionCPEffect` — gold income
* `OnMergeFreeRerollCPEffect`, `BannerRerollCostReductionCPEffect` — reroll economy
* `OnMergeFreeTreeCPEffect` — free unit on merge
* `AddResellCostCPEffect`, `IncreaseStartingResellValueCPEffect` — resell value
* `IncreaseSummoningPoolCPEffect` — summon pool size
* `MaxPopIncreaseCPEffect` — board population cap
* `DuplicationIncreaseCPEffect` — duplicate chance
* `XPGainBonusCPEffect`, `TokenGainBonusCPEffect` — meta-currency gain

## Spawn
* `SpawnTwoUnitsChanceCPEffect`, `IncreaseDoubleSpawnChance`, `IncreaseTripleSpawnChance`

## Hero / Support
* `FreeHeroReviveCPEffect`, `HeroReviveCostReductionCPEffect`
* `FountainHealHeroBonusCPEffect`, `FountainHealBuildingCPEffect`
* `SupplyCooldownReductionBonusCPEffect`, `SupplyProjectileBonusCPEffect`

## "Wardrum" synergy set (themed cluster)
* `WardrumAttackSpeedCPEffect`, `WardrumEnemyKnockbackCPEffect`, `WardrumHealOverTimeCPEffect`

# Design Notes for Reimplementation

* Implement perks as a **uniform interface** (`apply(value, context)`); store each perk's magnitude(s) and rarity as data, not code. This is the single most reusable architectural idea in the game.
* Group perks into themed sets (e.g., "Wardrum") to enable synergy builds and clearer draft choices.
* Draft 3-of-N each round; weight by rarity (see [Enums](/data-model/enums.md) `Rarity`).
* Most perks are tiny stat deltas — this is how the game gets huge build variety with almost no new art.

# Citations

[1] `*CPEffect` classes, `ICardPassiveEffect`, `CardPassiveValues` from `dump.cs`. See [methodology](/references/methodology.md).
