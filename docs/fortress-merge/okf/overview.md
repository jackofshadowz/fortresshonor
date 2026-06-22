---
type: game-overview
title: Fortress Merge — Overview
description: A merge + tower-defense auto-battler roguelite where merged units auto-defend a base across waves, with a drafted-perk run layer and heavy live-ops.
resource: app://com.ttt.fortressmerge/0.7.1
tags: [overview, genre, core-loop, merge, tower-defense, roguelite]
timestamp: 2026-06-21T00:00:00Z
---

# Summary

Fortress Merge is a **merge + tower-defense auto-battler with roguelite run modifiers**, built in **Unity 6000.2 (IL2CPP)** and published by **TapNation**. The player merges identical units to climb power tiers; those units automatically fight waves of enemies attacking a home base; between rounds the player drafts perks that reshape the run. A deep meta layer (heroes, gear, chests, currencies, daily systems) and aggressive live-ops (time-limited events, A/B-tested offers) wrap the core.

# Core Loop

1. **Produce** — spawner buildings generate basic units over time. See [Building System](/systems/building-system.md).
2. **Merge** — combine identical units to ascend tiers (`Peasant→Warrior→Knight`, etc.). See [Merge System](/systems/merge-system.md).
3. **Defend** — units auto-fight incoming enemy waves; the base has health. See [Combat & Waves](/systems/combat-and-waves.md).
4. **Draft** — between rounds, pick a perk (`CPEffect`) that modifies offense, economy, or spawns. See [Perk System](/systems/perk-system.md).
5. **Progress** — clear the level → rewards → next level; or the base falls and the run ends.
6. **Meta** — spend rewards on heroes, gear, upgrades; open chests; chase daily/event goals. See [Economy](/systems/economy-currencies.md), [Heroes](/systems/hero-system.md), [Chests & Gacha](/systems/chests-and-gacha.md).

# Pillars

* **Merge-to-power** — the satisfying tactile loop of combining units; clean, legible tier ladders.
* **Auto-battle defense** — low-APM, readable combat; player decisions are *placement, merge timing, and perk choice*, not micro.
* **Roguelite variety** — a large pool of small perk modifiers (`CPEffect`) creates build diversity from little art.
* **Collection & progression** — heroes and gear give long-term goals and the premium monetization anchor.
* **Live-ops engine** — modes and events recombine the same combat core; balance is tuned server-side for continuous A/B testing.

# Modes (content multipliers off one combat core)

Campaign, **Endless** (survival/high-score), **Boss Fight**, **Hero Battle** (build-phase → fight-phase deckbuilder), plus seasonal **Time-Limited Events**. See [Game Modes](/systems/game-modes.md) and [LiveOps & Events](/systems/liveops-events.md).

# What is NOT in this bundle

Specific tuning numbers (unit stats, costs, wave tables, perk magnitudes) are **server-delivered** and were not statically recoverable. The *systems, structures, and tunable parameters* are fully documented. See [Extraction Methodology](/references/methodology.md).
