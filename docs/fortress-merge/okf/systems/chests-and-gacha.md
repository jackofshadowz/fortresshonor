---
type: game-mechanic
title: Chests & Gacha
description: Chest/banner randomized reward systems with tiered chests, keys, and explicitly "rigged" deterministic contents for onboarding.
resource: app://com.ttt.fortressmerge/systems/chests
tags: [gacha, chests, rng, monetization, ftue]
timestamp: 2026-06-21T00:00:00Z
---

# Concept

Randomized reward acquisition via **chests** and the **summon banner**. Defined by `ChestSettings`:

```
ChestSettings {
  List<CardChestData> CardChests;
  List<GearChestData> GearChests;
  List<CardSettings> cardsForRiggedchests;   // deterministic contents
}
```

# Chest Tiers (from remote-config keys)

Card chests (`cards_chest_x10_opened` — a 10-pull), and gear/shard chests in three tiers each: `big/medium/legendary_gear_chest`, `big/medium/legendary_shard_chest`. Keys gate opens: `EpicChestKeys`, `CommonChestKeys`; bag keys `CommonBagKey`/`EpicBagKey`.

# Rigged / Deterministic Contents

`cardsForRiggedchests` is notable: the game ships a **deterministic chest path** — early/targeted chest pulls return predetermined cards rather than pure RNG. This is a standard onboarding (FTUE) and pity/retention technique: the first pulls feel great by design. Worth understanding for the report's monetization-ethics discussion.

# Summon Banner

The banner building (`UnitsBanner`, `BannerBuildingSettings.PriceDecrease`) is a gacha-style unit summon; `IncreaseSummoningPoolCPEffect` enlarges the in-run pool; rerolls via `BannerRerollCostReductionCPEffect`.

# Rarity Weights

Drops are weighted by `Rarity` (Common→Mythic) via `CardsSystemSettings.RaritySettings` (`List<RaritySettings>`). Exact weights are server-tunable.

# Design Notes for Reimplementation

* Separate **chest definition** (loot table by rarity weight) from **chest instance** (keys required, cost).
* If teaching monetization design, the `cardsForRiggedchests` mechanism is a concrete case study in scripted "luck" — implement transparently and discuss player-fairness.
* Pity timers / deterministic first-pulls dramatically improve early retention; make the rule explicit and data-driven.

# Citations

[1] `ChestSettings`, `CardsSystemSettings.RaritySettings`, chest remote-config keys from `dump.cs` + `stringliteral.json`. See [methodology](/references/methodology.md).
