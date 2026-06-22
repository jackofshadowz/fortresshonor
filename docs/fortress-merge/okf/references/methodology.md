---
type: technical-reference
title: Extraction Methodology
description: The static-analysis pipeline used to recover Fortress Merge's design — tools, steps, what worked, and the limits of static recovery.
resource: app://com.ttt.fortressmerge/methodology
tags: [methodology, reverse-engineering, static-analysis, il2cpp, tooling]
timestamp: 2026-06-21T00:00:00Z
---

# Principles

* **Static only** — the APK was decompiled and read, never executed. Analysis ran inside an isolated Docker container; the binary never ran on the host.
* **Design, not assets** — goal was to learn systems for a clean-room reimplementation; no art, audio, or code was copied out.

# Pipeline

1. **Acquire** — APK by package name from APKPure (base, 221 MB) + full XAPK (for the `libil2cpp.so` ABI split). Integrity: SHA-256 recorded; package verified as `com.ttt.fortressmerge` (rejected an early download that was the Uptodown store-client app).
2. **Unpack** — `apktool` (manifest, resources, asset listing) confirmed Unity + IL2CPP (`global-metadata.dat`).
3. **Asset graph** — **UnityPy** over `data.unity3d` + bundles: enumerated object types, extracted TextAssets, listed MonoBehaviour classes, GameObject/Sprite/Audio/Animation names. Revealed managers, modes, perks, roster, audio cues.
4. **Type model** — **Il2CppDumper** on `libil2cpp.so` + `global-metadata.dat` (metadata v31, Unity 6000.2) → `dump.cs` (35 MB: all classes, enums, fields, method signatures), `stringliteral.json` (config keys, URLs, events), DummyDll assemblies.
5. **ScriptableObject attempt** — **AssetRipper** (Cpp2IL) failed to find code registration on this binary; **TypeTreeGeneratorAPI + UnityPy** reconstructed type-trees, but the config ScriptableObjects are not shipped as assets.
6. **Native pass (in progress)** — **Ghidra** headless on `libil2cpp.so`, labeled via Il2CppDumper `script.json`, decompiling targeted initializer functions to read any hardcoded constants.

# What static analysis recovered

Complete: genre, core loop, all systems, the full type/enum model, the roster, the economy structure, the remote-config lever surface, the analytics funnel, the full tech/monetization stack.

# What it could NOT recover (and why)

The literal **balance magnitudes** (unit stats, merge/upgrade costs, wave tables, perk values, drop rates). Evidence:
* No config ScriptableObjects ship (Addressables catalog has only audio/font bundles; no remote content CDN).
* The MonoBehaviour instances that ship are near-empty stubs (e.g., `RoundManager` ≈ 48 bytes).
* Values are injected at runtime via `CardPassiveValues.value` from **Firebase Remote Config + TapNation A/B tests**.

To obtain live numbers would require either deep native RE (Ghidra reading method-body constants) or a dynamic run behind an MITM proxy to capture the remote-config payload — the latter requires executing the app, which was deliberately avoided.

# Tooling Summary

`apktool`, `UnityPy`, `Il2CppDumper`, `AssetRipper`, `TypeTreeGeneratorAPI`, `Ghidra` — all in a Debian-based Docker container (Java 17, Python 3.14, .NET 8).
