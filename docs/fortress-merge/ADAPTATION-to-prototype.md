---
type: adaptation-log
title: Adapting Fortress Merge learnings → Fortress of Honor prototype
description: What from the Fortress Merge teardown was implemented into prototype/index.html, how it maps to the source mechanics, and what to adapt next.
resource: app://fortress-of-honor/prototype
tags: [adaptation, prototype, perks, roguelite, implementation]
timestamp: 2026-06-22T00:00:00Z
---

# Context

The prototype (`prototype/index.html`) was already a strong **merge-defense** game — build→fight→clear phases, 3-connected merge to tiers 1–5, tower types (archer/cannon/frost), wave defense with a wall, gold economy, a hero with sortie, and persistent meta "mods." It structurally matches Fortress Merge's spine.

The one signature mechanic it lacked was Fortress Merge's **roguelite perk draft** (the `CPEffect` layer). That was the highest-value, lowest-risk thing to adapt, and the `clear` phase between waves was the natural seam.

# What was implemented (this change)

A **between-wave perk draft**, modeled directly on Fortress Merge's `ICardPassiveEffect` pattern:

* **Data-driven perks** — a `PERKS` array where each perk is `{ id, name, rarity, desc, apply(RUN) }`. Magnitudes live in data, not code (the teardown's core lesson). Adding a perk = one array entry.
* **Uniform apply()** — every perk mutates a run-scoped `RUN` modifier object; the combat/economy systems just read `RUN.*`. This mirrors `ICardPassiveEffect.ExecuteEffect(value, ctx)`.
* **Rarity-weighted draft** — `Common/Uncommon/Rare` with weights `5/3/1`; `rollPerks()` picks 3 distinct; a new `mode = 'perk'` shows the cards after each wave; tap to choose (`choosePerk`).
* **13 starter perks** mapped from Fortress Merge's CPEffect categories onto the prototype's actual systems:
  * Offense: Sharpened Edges (+dmg), Quick Hands (+rate), Eagle Eye (+range), Keen Aim (crit), Mortar Rounds (+splash), Frostbite (+slow), Overwhelm (dmg+rate).
  * Economy: Merchant (+kill gold), Alchemy (gold-on-merge), War Chest (+wave gold), Free Quarry (free rerolls).
  * Defense: Master Masons (+wall), Bloodstone (lifesteal → wall repair per kill).

## Source → prototype mapping

| Fortress Merge | Prototype implementation |
|---|---|
| `CPEffect` / `ICardPassiveEffect.ExecuteEffect(value,ctx)` | `PERKS[].apply(RUN)` |
| `CardPassiveValues.value` (server-tuned magnitude) | literal magnitudes in `PERKS` data |
| Rarity-weighted draft between rounds | `rollPerks()` + `mode='perk'` after `clear` |
| `GainGoldOnMergeCPEffect` | `alchemy` → `RUN.goldOnMerge` in `tryMerge()` |
| `CritChanceCPEffect` | `keenaim` → `RUN.critChance` in tower fire |
| `AtkRange/AOERadius/CooldownReduction` | `eagleeye`/`mortar`/`quickhands` → range/splash/rate mults |
| `LifestealCPEffect` | `bloodstone` → `RUN.wallPerKill` |
| `BannerRerollCostReductionCPEffect` | `quarry` → `RUN.freeReroll` |

## Integration points (in `prototype/index.html`)

* Perk system block (data + `rollPerks`/`choosePerk`/`drawPerkDraft`) after the building catalogue.
* `RUN = freshRun()` in `reset()`.
* Effects read in: `tryMerge` (gold-on-merge), tower-fire loop in `updateFight` (dmg/rate/range/splash/slow/crit), kill rewards (gold mult + wall repair), `waveCleared` (wave-gold mult), reroll handler (free reroll).
* Flow: `clear` → (`rollPerks`) → `mode='perk'` → `drawPerkDraft` overlay → tap → `choosePerk` → `build`.
* Pointer handling for `mode==='perk'` added to `pointerdown`.

Verified with `node --check` (syntax clean). No assets added; pure logic + canvas UI in the existing style.

# Implemented so far

* **Roguelite perk draft** (above) — `PERKS` data + `RUN` modifiers + `mode='perk'` draft.
* **Data-driven CONFIG** ✅ — a single `CONFIG` "balance surface" now holds economy (start gold, wall, reroll, wave-clear bonus/regen), tier tables (dmg/range/sell/max), wave generation (count/gap/hp-scaling/boss cadence), and the full enemy stat table. `buildWave`, `makeEnemy`, `reset`, `waveCleared`, and `rerollCost` all read from it. This mirrors Fortress Merge's remote-config indirection — the entire game can be rebalanced by editing one object, no logic changes. See [remote-config](okf/references/remote-config.md).

# What to adapt next (mapped to the blueprint)

Prioritized follow-ons from [the blueprint](okf/blueprint/index.md), in rough ROI order:

1. **Buff/status system** — generalize `slow` into a `BuffId`-style engine (Slow/Stun/Burn/Poison/Shield…) shared by towers and enemies. See [buff-status-system](okf/systems/buff-status-system.md).
2. **Rarity/Tier on towers** — add a quality axis (a rare tower line) to deepen merge decisions. See [enums](okf/data-model/enums.md).
3. **A second mode** — Endless (already most of the way there) or a Boss mode using the existing boss-every-5 logic promoted to a dedicated flow. See [game-modes](okf/systems/game-modes.md).
4. **Analytics logger** — emit `wave_start/clear/fail` + `lost_at_wave` to localStorage/console for balancing. See [analytics-events](okf/references/analytics-events.md).
5. **Perk synergy sets** — themed clusters (Fortress Merge's "Wardrum") for build identity. See [perk-system](okf/systems/perk-system.md).

# How to run / test

Open `prototype/index.html` in a browser. Play through wave 1; on clear, the **CHOOSE A BLESSING** draft appears — pick one and confirm its effect compounds across waves (e.g., stack Sharpened Edges and watch tower damage climb; take Alchemy and see gold on every merge).
