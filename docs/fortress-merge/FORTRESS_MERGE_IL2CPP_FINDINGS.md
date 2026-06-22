# Fortress Merge — IL2CPP Deep Dive (the "numbers" pass)

Follow-up to `FORTRESS_MERGE_TEARDOWN.md`. Goal was to recover the actual balance **values**. Verdict up front: **the literal tuning numbers are not statically present in the APK** — they're hardcoded in native code and/or fetched at runtime. Everything *structural* was recovered. Details and evidence below.

## Toolchain (all in-container, nothing executed)
- **Il2CppDumper** ✅ — recovered the full type model from `libil2cpp.so` (armeabi-v7a split) + `global-metadata.dat` (metadata v31, Unity 6000.2.6f2). Outputs: `dump.cs` (35 MB), `stringliteral.json`, DummyDll assemblies. (Binary flagged "may be protected" but dump succeeded.)
- **AssetRipper** (arm64) ❌ for scripts — its Cpp2IL engine "failed to find code registration" on this binary → fell back to the Unknown backend, so it can't type ScriptableObjects.
- **TypeTreeGeneratorAPI + UnityPy** ✅ tooling, but see finding below — there were no config ScriptableObjects to read.

## Why the numbers aren't extractable statically (evidence)
1. **No config ScriptableObjects ship in the bundles.** The Addressables catalog (`catalog.bin`) references only **3 local bundles**: audio, fonts, monoscripts. There is **no remote content CDN** and no `*_settings`/`*_data` content bundle. The `CardSettings`/`RaritySettings`/`UnitData`/`WaveData` classes exist in the type model but are never instantiated as assets in `data.unity3d`.
2. **The MonoBehaviour instances that do ship are empty stubs.** Reading `RoundManager` via reconstructed type-trees failed with *"expected 23088 bytes, read 48"* — i.e. the serialized component is ~48 bytes (base fields only). Configs are populated at runtime, not serialized.
3. **Values are data-driven + remote.** Perks run through `ExecuteEffect(float value, …)` and `CardPassiveValues { float value }` — magnitudes are injected at runtime. Tuning comes from **Firebase Remote Config** (`FGRemoteConfigManager`) and **TapNation A/B testing** (`api.tnapps.xyz/v1/abtests/com.TapNation.SDKv3`, key `pp_fg_ab_test_ids`).

So the real balance lives in: (a) hardcoded defaults inside `libil2cpp.so` method bodies, and (b) server-delivered remote-config/A-B overrides.

## Identity & infrastructure (recovered)
- **Publisher:** TapNation (the `FG*` classes = TapNation/FunGames SDK). UA/creative testing via **GeekLab** (`analytics.geeklab.app`).
- **Backend:** Firebase Cloud Functions `europe-west1-fortress-merge.cloudfunctions.net` — cloud save/state only (`saveOps`, `transferSave`, `adminGetState`, `devWipe`).
- **Monetization mediation:** AppLovin MAX (primary), IronSource, AdMob, Meta Audience Network, Moloco, Mintegral, Fyber. Attribution: Adjust. Ad-quality: AppHarbr. Analytics: GameAnalytics + Firebase.

## Recovered data model (structure, complete)
- `CardType`: **Unit, Building, Hero** — the three card categories.
- `Rarity`: Common, Uncommon, Rare, Epic, Legendary, Mythic.
- `Tier`: Normal, S, SS, SSS.
- `UnitType` (merge chains): melee `Peasant→Warrior→Knight`, ranged `Bowman→Archer→Marksman`, cavalry `Horseman→Chevalier`; plus full enemy/boss roster (Slime/Skeleton/Zombie/Cursed/Orc lines, Golem family, Dragon/Demon/Medusa/Reaper bosses).
- `ResourceType`: Coins, Gems, XP, Tokens, SkipIt, Blueprints (Head/Chest/Boots/Ring/Hero), Chest keys (Epic/Common), per-mode Tickets (Endless/BossFight/HeroBattle), Bag keys, event currencies (Pumpkin/ValentineGiftBox/EasterEgg/TrampolineCoin).
- `RewardType`: Resource, Gear, SpeedUp, Unit, NoAds, RandomGear, RandomBlueprint, Blessing, DiscountedBundle, GooglePlayPass, InfiniteSkipItTicket…
- `BuffId` (status-effect system):
  - **CC:** Slow, Stun, Freeze, Glue
  - **DoT:** Burn, Poison, Bleed
  - **Offense buffs:** AtkDmg, AttackSpeed, Critical, Drunk
  - **Defense:** HealOverTime, Shield, Invincibility
  - **Mobility:** Speed, Sprint
  - **Range/AOE:** Range, AOERadius, AOEDamage
- Card economy (`CardData`): per-card upgrade prices in Coins / Gems / **Shards**, `LNum` big-number scaling, level/maxlevel, gem-pay path, shard-gated upgrades.
- Chests (`ChestSettings`): `CardChests`, `GearChests`, and **`cardsForRiggedchests`** (deterministic/"rigged" chest contents — a tuned first-time-user funnel).

## LiveOps / monetization levers (remote-config keys → `out/remote_config_levers.txt`)
The server-tunable surface, e.g.:
- **Ad pacing:** `interstitial_starts_from_level`, `interstitial_allow_end_level`, `interstitial_allow_end_wave`, `interstitial_allow_perks_screen`, `interstitial_default_delay`, `disable_b2b_ad_unit_ids`.
- **Difficulty pacing:** `difficulty_offset_start_level`, `plane_mode_blocker_appear_level`.
- **Offers:** `fail_offer_enabled`, `fail_offer_min_level`, `fail_offer_relaunch_count`, `confirm_subscription_price_change`, introductory-price flags.
- **Economy toggles:** `can_buy_hero_with_gems`, chest tiers (`big/medium/legendary` × `gear/shard`).

## Analytics funnel (→ `out/analytics_events.txt`)
`level_start/complete/fail`, `level_up`, `player_lost_at_level_wave`, `chest_open`, `cards_chest_x10_opened`, `daily_reward_claim`, `daily_mission_completed`, `daily_missions_reroll_bought`, purchase/price events, etc. — a standard hyper-casual progression+monetization funnel.

## Two paths to the actual numbers (if ever needed)
1. **Native RE (static):** load `libil2cpp.so` into Ghidra/IDA with the Il2CppDumper `script.json` to label functions, then read hardcoded default constants out of method bodies. Heavy, partial, no execution.
2. **Runtime capture (dynamic):** run the game on an emulator behind an MITM proxy and capture the Firebase Remote Config / TapNation A/B payloads. Gets the *live* tuned values directly — but requires executing the app (the thing we deliberately avoided).

## Net takeaway for Fortress of Honor
You now have the **complete mechanics + data model**: card types, rarities, tiers, merge chains, currencies, the buff/status system, the chest/gacha structure, and the exact LiveOps levers a shipped merge-defense game tunes. The specific magnitudes are intentionally server-side (so they A/B test them) — which is itself the lesson: **build your config as data + a remote-tunable layer from day one.** The values themselves are theirs to tune; the *system design* is what's worth adopting.
