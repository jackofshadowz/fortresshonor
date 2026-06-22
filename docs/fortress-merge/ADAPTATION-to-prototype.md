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

# Provenance legend

Every adaptation is tagged by how directly it derives from the Fortress Merge teardown — important for an honest clean-room/educational record:

* 🟢 **Ported** — the mechanic was *observed* in FM (class/enum/string evidence) and faithfully adapted.
* 🟡 **Inferred** — the *feature* was observed in FM, but its exact logic is server-side/native and was not directly readable, so the mechanic here is our reasonable interpretation.
* 🔵 **Enrichment** — our own addition, not evidenced in FM.

# Implemented so far

* 🟢 **Roguelite perk draft** — `PERKS` data + `RUN` modifiers + `mode='perk'` draft. Observed: `*CPEffect` / `ICardPassiveEffect` classes.
* 🟢 **Data-driven CONFIG** — one `CONFIG` "balance surface" (economy, tiers, wave gen, enemy table, buff + rarity tunables) read by `buildWave`/`makeEnemy`/`reset`/`waveCleared`/`rerollCost`. Observed: FM's remote-config indirection (`FGRemoteConfigManager`, key surface). See [remote-config](okf/references/remote-config.md).
* 🟢 **Buff/status engine** — `applyBuff`/`hasBuff`/`tickBuffs` + `CONFIG.buffs`; Slow/Stun/Burn/Poison via `hitEnemy`'s `fx` arg; perks Wildfire/Venom/Concussion + overlays. Observed: `BuffId` enum (Slow/Stun/Burn/Poison/…). See [buff-status-system](okf/systems/buff-status-system.md).
* 🟢 **Rarity axis** — Common/Rare/Epic carried through merge (`tryMerge` group-max, all-rare promotes), multiplying damage; grid gem + tray outline. Observed: `Rarity` enum. See [enums](okf/data-model/enums.md).
* 🟢 **Second mode (Boss Rush)** — title toggle; `runMode` drives `bossRushWave()` vs `buildWave`. Observed: FM ships a Boss Fight mode (and modes as thin variants over one combat core); the specific wave composition is our tuning.
* 🟢 **Analytics logger** — `ANALYTICS.log` → console + `localStorage('foh_events')`; `run_over` carries the wave reached. Observed: FM's analytics funnel incl. `player_lost_at_level_wave`. See [analytics-events](okf/references/analytics-events.md).
* 🟢 **Perk synergy sets** — `SETS` (Warband/Fortune/Elements), escalating bonuses via `checkSetBonus`; cards show set tags. Observed: FM's "Wardrum" synergy cluster. See [perk-system](okf/systems/perk-system.md).
* 🟡 **Idle "AFK village" income** — the city produces Gems in real time while closed (`claimIdle`, claimed on load + tab-return, capped), scaling with owned prestige buildings (`IDLE.yield`). **Observed:** FM's AFK Village *exists* (`AfkVillageTouchManager`, `AfkVillageSettings.villageUpgrade`, ~40 `AFKVillageUnitPrefab`). **Inferred:** the actual idle-income logic/rate is server/native-side and was not directly read — this gems-per-hour model is our interpretation of "AFK village." See [hero-system](okf/systems/hero-system.md) (meta systems).
* 🟡 **Village-upgrade loop** — two leveled, Gems-bought upgrades on the title (`buyVillage`): **Granary** (+idle rate) and **Warehouse** (+idle cap), stored in `META.s.village`. Closes the idle loop (idle gems → upgrade → more idle). **Observed:** FM's `AfkVillageSettings.villageUpgrade` / `VillageUpgradeData` (a village-upgrade list exists). **Inferred:** the specific upgrades and their effects are our design.
* 🟢 **Visible city growth** — owned `PRESTIGE_CAT` decorations (statue/fountain/gardens/cathedral/throne) render on the title keep via `drawDeco` + `DECO_SLOTS`, so spending Gems on prestige visibly builds your city. Observed feature: the prestige decorations already exist as data; the chunky art is our rendering.

* 🟢 **Hero roster** — `HEROES` catalog of 5 collectible champions (Commander/Markswoman/Merchant Prince/Pyromancer/Frostweaver), each granting a run-long boon applied in `reset()`; owned/selected in `META.s.heroes`; a dedicated **Heroes screen** (`drawHeroes`, reachable from the title) to select owned or recruit locked ones with Gems. Observed: FM's multi-hero collection; our heroes/effects are clean-room (shared knight art for now).

**The in-run combat track is complete** (perks, config, buffs, rarity, 2nd mode, analytics, synergy). **The City/Meta spine is complete** (idle income → village upgrades → visible city growth). **Hero spine started** (roster + selection).

# What to adapt next (mapped to the blueprint)

1. **Kingdom notebook** 🔵 — a consolidated overview screen (city view, champion, currencies/rank, idle income, owned upgrades, badges summary, run stats) linking to Store/Heroes/Badges. The "one place to see everything" dashboard. *(Our IA addition; FM has per-system screens.)*
2. **Hero depth** — gear/blueprints + per-hero levels + the revive-cost loop, deepening the roster. See [hero-system](okf/systems/hero-system.md).

# How to run / test

Open `prototype/index.html` in a browser. Play through wave 1; on clear, the **CHOOSE A BLESSING** draft appears — pick one and confirm its effect compounds across waves (e.g., stack Sharpened Edges and watch tower damage climb; take Alchemy and see gold on every merge).
