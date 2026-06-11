# Asset Library — Catalog & Usage Reference

> Reference for humans **and coding agents** working on Fortress of Honor.
> The raw packs live in `asset library/` (~40 packs, ~31k files, gitignored-large).
> For the assets actually wired into the game and how to add more, see
> **`prototype/assets/README.md`** — read that first if you're touching art.

## TL;DR for agents

- **Art source of record:** **Tiny Swords** by Pixel Frog (CC0). Top-down pixel-art,
  192px unit frames, 64px terrain tiles. Everything in the shipping game comes from here.
- **Style policy:** Stay inside the Tiny Swords look. Other pixel packs below are
  *compatible-ish* but mixing them dilutes identity — only pull them for things Tiny
  Swords lacks (e.g. distinct monster types, spell VFX, item icons). **Never** use the
  3D (KayKit), platformer (Sunny-land), or off-style packs as in-game sprites.
- **License first:** Prefer **CC0** packs (Tiny Swords, KayKit, NOX Sound, monogram) for
  anything shipped. Flag non-commercial / GPL / attribution-required packs before use.

## Relevance legend

| Tag | Meaning |
|-----|---------|
| ✅ **canonical** | The game's art source — use freely |
| 🟢 **drop-in** | CC0/compatible pixel style, safe to mix sparingly |
| 🟡 **compatible** | Right medium (top-down pixel) but different artist; mix only for gaps |
| 🟠 **off-style** | Pixel but wrong perspective/scale/vibe — avoid for sprites |
| 🔴 **wrong medium** | 3D / platformer / non-game — not usable as 2D sprites |
| 🎵 **audio/font** | Sound or typography, medium-agnostic |

---

## ✅ In use — Tiny Swords (the source of record)

| Pack | Style / Res | License | Notes |
|------|-------------|---------|-------|
| **Tiny Swords (Update 010)** | Pixel, top-down. Units 192px frames (6–8 cols), tiles 64px | **CC0** | Primary source. Knights + Goblins factions, terrain, resources, effects, **full UI kit**. Most of `prototype/assets/` is copied from here. |
| **Tiny Swords (Free Pack)** | Pixel, top-down. Buildings ~192px | **CC0** | Older sibling pack — *different, extra* content not in Update 010: building types **Archery / Barracks / Monastery**, 3 house variants, **5 faction colors**, **health-bar UI**, clouds/bushes. Same style, safe to mix. |

These two together cover ~90% of what the game needs. The biggest **unused** value:
the Tiny Swords **UI kit** (buttons, ribbons, banners, bars, icons, pointers), the
**Goblin faction buildings**, **resource/economy sprites** (gold/wood/meat, gold mine),
**elevation + bridge terrain**, the **Dead/corpse** sprite, and the extra **building
types**. Exact paths + copy recipe are in `prototype/assets/README.md`.

---

## 🟢🟡 Compatible pixel packs — pull only for gaps Tiny Swords doesn't cover

### Monsters / enemies (Tiny Swords only ships goblins)
| Pack | Style / Res | License | What's there |
|------|-------------|---------|--------------|
| Monsters_Creatures_Fantasy | Pixel, side-view strips ~150px | none stated | 🟡 Goblin, Skeleton, Mushroom, Flying Eye — full idle/walk/attack/hit/death. Best monster variety, but **side-view** (not top-down) — needs care. |
| Skeletons_Free_Pack | Pixel strips 64px | none stated | 🟡 Sword skeleton, 2 colors, 6 anims, with/without VFX |
| Golems_Free_Version | Pixel strips 64px | none stated | 🟡 Golem, 2 colors, 5 anims — good heavy/boss enemy |
| Forest_Monsters_FREE | Pixel strips 64px | none stated | 🟡 Mushroom, 7 anims w/ VFX |
| Pixel Crawler – Free Pack (Entities) | Pixel 32px | free commercial, credit opt. | 🟡 Skeleton & Orc crews (warrior/rogue/mage variants), small scale |
| Cute_Fantasy_Free | Pixel 32px, top-down | **non-commercial** | 🟠 Skeleton+Slime (static), farm animals. License blocks shipping. |

### Characters / heroes
| Pack | Style / Res | License | What's there |
|------|-------------|---------|--------------|
| Tiny RPG (v1.03 / v1.02 / Demo) | Pixel 100px | none stated | 🟡 Soldier + Orc, full anims + arrow projectile. Close-ish to Tiny Swords scale. |
| FREE_Adventurer / FREE_Kobold | Pixel ~64–96px, 4-dir | personal+commercial, no resale | 🟡 Single hero each, idle/run/attack |
| 32rogues | Pixel 32px | **no AI-train, no resale** | 🟡 30+ character roster + monsters/items/tiles, one sheet |
| FreeCharactersAnimations | Pixel 64×96 | free commercial | 🟡 Soldier+shield, slime, w/ Aseprite sources |
| lpc_entry / expansion_pack-0.04 | Pixel 64px LPC | open/LPC | 🟠 Modular LPC human — different style from Tiny Swords |
| GandalfHardcore | Pixel modular | no resale | 🟠 Character-creator layers, not top-down |

### Terrain / tiles / buildings (beyond Tiny Swords)
| Pack | Style / Res | License | What's there |
|------|-------------|---------|--------------|
| SevenKingdoms_graphics (1 & 2) | Pixel, top-down RTS | **GPL** | 🟠 Big RTS building/terrain/faction set — GPL is viral, avoid for shipping |
| mystic_woods_free_2 | Pixel 16px top-down | **non-commercial** | 🟠 Forest tiles/props/chars |
| pixel_16_woods v2 | Pixel 16px | none stated | 🟠 Woodland tileset |
| isometric tileset / FreeEnvironment | Pixel iso / mixed | none stated | 🔴 Isometric — wrong projection |
| Sunny-land-files | Pixel platformer | public license | 🔴 Side-scroller |

### UI / icons (game currently draws UI on canvas — these could reskin it)
| Pack | Style / Res | License | What's there |
|------|-------------|---------|--------------|
| **Tiny Swords UI** (inside the two packs above) | Pixel, matches game | **CC0** | ✅ **Use this first** — buttons, ribbons, banners, bars, icons, pointers in-style |
| Shikashi's Fantasy Icons (v1 & v2) | Pixel 32px | commercial OK (game-icons CC BY 3.0) | 🟢 ~245 item/status/weapon/potion/food icons — great for upgrade & ability icons |
| Free – Raven Fantasy Icons | Pixel 16/32/64px | credit optional | 🟢 6,500+ icons, every category. Huge fallback library. |
| items | Pixel 16px | unknown | 🟡 1,243 generic item icons (no license — verify before ship) |
| Complete_UI_Essential / Book_Styles | Flat / book theme | CC BY 4.0 / custom | 🟠 Non-pixel UI — clashes with Tiny Swords |
| Humble Gift, Legacy Collection, Pixel Crawler (Icons/Weapons) | Pixel, mixed | mixed | 🟡 HUD modules, weapon icons, gem/FX sets — cherry-pick |

### Effects / VFX
| Pack | Style / Res | License | What's there |
|------|-------------|---------|--------------|
| **Tiny Swords Effects** (in-pack) | Pixel, in-style | **CC0** | ✅ Explosion + Fire sheets (already used); Free Pack adds Dust, Water Splash |
| Super Pixel Effects Gigapack | Pixel, multi | **attribution required** | 🟢 182+ frames: explosions, spells, impacts, lightning, smoke. Best for ability/magic VFX. |
| Legacy Collection (Explosions & Magic) | Pixel | public license | 🟡 Explosions, hits, death, magic FX |
| New_All_Fire_Bullet_Pixel_16x16 | Pixel 16px | unknown (.rar, not extracted) | 🟡 Fire/bullet projectiles |
| Helton Yan's Pixel Combat | **SFX, 3.8 GB** | none stated | 🎵 ~2,100 combat WAVs — see audio note |

### 🔴 Wrong medium (do not use as 2D sprites)
| Pack | What it is |
|------|------------|
| KayKit_Adventurers_2.0 / Medieval_Hexagon / ResourceBits | **3D models** (FBX/GLTF/OBJ), all **CC0** — only useful if the game ever goes 3D |

### 🎵 Audio & fonts
| Pack | License | What's there |
|------|---------|--------------|
| **xDeviruchi – 16 bit Fantasy & Adventure** | attribution: "Marllon Silva (xDeviruchi)" | 22 looped 16-bit tracks (Title, Battle 1/2, Victory, Final Battle…). **Best music fit.** |
| **Essentials_Series_NOX_SOUND** (1.1 GB) | **CC0** | Footsteps (12 surfaces), nature ambience, voices, impacts. Safe SFX source. |
| Helton Yan's Pixel Combat (3.8 GB) | none stated | Huge combat SFX set — license unclear, prefer NOX for shipping |
| **monogram** | **CC0** | Pixel font, TTF + bitmap + PICO-8. Drop-in if replacing the Fredoka web font for a pixel HUD. |

---

## Practical shortlist (what to actually reach for)

1. **Need UI / HUD / buttons / health bars** → Tiny Swords UI kit (CC0, already in-style). Don't use a foreign UI pack.
2. **Need more enemy types** → Tiny Swords goblins first; then Skeletons/Golems/Monsters_Creatures for variety (watch side-view + license).
3. **Need ability / upgrade / currency icons** → Shikashi (commercial-OK) or Raven (huge).
4. **Need spell/magic VFX** → Super Pixel Effects Gigapack (credit required) or Tiny Swords fire/explosion.
5. **Need music** → xDeviruchi (credit). **Need SFX** → NOX Sound (CC0).
6. **Avoid for shipping:** GPL (SevenKingdoms), non-commercial (mystic_woods, Cute_Fantasy), and unknown-license packs until cleared.
