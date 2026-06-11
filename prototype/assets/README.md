# prototype/assets — in-use manifest & how to add more

Every PNG here is copied from **Tiny Swords** by Pixel Frog (**CC0**) under a short name.
The game loads them by short name in `prototype/index.html`:

```js
im.src = 'assets/' + name + '.png';        // I(name) loader, ~line 2080
```

To add a sprite: copy the source PNG here with a short name, add that name to the
preload array (the `[...].forEach(I)` list, ~line 2085), then draw it.

## How sprites are drawn (conventions)

| Helper | Use | Frame convention |
|--------|-----|------------------|
| `img(name, x, y, s, ay)` | whole-image (buildings, deco, resources) | draws full PNG |
| `spr(name, fw, fh, col, row, x, y, s, flip, ay)` | animated sheets (units, fire, foam) | `fw×fh` = one frame; `col,row` index into the grid |
| `tileDraw(col, row, x, y, d)` | terrain | `tiles.png` sliced at **64px** |

**Unit sheets are 192px frames.** Row 0 = idle, row 1 = run, row 2 = attack; columns are
animation frames (Pawn = 6×6 grid = 1152px; Archer = 8×7 grid). Buildings are static PNGs;
`_x` suffix = the pack's `*_Destroyed` variant.

## What's here now → where it came from

Source root: `asset library/Tiny Swords (Update 010)/`

| Short name(s) | Source path (under Update 010) |
|---------------|-------------------------------|
| `castle_b/p/r`, `castle_x` | `Factions/Knights/Buildings/Castle/Castle_{Blue,Purple,Red,Destroyed}` |
| `house_b/p/r`, `house_x` | `Factions/Knights/Buildings/House/House_{…}` |
| `tower_b/p/r`, `tower_x` | `Factions/Knights/Buildings/Tower/Tower_{…}` |
| `pawn_b/p/r/y` | `Factions/Knights/Troops/Pawn/{color}/Pawn_{color}` |
| `warrior_b/r` | `Factions/Knights/Troops/Warrior/{color}/…` |
| `archer_r`, `arrow` | `Factions/Knights/Troops/Archer/Red/…`, `Archer/Arrow/Arrow` |
| `torch_r/y/p`, `tnt_r/y`, `dynamite`, `barrel_r` | `Factions/Goblins/Troops/{Torch,TNT,Barrel}/…` |
| `tiles` | `Terrain/Ground/Tilemap_Flat` (64px tiles) |
| `water`, `foam`, `rocks` | `Terrain/Water/{Water, Foam/Foam, Rocks/Rocks_0x}` |
| `tree`, `sheep` | `Resources/Trees/Tree`, `Resources/Sheep/HappySheep_*` |
| `deco01…16` | `Deco/0x.png` |
| `explosions`, `fire` | `Effects/{Explosion/Explosions, Fire/Fire}` |

## Unused-but-useful — pull more from the SAME source

These are all Tiny Swords (CC0, in-style), not yet copied in. Priority order:

### 1. UI kit — replace canvas-drawn HUD (highest leverage)
`Tiny Swords (Update 010)/UI/` — `Buttons/` (Blue/Red, hover/pressed/disabled, 3- & 9-slice),
`Banners/`, `Ribbons/` (Blue/Red/Yellow), `Icons/` (Regular/Pressed/Disable 01–10),
`Pointers/` (6). Plus **health bars** in `Tiny Swords (Free Pack)/UI Elements/UI Elements/Bars/`
(`BigBar_Base/Fill`, `SmallBar_Base/Fill`, 320×64) — ideal for wall/building HP and wave bar.

### 2. Goblin faction buildings — enemy structures / 2nd faction
`Update 010/Factions/Goblins/Buildings/{Wood_House, Wood_Tower}` (Wood_Tower in 4 colors,
plus `_Destroyed` / `_InConstruction`).

### 3. Economy / resource sprites — currency, drops, HUD
`Update 010/Resources/` — `Resources/{G,W,M}_Idle|Spawn` (Gold/Wood/Meat, 128px),
`Gold Mine/GoldMine_{Active,Inactive,Destroyed}` (192×128). Use `G_Idle` as the gold-coin HUD icon.

### 4. Extra building TYPES — tier/role visual variety
`Tiny Swords (Free Pack)/Buildings/{color} Buildings/` adds **Archery, Barracks, Monastery**
and **House1/2/3** in 5 colors (192×256) — not present in Update 010. Good for distinguishing
the archer/cannon/frost tower archetypes or new building kinds.

### 5. Combat & terrain polish
- `Update 010/Factions/Knights/Troops/Dead/Dead.png` — corpse/death sprite (currently none).
- `Archer + Bow/` — layered archer (separate arms + bow) for directional aiming.
- `Terrain/Ground/Tilemap_Elevation` (cliffs), `Terrain/Bridge/Bridge_All`, `Terrain/Ground/Shadows`.
- `Free Pack/Terrain/Decorations/Clouds` (8) for parallax sky; `Bushes`, `Rocks`.

### Copy recipe

```bash
cd "asset library/Tiny Swords (Update 010)"
cp "Factions/Goblins/Buildings/Wood_Tower/Wood_Tower_Red.png" "../../prototype/assets/gtower_r.png"
cp "Resources/Resources/G_Idle.png"                            "../../prototype/assets/gold.png"
# then add 'gtower_r','gold' to the forEach(I) preload list in index.html (~line 2085)
```
