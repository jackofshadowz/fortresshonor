---
type: data-schema
title: Core Enums
description: The recovered C# enumerations that define rarity, tier, unit/enemy types, resources, reward types, card types, buffs, and game mode.
resource: app://com.ttt.fortressmerge/data/enums
tags: [enums, schema, data-model]
timestamp: 2026-06-21T00:00:00Z
---

# Schema

Verbatim enum members recovered from `dump.cs` (IL2CPP metadata v31). Numeric values shown where meaningful.

## Rarity
`Common(0), Uncommon(1), Rare(2), Epic(3), Legendary(4), Mythic(5)`

## Tier
`Normal(0), S(1), SS(2), SSS(3)`

## CardType
`Unit(0), Building(1), Hero(2)`

## GameMode
`Home(0), Gameplay(1)` — top-level app state (menu vs in-run). Distinct from the *play modes* in [Game Modes](/systems/game-modes.md).

## ResourceType
`Tokens(0), Gems(1), XP(2), Coins(3), SkipIt(4),`
`BlueprintHead(100), BlueprintChest(101), BlueprintBoots(102), BlueprintRing(103), BlueprintHero(110),`
`EpicChestKeys(200), CommonChestKeys(201),`
`EndlessModeTicket(301), BossFightModeTicket(302), HeroBattleModeTicket(303),`
`CommonBagKey(401), EpicBagKey(402),`
`Pumpkin(801), ValentineGiftBox(802), EasterEgg(803), TrampolineCoin(804)`

## RewardType
`None(0), Resource(1), Gear(2), SpeedUp(3), Unit(4), NoAds(5), RandomGear(6), RandomBlueprint(7), Blessing(8), RandomShardContent(9), RandomGearContent(10), GooglePlayPass(11), DiscountedBundle(12), BlessingLimitedTime(13), InfiniteSkipItTicket(14)`

## BuffId
CC: `Slow(100), Stun(101), Freeze(102), Glue(103)`
DoT: `Burn(200), Poison(201), Bleed(202)`
Offense: `AtkDmgBuff(300), AttackSpeedBuff(301), CriticalBuff(303), DrunkBuff(304)`
Defense: `HealOverTime(400), Shield(401), Invincibility(402)`
Mobility: `SpeedBuff(500), Sprint(501)`
Area: `RangeBuff(600), AOERadiusBuff(601), AOEDamageBuff(602)`

## UnitType (abridged — full roster in [unit-roster](unit-roster.md))
Player lines: `Peasant(2)→Warrior(6)→Knight(9)`, `Bowman(4)→Archer(7)→Marksman(10)`, `Horseman(8)→Chevalier(11)`, plus `Melee3D(1)`, `Range3D(3)`, `Farmer(5)`.
Enemies/bosses: slimes, skeletons, golems (Green/Grey/Sand/Red/Blue), zombies, cursed, orcs, `DragonBoss(28)`, `DemonBoss(29)`, allies (`AllyMage`, `AllySkeleton`), etc.

# Notes

* Numeric **ID ranges encode families** (e.g., ResourceType 100s = blueprints, 300s = mode tickets, 800s = event currencies; BuffId 100s = CC, 200s = DoT). Preserve this convention — it makes the data self-documenting and tooling-friendly.

# Citations

[1] `dump.cs` enum definitions, Il2CppDumper, metadata v31. See [methodology](/references/methodology.md).
