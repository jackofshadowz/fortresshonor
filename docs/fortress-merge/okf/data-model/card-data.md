---
type: data-schema
title: Card Data Abstraction
description: The unified "card" abstraction that represents units, buildings, and heroes with a shared upgrade economy, passives, rarity, and level gating.
resource: app://com.ttt.fortressmerge/data/card
tags: [card, schema, upgrade, passives, data-model]
timestamp: 2026-06-21T00:00:00Z
---

# Concept

Units, buildings, and heroes are unified under a **Card** abstraction (`CardType: Unit/Building/Hero`). A `CardSettings` (designer data, a ScriptableObject-style definition) pairs with a runtime `CardData` (player-owned state).

# Schema (recovered)

## CardSettings (definition)
```
CardType CardType
int SpecificType            // index within the type (which unit/building/hero)
Sprite MainSpriteForCard
string DisplayName
string Description
Rarity Rarity
int LevelUnlocked
List<CardPassiveValues> Passives
int Level / MaxLevel / IsMaxed / IsUnlocked
```

## CardData (runtime/owned)
```
CardType CardType
int SpecificType
int Level
LNum Amount                 // owned count (big-number)
CardSettings Settings
List<CardPassiveValues> ActivePassives
LNum CoinsPriceUpgrade
LNum GemsPriceUpgrade
LNum ShardsPriceUpgrade
bool UsingGemsToPay
bool HasEnoughShardToUpgrade
bool HasUpgradeReady
```

## CardPassiveValues
```
CardPassiveSetting cardPassiveSetting   // which passive
float value                             // magnitude (server-tunable)
```

## Supporting
`CardStatsSettings { List<CardStatData> cardStats }`, `CardsSystemSettings { List<RaritySettings> RaritySettings }`, `CardChestData`, `CardSaveData`.

# Key Observations

* **`LNum`** = a large-number type for idle-style exponential scaling of owned counts and upgrade prices.
* **Passives carry their magnitude as `float value`** — the link between a card and a [perk/buff](/systems/perk-system.md) effect; this is why specific numbers are data, not code.
* The same upgrade economy (Coins/Gems/Shards) spans all card types; heroes add gear/blueprints on top (see [Hero System](/systems/hero-system.md)).

# Design Notes for Reimplementation

* Model one `Card` type with a `category` discriminator; share the upgrade/level economy; specialize via composition (gear for heroes, spawn-rate for buildings).
* Keep `CardSettings` as data (JSON/ScriptableObject), `CardData` as save state. Clean separation = moddable balance.

# Citations

[1] `CardData`, `CardSettings`, `CardPassiveValues`, `CardStatsSettings`, `CardsSystemSettings` from `dump.cs`. See [methodology](/references/methodology.md).
