---
type: technical-reference
title: Remote Config Levers
description: The server-tunable parameter surface (Firebase Remote Config keys) controlling ad pacing, difficulty, offers, and economy — why balance numbers aren't in the binary.
resource: app://com.ttt.fortressmerge/remote-config
tags: [remote-config, liveops, ab-testing, monetization, tuning]
timestamp: 2026-06-21T00:00:00Z
---

# Why This Matters

Fortress Merge reads tuning from **Firebase Remote Config** at runtime and A/B-tests it via TapNation. The client ships *systems*; the server ships *numbers*. This is the structural reason the actual balance values are not statically extractable — and the key live-service lesson for the report.

# Recovered Keys (selection, from `stringliteral.json`)

## Ad pacing
* `interstitial_starts_from_level` — gate before interstitials begin
* `interstitial_allow_end_level`, `interstitial_allow_end_wave`, `interstitial_allow_perks_screen` — where interstitials may show
* `interstitial_default_delay` — min seconds between interstitials
* `disable_b2b_ad_unit_ids` — prevent back-to-back ads
* rewarded-ad gating keys

## Difficulty / pacing
* `difficulty_offset_start_level` — difficulty ramp offset
* `plane_mode_blocker_appear_level` — when a specific blocker mechanic appears

## Offers / monetization
* `fail_offer_enabled`, `fail_offer_min_level`, `fail_offer_relaunch_count` — post-loss offer logic
* `confirm_subscription_price_change`, `is_introductory_price_period`, `has_introductory_price_trial`, `new_sku_price_in_micros`, `old_sku_price_in_micros`
* `can_buy_hero_with_gems` — economy toggle

## Chests / economy
* `big_gear_chest`, `medium_gear_chest`, `legendary_gear_chest`
* `big_shard_chest`, `medium_shard_chest`, `legendary_shard_chest`
* `cards_chest_x10_opened`

## A/B / config plumbing
* `pp_fg_ab_test_ids`, `remote_config`, `firebase_uid`, `firebase_read`, `firebase_write`

Full extracted list: `out/remote_config_levers.txt`.

# Design Notes for Reimplementation

* Mirror this with a single `config.json` of named keys → values, loaded at boot and overridable. Every balance number lives there, not in code.
* Even without a server, modeling the *indirection* (code reads keys, never literals) teaches the live-service architecture and makes the game tunable by non-programmers.

# Citations

[1] `stringliteral.json` (Il2CppDumper), filtered to config-key shapes. See [methodology](methodology.md).
