---
type: game-mechanic
title: Hero System
description: Collectible hero units with decks, gear/blueprints, level/unlock progression, and a revive-for-cost in-run economy; the primary long-term and monetization hook.
resource: app://com.ttt.fortressmerge/systems/heroes
tags: [heroes, collection, gear, progression, deckbuilding]
timestamp: 2026-06-21T00:00:00Z
---

# Concept

Heroes are powerful special units the player collects, upgrades, and brings into runs. They are the meta-progression spine and the premium monetization anchor (`can_buy_hero_with_gems` is a remote-config flag). A hero is a `CardType.Hero` (see [Card Data](/data-model/card-data.md)).

# Roster

Baker (a.k.a. Croissant/CrossaintHeroBaker), Berserker, Commander, Electric Goddess, Elf Archer, Esmeralda, Paladin, Undertaker, King — represented as `Hero*MeleeUnit2D` prefabs and `UIHero*Deck` deck cards. Each has cosmetic flourishes (e.g., surfing particles for Claire/Elfe/Emile/Paladin).

# Progression & Customization

* **Unlock/level gating** — `LockedHero`, `HeroMaxedText`, `LevelUnlocked`, `MaxLevel`.
* **Gear & blueprints** — `HeroTabIGearsnventoryCategory` (gear inventory), `HeroTabBlueprintsCategory`; gear blueprint resources: `BlueprintHead/Chest/Boots/Ring` and `BlueprintHero` (see [Economy](economy-currencies.md)). Gear slots imply equippable stat items.
* **Tabs/UI** — `HeroTab`, `HeroesPage`, `HeroInventory`, `HeroStats`, `UpdateHeroTabPopup`, `HeroTabNotificationsController`.

# In-Run Hero Mechanics

* Heroes can **die and be revived for a cost** mid-run: `ButtonHeroRevivePrefab`, with perks `FreeHeroReviveCPEffect` and `HeroReviveCostReductionCPEffect`.
* Hero healing: `FountainHealHeroBonusCPEffect`.
* Dedicated **Hero Battle** mode uses a build-phase/fight-phase deck flow (see [Game Modes](game-modes.md)).

# Design Notes for Reimplementation

* Treat heroes as a *card* specialization sharing the `CardData` upgrade economy (Coins/Gems/Shards) but adding gear slots + blueprints.
* The **revive-cost** loop is a soft monetization pressure valve in-run — keep it tunable.
* Gear blueprints (shard-collect → craft) are a classic retention sink; one resource per slot type.

# Citations

[1] Hero prefabs, `HeroTab*`, blueprint `ResourceType` members, revive perks from asset graph + `dump.cs`. See [methodology](/references/methodology.md).
