---
type: game-structure
title: LiveOps & Events
description: Seasonal time-limited events, time-limited offers, fail-offers, and server-side A/B testing that continuously retune balance and monetization.
resource: app://com.ttt.fortressmerge/systems/liveops
tags: [liveops, events, ab-testing, offers, monetization, retention]
timestamp: 2026-06-21T00:00:00Z
---

# Concept

The game is built as a **live service**: balance and monetization are tuned server-side via Firebase Remote Config and TapNation A/B testing (key `pp_fg_ab_test_ids`, endpoint `api.tnapps.xyz/v1/abtests/com.TapNation.SDKv3`), with a calendar of recurring events. The client ships systems; the server ships the numbers.

# Seasonal Events (TLE = Time-Limited Event)

Recovered event managers and currencies:
* **Easter** (`EasterEventManager`, `EasterEgg` currency)
* **Valentine's** (`ValentinesDayEventManager`, `ValentineGiftBox`, `ValentinesDayFlyAnimation`)
* **St. Patrick's** (`StPatrickDayTimeLimitedOffer`)
* **Halloween** (`Pumpkin` currency)
* **Undertaker** (`UnderTakerEventManager`, themed skeleton hero/enemies)
* **Trampoline Madness** (`TrampolineMadnessEventManager`, `TrampolineCoin`, `Trampoline_Jump`)

Each event typically reskins a core mode (`TLE*` managers) and adds an event currency + reward track + limited offer.

# Offers

* `TimeLimitedOffersManager`, `RewardType.DiscountedBundle`, `RewardType.GooglePlayPass`.
* **Fail offers** — surfaced after losing: `fail_offer_enabled`, `fail_offer_min_level`, `fail_offer_relaunch_count`.
* Subscriptions with intro pricing (`is_introductory_price_period`, `confirm_subscription_price_change`).

# Server-Tunable Levers

Ad pacing, difficulty offset, chest tiers, hero-gem purchase, offer gating — the full list is in [Remote Config Levers](/references/remote-config.md). These are read at runtime and A/B-tested, which is precisely why static analysis cannot recover the live values.

# Design Notes for Reimplementation

* For a course project, stub the remote layer with a **local JSON config** that mimics remote config — same architecture, no server. This teaches the live-service pattern safely.
* Events = (reskin + event currency + reward track + offer) over an existing mode. Cheap to produce, strong for retention.
* A/B testing is optional for a course but worth *documenting* as the reason commercial games keep balance server-side.

# Citations

[1] Event managers, TLE classes, offer/remote-config keys from `dump.cs` + `stringliteral.json` + network strings. See [methodology](/references/methodology.md).
