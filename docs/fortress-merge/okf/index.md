# Fortress Merge — Knowledge Bundle

Reverse-engineering knowledge base for **Fortress Merge** (`com.ttt.fortressmerge`, v0.7.1, publisher TapNation), produced by static analysis for an educational clean-room reimplementation. No game assets or code are reproduced here — this documents *design and architecture* only.

# Start Here

* [Overview](overview.md) - what the game is, genre, and the one-paragraph core loop
* [Comprehensive Report](../COMPREHENSIVE_REPORT.md) - the full human-readable report
* [Blueprint](blueprint/) - buildable spec for a new version (course project)

# Systems

* [Systems index](systems/) - all gameplay systems
* [Merge System](systems/merge-system.md) - unit/building merging and tier progression
* [Combat & Waves](systems/combat-and-waves.md) - auto-battle, wave defense, base health
* [Perk System](systems/perk-system.md) - the roguelite `CPEffect` drafting layer
* [Buff / Status System](systems/buff-status-system.md) - `BuffId` crowd-control and buffs
* [Hero System](systems/hero-system.md) - heroes, decks, gear, revive economy
* [Building System](systems/building-system.md) - spawners and building upgrades
* [Economy & Currencies](systems/economy-currencies.md) - resources, sinks, and sources
* [Chests & Gacha](systems/chests-and-gacha.md) - chests, keys, rigged contents
* [Game Modes](systems/game-modes.md) - campaign, endless, boss, hero battle
* [LiveOps & Events](systems/liveops-events.md) - TLE events, offers, A/B testing

# Data Model

* [Data model index](data-model/) - enums and serialized structures
* [Enums](data-model/enums.md) - the recovered C# enums (rarity, tier, units, resources…)
* [Card Data](data-model/card-data.md) - the unified card abstraction
* [Unit & Enemy Roster](data-model/unit-roster.md) - full cast

# Reference

* [Reference index](references/) - infrastructure and methodology
* [Tech Stack & SDKs](references/tech-stack.md) - engine, publisher, ad/analytics stack
* [Remote Config Levers](references/remote-config.md) - server-tunable parameters
* [Analytics Events](references/analytics-events.md) - telemetry funnel
* [Extraction Methodology](references/methodology.md) - how this was recovered
