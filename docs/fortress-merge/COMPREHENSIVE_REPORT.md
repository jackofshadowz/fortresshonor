# Fortress Merge — Comprehensive Teardown & Reimplementation Report

**Subject:** `com.ttt.fortressmerge` v0.7.1 · Publisher **TapNation** · **Unity 6000.2 (IL2CPP)**
**Purpose:** Understand how the game is built in order to write a comprehensive report and reimplement a clean-room version for a college course.
**Method:** Static reverse-engineering only — the APK was decompiled and read, never executed; **no art, audio, or code was copied**. This document records *design and architecture*.
**Companion artifacts:** OKF knowledge bundle in [`okf/`](okf/index.md); machine-readable [`spec.json`](spec.json); raw extracts in `remote_config_levers.txt`, `analytics_events.txt`, `names/`.

---

## 1. Executive Summary

Fortress Merge is a **merge + tower-defense auto-battler with a roguelite run layer**, wrapped in a deep collection meta and an aggressive live-service monetization model. The player merges identical units to climb power tiers; those units auto-fight waves of enemies attacking a home base; between rounds the player drafts perks that reshape the run. Four play modes (Campaign, Endless, Boss Fight, Hero Battle) and a calendar of seasonal events recombine a single combat core. Crucially, the game's **balance is tuned server-side** (Firebase Remote Config + TapNation A/B testing), so the client ships *systems* while the server ships *numbers*.

For the course, the most valuable lessons are architectural: a **data-driven, card-unified, effect-pluggable** design that produces enormous content variety from very little code or art.

---

## 2. Genre & Core Loop

**Genre:** merge + tower-defense auto-battler roguelite (hyper-/hybrid-casual).

**Core loop:**
1. **Produce** — spawner buildings emit basic units on a timer.
2. **Merge** — combine identical units to ascend short tier ladders (`Peasant→Warrior→Knight`, `Bowman→Archer→Marksman`, `Horseman→Chevalier`).
3. **Defend** — units auto-target and fight enemy waves advancing on a base with finite HP.
4. **Draft** — between rounds, choose a perk (a small stackable modifier) from a rarity-weighted offer.
5. **Progress** — clear the level for rewards and advance; lose if the base falls.
6. **Meta** — spend rewards on heroes, gear, upgrades, and chests; pursue daily/event goals.

Player agency is **placement, merge timing, and perk choice** — not per-unit micro. That low-APM, high-decision profile is the design's accessibility hook.

---

## 3. Systems (how it works)

Each system has a dedicated OKF document; summaries follow.

* **[Merge](okf/systems/merge-system.md)** — identical units (and buildings) merge into higher tiers on a bounded board (population cap is the core tension knob). Spawn-multiplier, duplication, and gold-on-merge perks feed the loop.
* **[Combat & Waves](okf/systems/combat-and-waves.md)** — auto-resolving combat; enemies arrive in previewed waves via portals along checkpoints toward a base that has HP and can be healed (fountain). `UBasicAttack`, projectiles, AOE.
* **[Perks (CPEffect)](okf/systems/perk-system.md)** — ~40 modifiers implementing a uniform `ICardPassiveEffect.ExecuteEffect(float value, ctx)`; magnitudes are **data**, not code. Offense / economy / spawn / hero-support categories plus a "Wardrum" synergy set.
* **[Buffs (BuffId)](okf/systems/buff-status-system.md)** — a shared status engine: CC (Slow/Stun/Freeze/Glue), DoT (Burn/Poison/Bleed), offense/defense/mobility/area buffs, keyed by numeric ID families.
* **[Heroes](okf/systems/hero-system.md)** — nine collectible heroes with decks, gear + blueprints, level/unlock gating, and an in-run revive-for-cost loop; the meta spine and premium anchor.
* **[Buildings](okf/systems/building-system.md)** — spawners, bank, banner (summon), barricade, catapult; buildings merge to level up.
* **[Economy](okf/systems/economy-currencies.md)** — Coins (soft), Gems (premium), Shards + Blueprints (upgrade/craft), tokens, tickets, keys, event currencies; card upgrades priced in Coins/Gems/Shards with an `LNum` big-number type for idle-style scaling.
* **[Chests & Gacha](okf/systems/chests-and-gacha.md)** — tiered chests + banner summons, rarity-weighted, including an explicit **deterministic ("rigged") first-pull path** for onboarding.
* **[Game Modes](okf/systems/game-modes.md)** — one combat core; modes are thin `RoundManager` subclasses + config + a ticket cost.
* **[LiveOps & Events](okf/systems/liveops-events.md)** — seasonal TLE reskins, time-limited/fail offers, subscriptions, and server-side A/B testing.

---

## 4. Data Model

The game unifies units, buildings, and heroes under a **Card** abstraction (`CardSettings` definition + `CardData` runtime state) sharing one upgrade economy. Balance lives in serialized data and remote config, not in code paths. Full detail: [Card Data](okf/data-model/card-data.md), [Enums](okf/data-model/enums.md), [Roster](okf/data-model/unit-roster.md), and [`spec.json`](spec.json).

Notable conventions worth copying: **numeric ID ranges encode families** (ResourceType 100s=blueprints, 300s=tickets, 800s=event currencies; BuffId 100s=CC, 200s=DoT…), making the data self-documenting.

---

## 5. Technical Architecture & Stack

* **Engine:** Unity 6000.2.6f2, IL2CPP, Addressables (only audio/font/monoscript bundles ship locally — no remote content CDN), TextMeshPro, Newtonsoft.Json. Android App Bundle with per-ABI splits.
* **Publisher SDK:** TapNation (`FG*` classes wrap IAP, ads, remote config, consent, analytics).
* **Monetization:** AppLovin MAX (primary mediation) + IronSource, AdMob, Meta Audience Network, Moloco, Mintegral, Fyber; Adjust (attribution); AppHarbr (ad quality); Unity IAP + Play Billing with subscriptions.
* **Analytics/Backend:** GameAnalytics + Firebase (Analytics, Crashlytics, Remote Config); Firebase Cloud Functions for **cloud-save only**; GeekLab for UA testing.

Detail: [Tech Stack](okf/references/tech-stack.md).

---

## 6. The Critical Finding: Balance Is Server-Side

Static analysis recovered the complete *system and data structure* but **not the literal tuning numbers** (unit stats, merge/upgrade costs, wave tables, perk magnitudes, drop rates). Three independent pieces of evidence:

1. **No config ScriptableObjects ship** — the Addressables catalog references only audio/font/monoscript bundles; there is no remote content CDN and no balance asset bundle.
2. **Shipped components are empty stubs** — reconstructing the type-tree for `RoundManager` and reading its instance yielded ~48 bytes (base fields only); configs are populated at runtime.
3. **Values are injected at runtime** — perks take `ExecuteEffect(float value)` with `value` from `CardPassiveValues`, sourced from **Firebase Remote Config + TapNation A/B tests**.

This is itself the headline lesson: **commercial live-service games keep balance server-side so they can A/B test and retune without app updates.** Recovering the live values would require either deep native disassembly (Ghidra reading hardcoded defaults from `libil2cpp.so`) or a dynamic MITM capture of the remote-config payload (which requires running the app — deliberately out of scope here). The server-tunable surface itself *was* recovered: see [Remote Config Levers](okf/references/remote-config.md).

---

## 7. Monetization & Live-Ops (for analysis, not adoption)

Rewarded video throughout; interstitials gated and paced by remote config; IAP + subscriptions with introductory pricing; **fail-offers** after losses; time-limited and discounted bundles; seasonal events each pairing a reskinned mode with an event currency, reward track, and offer. The **rigged-chest** mechanism (deterministic early pulls) and `player_lost_at_level_wave` telemetry show a tightly instrumented retention/monetization funnel. For a course, this is a rich case study in live-service design *and* its player-fairness tradeoffs.

---

## 8. Reimplementation Blueprint (course project)

A clean-room build should adopt the *architecture*, not the content. Full spec in [`okf/blueprint/`](okf/blueprint/index.md):

* **[Scope & MVP](okf/blueprint/scope-and-mvp.md)** — a vertical slice (one board, one spawner, one 3-tier merge line, scripted waves, auto-combat, ~8 perks, one currency, local config) captures the fun; everything else is a content multiplier.
* **[Architecture](okf/blueprint/architecture.md)** — data-driven config; unified Card; pluggable `ICardPassiveEffect`; one combat core + thin mode subclasses; shared buff engine; deterministic, headless-runnable simulation.
* **[Data Schemas](okf/blueprint/data-schemas.md)** — concrete JSON for cards, perks, buffs, waves, loot tables, and the central `config.json`.
* **[Roadmap](okf/blueprint/roadmap.md)** — a ~10–12 week phased plan from prototype to instrumented, balanced build, with per-phase learning objectives.

**Five architectural takeaways to teach:**
1. Data-driven balance — code reads named config; never hardcode numbers.
2. Unified Card abstraction across units/buildings/heroes.
3. Uniform pluggable effect interface for perks and buffs.
4. One combat core, many modes (config + thin subclass).
5. A shared status engine for allies and enemies.

---

## 9. Methodology & Provenance

Pipeline: APK/XAPK acquisition (package-verified, hashed) → `apktool` (manifest/resources) → **UnityPy** (asset graph, class/roster/audio names) → **Il2CppDumper** (full type model `dump.cs`, `stringliteral.json`, DummyDll) → **AssetRipper** + **TypeTreeGeneratorAPI** (ScriptableObject attempt) → **Ghidra** (native constant pass). All inside an isolated Docker container; APK never executed. Full account and limitations: [Extraction Methodology](okf/references/methodology.md).

---

## 10. Legal & Ethical Note

This analysis is for **education and clean-room reimplementation**. Game mechanics and ideas are not copyrightable; the recovered material is *design and structure*. No proprietary assets or code are reproduced in this report or intended for the course build. The reimplementation should use original/licensed (e.g., CC0) art and independently written code.
