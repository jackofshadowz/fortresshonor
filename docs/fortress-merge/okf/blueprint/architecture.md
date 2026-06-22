---
type: blueprint
title: Architecture
description: An engine-agnostic component/system architecture for the reimplementation, mirroring the source's data-driven, card-unified, config-indirected design.
resource: app://fortress-merge-clone/architecture
tags: [blueprint, architecture, ecs, data-driven]
timestamp: 2026-06-21T00:00:00Z
---

# Guiding Principles (learned from the teardown)

1. **Data-driven balance** — code reads named config values; never hardcode numbers.
2. **Unified Card abstraction** — units, buildings, heroes share one type with a category discriminator.
3. **Uniform effect interface** — perks and buffs are pluggable `apply(value, context)` objects.
4. **One combat core, many modes** — modes are config + a thin manager subclass.
5. **Shared status engine** — player and enemy units use the same buff system.

# Core Modules

## Config
* `ConfigService` — loads `config.json` (and per-table JSON) at boot; exposes `get(key)`. The single source of balance. (Mirrors `FGRemoteConfigManager`, but local.)

## Data / Definitions (authored content)
* `CardDefinition` (≈ `CardSettings`): `{ id, category: Unit|Building|Hero, line, tier, rarity, displayName, sprite, passives[], stats }`
* `WaveDefinition`, `EnemyDefinition`, `PerkDefinition`, `BuffTemplate`, `LootTable` — all JSON. See [Data Schemas](data-schemas.md).

## Runtime State
* `CardInstance` (≈ `CardData`): owned count, level, computed prices.
* `RunState`: board, currency, active perks, current wave, base HP.
* `SaveState`: meta progression (unlocked cards/heroes, currencies, gear).

## Systems (the simulation)
* `BoardSystem` — grid, placement, population cap.
* `MergeSystem` — `(line,n)+(line,n) → (line,n+1)`; emits merge events (perks hook here).
* `SpawnSystem` — building timers → unit creation; honors spawn-multiplier perks.
* `CombatSystem` — targeting, attack cooldowns, projectiles, damage.
* `BuffSystem` — applies/ticks `BuffTemplate`s on any unit (shared by allies/enemies).
* `WaveSystem` — schedules enemy spawns from `WaveDefinition`; tracks wave progress + preview.
* `BaseSystem` — base HP, damage intake, fountain heal; triggers lose condition.
* `PerkSystem` — generates draft offers (rarity-weighted), applies chosen `PerkDefinition` via `ICardPassiveEffect.apply(value, ctx)`.
* `EconomySystem` — currency sources/sinks, upgrade pricing (big-number type for idle scaling).
* `RewardSystem` / `LootSystem` — resolves `RewardType`/`LootTable` (incl. deterministic "rigged" first pulls).

## Mode Layer
* `RoundManager` (base) + subclasses `Campaign`, `Endless`, `BossFight`, `HeroBattle` — each supplies wave sources, win/lose rules, and entry cost (ticket).

## Presentation (kept thin)
* Renderers/UI read state; input is drag-to-merge + perk selection + hero tap. No game logic in views.

## Telemetry (course version)
* `AnalyticsLogger` — local CSV/JSON; emit `level_start/complete/fail`, `player_lost_at_level_wave`, economy events. Drives your balancing.

# Effect/Buff Plug-in Pattern (the key reusable idea)

```
interface ICardPassiveEffect { void apply(float value, PassiveContext ctx); }
// e.g. CritChanceEffect, GainGoldOnMergeEffect, SpawnMultiplierEffect ...
// magnitude `value` comes from PerkDefinition (data), not the class.
```
Register effects by id; the draft picks definitions; the system calls `apply`. Adding a perk = a data row + (sometimes) a tiny effect class.

# Engine Suggestion

Any engine works; the architecture is engine-agnostic. Unity or Godot suit the 2D sprite + auto-battler style. Keep simulation deterministic and separable from rendering so it's testable (and so you can run headless balance sims).

# Citations

[1] Patterns mirror `CardData`/`CardSettings`, `ICardPassiveEffect`, `BuffGlobalSettings`, `*RoundManager`. See [Data Model](/data-model/) and [Systems](/systems/).
