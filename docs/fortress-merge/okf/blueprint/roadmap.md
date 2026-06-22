---
type: blueprint
title: Implementation Roadmap
description: A phased, week-oriented plan taking a team from prototype to a feature-rich clone, with deliverables and learning objectives per phase.
resource: app://fortress-merge-clone/roadmap
tags: [blueprint, roadmap, milestones, course]
timestamp: 2026-06-21T00:00:00Z
---

# Roadmap (course-oriented, ~10–12 weeks)

## Phase 0 — Foundations (week 1)
* Project setup, input (drag), a grid `BoardSystem`, `ConfigService` loading `config.json`.
* **Deliverable:** place and drag a sprite on a grid. **Learn:** data-driven config from day one.

## Phase 1 — Core loop prototype (weeks 2–3)
* `SpawnSystem`, `MergeSystem` (one 3-tier melee line), `CombatSystem` (auto-target/attack), `WaveSystem` + `BaseSystem`, win/lose, wave preview.
* **Deliverable:** survive scripted waves by merging. **Learn:** auto-battler simulation, merge tension.

## Phase 2 — The hook (weeks 4–5)
* `PerkSystem` (draft 3-of-N, ~8 perks via `ICardPassiveEffect`), `EconomySystem` (Coins), upgrade pricing.
* **Deliverable:** a roguelite run that feels different each time. **Learn:** pluggable effects, build variety, economy sources/sinks.

## Phase 3 — Depth & content (weeks 6–7)
* Extra unit lines + enemy families (shared archetype kits), buildings (bank/barricade/catapult) with merge-leveling, `BuffSystem` (CC/DoT/buffs), one boss (special attack + telegraph).
* **Deliverable:** a richer roster and a boss fight. **Learn:** content scaling via data + reskins.

## Phase 4 — Meta & modes (weeks 8–9)
* Heroes (cards + gear + revive loop), chests (`LootSystem` with rarity weights + rigged FTUE), Gems currency, Endless mode.
* **Deliverable:** collection + a second mode. **Learn:** meta progression, gacha/loot design, monetization ethics discussion.

## Phase 5 — Live-service patterns & balancing (weeks 10–11)
* Remote-config indirection (local), `AnalyticsLogger`, daily login/missions, one seasonal event (reskin + currency + reward track).
* **Deliverable:** an instrumented build + a balancing pass using `player_lost_at_level_wave` data.
* **Learn:** live-service architecture, telemetry-driven balancing, A/B concept.

## Phase 6 — Polish & presentation (week 12)
* Juice (feedback, particles, audio cues), tutorial hands, a written post-mortem comparing your build to the Fortress Merge teardown.

# Cross-Cutting Practices

* Keep the **simulation deterministic & headless-runnable** so you can auto-run balance sims.
* Every number in `config.json` / data files — none in code.
* Commit the data files as the "design surface"; treat code as the engine.

# Citations

[1] Phasing maps to [Scope & MVP](scope-and-mvp.md) and the [Systems](/systems/) docs.
