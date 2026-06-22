---
type: blueprint
title: Scope & MVP
description: A phased scope for reimplementing the game in a course — the minimum vertical slice, then content multipliers, with explicit cut lines.
resource: app://fortress-merge-clone/scope
tags: [blueprint, scope, mvp, planning]
timestamp: 2026-06-21T00:00:00Z
---

# Design Goal

Reproduce the **feel** of Fortress Merge's core loop — *merge units → auto-defend waves → draft perks* — as a clean-room build. Optimize for a learnable, demonstrable vertical slice, not feature parity.

# MVP (the vertical slice)

The smallest build that captures the fun:

1. **One board** with a fixed grid + a population cap.
2. **One spawner building** producing a basic unit on an interval.
3. **Merge** of identical units along one short ladder (e.g., 3 tiers, melee only).
4. **One enemy type** arriving in **scripted waves** toward a base with health.
5. **Auto-combat** (nearest-target, attack on cooldown, simple projectile/melee).
6. **Wave preview** + win/lose (clear N waves vs base destroyed).
7. **Perk draft** between waves: 3-of-N from ~8 perks (a couple offense, a couple economy, a couple spawn).
8. **One soft currency** (Coins) earned on clear, spent to buy/merge.
9. **Local config.json** holding every tunable number (no hardcoded balance).

This is enough to study merge tension, difficulty pacing, and roguelite variety.

# Phase 2 (content multipliers, same core)

* Second + third unit lines (ranged, cavalry) and **enemy families** (reskinned archetypes).
* **Buildings** beyond the spawner (bank, barricade, catapult) with merge-leveling.
* **Heroes** as special cards + a revive-cost loop.
* **Bosses** (enemy + special attack + telegraph + healthbar).
* **Chests** (loot table by rarity weight) + a second currency (Gems).
* **Modes:** Endless (survival), then Boss Fight.

# Phase 3 (live-service patterns — for teaching, not revenue)

* **Hero Battle** two-phase deckbuilder mode.
* Daily login/missions; an **event** (reskin + event currency + reward track).
* A **remote-config indirection** layer (still local JSON) + a local analytics event logger.

# Explicit Cut Lines (do NOT build for a course)

* No ad SDKs, IAP, subscriptions, or attribution.
* No third-party analytics; a local logger only.
* No backend; cloud-save is out of scope (local save is fine).
* The AFK idle village and the full 9-hero/40-perk catalog are stretch goals, not MVP.

# Suggested Milestones

* **M1 (prototype):** items 1–6 above — merge + waves + auto-combat + win/lose.
* **M2 (the hook):** add perks (7), economy (8), config (9) — the loop is now "fun".
* **M3 (depth):** Phase 2 subset (extra lines + heroes + one boss + chests).
* **M4 (polish/teach):** one mode + one event + analytics logger + a balancing pass using the event data.

# Citations

[1] Scope derived from the system docs in this bundle. See [Systems](/systems/).
