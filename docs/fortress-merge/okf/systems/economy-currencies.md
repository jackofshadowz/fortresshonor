---
type: game-economy
title: Economy & Currencies
description: The full resource set (ResourceType) plus the card-upgrade economy (Coins/Gems/Shards with big-number scaling) and its sources and sinks.
resource: app://com.ttt.fortressmerge/systems/economy
tags: [economy, currencies, monetization, sinks, sources]
timestamp: 2026-06-21T00:00:00Z
---

# Currencies & Resources (`ResourceType` enum)

* **Coins** (3) — primary soft currency; in-run + production. Upgrade card levels.
* **Gems** (1) — premium currency; buy heroes, skips, rerolls, time-savers.
* **XP** (2) — account/card progression.
* **Tokens** (0) — meta/event currency (`TokenGainBonusCPEffect`).
* **SkipIt** (4) — skip/fast-forward tickets (reward `InfiniteSkipItTicket`).
* **Shards** — card upgrade material (`ShardsPriceUpgrade`, `HasEnoughShardToUpgrade`); gear/shard chests (`big/medium/legendary_shard_chest`).
* **Gear Blueprints** — `BlueprintHead` (100), `BlueprintChest` (101), `BlueprintBoots` (102), `BlueprintRing` (103), `BlueprintHero` (110).
* **Chest keys** — `EpicChestKeys` (200), `CommonChestKeys` (201).
* **Mode tickets** — `EndlessModeTicket` (301), `BossFightModeTicket` (302), `HeroBattleModeTicket` (303).
* **Bag keys** — `CommonBagKey` (401), `EpicBagKey` (402).
* **Event currencies** — `Pumpkin` (801), `ValentineGiftBox` (802), `EasterEgg` (803), `TrampolineCoin` (804). See [LiveOps & Events](liveops-events.md).

# Card Upgrade Economy

Cards (units/buildings/heroes) upgrade via `CardData`: `CoinsPriceUpgrade`, `GemsPriceUpgrade`, `ShardsPriceUpgrade`, with `UsingGemsToPay` and `HasUpgradeReady` gates. Values use **`LNum`** — a large-number type (idle-game style) enabling exponential cost/power scaling across many levels.

# Sources (where currency comes from)

In-run gold (production buildings, `GainGoldOnMergeCPEffect`, `OnTreeGoldCPEffect`, `DoubleGoldProductionCPEffect`), level/wave clear rewards, chests, daily login (`DailyLoginRewardSettings.rewards[]`), daily missions, ad rewards (`RVManager` / rewarded video), and IAP.

# Sinks (where currency goes)

Card upgrades (Coins/Gems/Shards), hero purchase/revive, banner summons & rerolls, chest keys, mode tickets, skips, gear crafting from blueprints.

# Monetization Surface

Rewarded video everywhere (`Image_NoAds`, ad on/off), IAP + subscriptions (`IAPManager`, intro-price flags), time-limited & discounted bundles (`RewardType.DiscountedBundle`, `TimeLimitedOffersManager`), fail-offers. Pricing/pacing is remote-config driven — see [Remote Config Levers](/references/remote-config.md).

# Design Notes for Reimplementation

* Two-currency minimum (soft **Coins**, premium **Gems**) covers MVP; add **Shards** + **Blueprints** only when you build the gear/hero meta.
* Use a **big-number type** (`LNum` equivalent) from day one if you want idle-style exponential progression — retrofitting is painful.
* Keep every price/rate in a remote-tunable config table, not in code (the source does exactly this).

# Citations

[1] `ResourceType` enum, `CardData` price properties, `LNum`, `DailyLoginRewardSettings` from `dump.cs`. See [methodology](/references/methodology.md).
