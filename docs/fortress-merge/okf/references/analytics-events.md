---
type: technical-reference
title: Analytics Events
description: The telemetry/monetization funnel — the analytics events the game emits, revealing the designed player progression and KPI focus.
resource: app://com.ttt.fortressmerge/analytics
tags: [analytics, telemetry, funnel, kpis]
timestamp: 2026-06-21T00:00:00Z
---

# Concept

The event taxonomy (GameAnalytics + Firebase) reveals which moments the studio measures — effectively a map of the intended player journey and the KPIs that drive live-ops decisions.

# Recovered Events (selection)

## Progression funnel
* `level_start`, `level_complete`, `level_end`, `level_fail`, `level_up`
* `current_level`, `main_level`, `mode_level`, `level_progression`, `level_name`
* `player_lost_at_level_wave` — the precise loss point (difficulty tuning signal)

## Economy / monetization
* `chest_open`, `chest_opened`, `cards_chest_x10_opened`
* `daily_reward_claim`, `daily_mission_completed`, `daily_missions_reroll_bought`
* purchase/price events (`price_amount`, `price_amount_micros`, `price_currency_code`, `purchase_price`, `pricepoint_id`)
* ad events (`ad_impression`, `custom_ad_impression`, `rewarded_ad`, `ad_source`, `ad_format`, `ad_unit_name`), `fg_ltv_ad_impression`
* `no_ads`, `generate_lead`

Full extracted list: `out/analytics_events.txt`.

# Design Notes for Reimplementation

* The funnel `level_start → level_complete | level_fail (+ player_lost_at_level_wave)` is the minimum viable analytics for balancing difficulty — instrument these first.
* `player_lost_at_level_wave` is the single most useful balance signal: it tells you exactly where players churn. Build the equivalent into the course project.
* For a course, a local CSV/JSON event logger reproduces the *analysis* value without any third-party SDK.

# Citations

[1] `stringliteral.json` event-name strings (Il2CppDumper). See [methodology](methodology.md).
