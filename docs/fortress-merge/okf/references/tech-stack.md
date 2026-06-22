---
type: technical-reference
title: Tech Stack & SDKs
description: The engine, scripting backend, publisher SDK, monetization mediation, analytics, and backend services that compose Fortress Merge.
resource: app://com.ttt.fortressmerge/tech-stack
tags: [tech-stack, unity, sdks, monetization, backend]
timestamp: 2026-06-21T00:00:00Z
---

# Engine & Build

* **Unity 6000.2.6f2** (Unity 6.2), **IL2CPP** scripting backend, metadata v31.
* **Addressables** for content (catalog `catalog.bin`; only audio/fonts/monoscripts bundles ship locally — no remote content CDN).
* **TextMeshPro** UI text; **Newtonsoft.Json** for serialization.
* Distributed as an Android App Bundle (base + per-ABI splits: armeabi-v7a, arm64-v8a, x86, x86_64).

# Publisher & Framework

* **TapNation** — the `FG*` classes (FGMax, FGIAPManager, FGRemoteConfigManager, FGAnalyticsManager, FGAdjust, FGFireBase, FGGDPRManager, FGUserConsentManager…) are TapNation's publisher SDK wrapper. Developer id "ttt".
* A/B testing: `api.tnapps.xyz/v1/abtests/com.TapNation.SDKv3` (remote-config key `pp_fg_ab_test_ids`).
* UA/creative testing: **GeekLab** (`analytics.geeklab.app`).

# Monetization Mediation

* **AppLovin MAX** (primary mediation), **IronSource**, **Google AdMob**, **Meta Audience Network**, **Moloco**, **Mintegral** (mbridge), **Fyber**.
* Attribution: **Adjust**. Ad-quality/safety: **AppHarbr**. IAP: Unity IAP + Google Play Billing (subscriptions supported).

# Analytics & Backend

* **GameAnalytics** + **Firebase** (Analytics, Crashlytics, Remote Config).
* Backend: **Firebase Cloud Functions** at `europe-west1-fortress-merge.cloudfunctions.net` — cloud save/state only (`saveOps`, `transferSave`, `adminGetState`, `adminSetState`, `devWipe`). No gameplay simulation server.

# Implication for Reimplementation

* A course build needs **none** of the monetization/analytics stack. Replace the entire `FG*` + ad/analytics layer with no-op stubs or a single local analytics logger.
* Replace Firebase Remote Config with a **local JSON config file** loaded at startup (same architecture, zero backend). See [Blueprint](/blueprint/).

# Citations

[1] `boot.config`, manifest, `dump.cs` class names, `stringliteral.json` URLs. See [methodology](methodology.md).
