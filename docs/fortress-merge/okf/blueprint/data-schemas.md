---
type: blueprint
title: Data Schemas
description: Concrete JSON schemas for the data-driven content — cards, perks, buffs, waves, loot tables, and the central config — ready to author against.
resource: app://fortress-merge-clone/schemas
tags: [blueprint, schema, json, data-driven]
timestamp: 2026-06-21T00:00:00Z
---

# Schema

Authoring formats for the reimplementation. Values shown are illustrative placeholders — the point is the *shape*, which mirrors the source's data model.

## card.json (units / buildings / heroes)
```json
{
  "id": "warrior",
  "category": "Unit",            // Unit | Building | Hero
  "line": "melee",               // merge ladder family
  "tier": 1,                     // position in ladder (merge increments)
  "rarity": "Common",            // Common..Mythic
  "displayName": "Warrior",
  "stats": { "hp": 100, "damage": 15, "attackSpeed": 1.0, "range": 1.5, "moveSpeed": 2.0 },
  "passives": [ { "effect": "none", "value": 0 } ],
  "upgrade": { "coins": 50, "gems": 0, "shards": 0, "maxLevel": 20 },
  "mergeInto": "knight"          // null if top of ladder
}
```

## perk.json (CPEffect equivalent)
```json
{
  "id": "crit_chance",
  "effect": "CritChanceEffect",  // maps to an ICardPassiveEffect implementation
  "rarity": "Uncommon",
  "set": null,                    // e.g. "Wardrum" for synergy clusters
  "displayName": "Sharpened Blades",
  "description": "+{value}% crit chance",
  "value": 0.10,                  // magnitude (the data-driven number)
  "stacks": true
}
```

## buff.json (BuffTemplate, keyed by BuffId family)
```json
{
  "id": "Burn",                  // 200-range = DoT
  "category": "dot",
  "magnitude": 5,                // per tick
  "duration": 3.0,
  "tickInterval": 0.5,
  "stackRule": "refresh"         // refresh | stack | ignore
}
```

## wave.json
```json
{
  "mode": "campaign",
  "level": 1,
  "baseHp": 100,
  "waves": [
    { "index": 1, "spawns": [ { "enemy": "orc_warrior", "count": 5, "delay": 0.8 } ] },
    { "index": 2, "spawns": [ { "enemy": "orc_warrior", "count": 6, "delay": 0.7 },
                              { "enemy": "orc_archer",  "count": 2, "delay": 1.2 } ] }
  ],
  "rewards": [ { "type": "Resource", "resource": "Coins", "amount": 120 } ]
}
```

## loot_table.json (chests)
```json
{
  "id": "common_card_chest",
  "key": "CommonChestKeys",
  "rollsByRarity": { "Common": 0.70, "Uncommon": 0.22, "Rare": 0.07, "Epic": 0.01 },
  "rigged": [ "warrior", "archer" ]   // deterministic first-pull contents (FTUE)
}
```

## config.json (remote-config equivalent — all balance lives here)
```json
{
  "interstitial_starts_from_level": 3,
  "difficulty_offset_start_level": 0,
  "fail_offer_enabled": false,
  "can_buy_hero_with_gems": true,
  "population_cap_base": 12,
  "spawn_interval_seconds": 2.0,
  "merge_gold_reward": 5
}
```

# Notes

* Keep **definitions** (above) separate from **save state** (owned counts, levels, currencies).
* Reference the recovered enums in [Enums](/data-model/enums.md) for valid values (`category`, `rarity`, `resource`, `BuffId` families).
* Author content as data so a non-programmer (or a balance script) can tune without recompiling — the core lesson of the teardown.

# Citations

[1] Shapes derived from `CardData`/`CardSettings`, `CardPassiveValues`, `ChestSettings`, `BuffTemplate`, wave/`EnemySpawnManager`. See [Data Model](/data-model/).
