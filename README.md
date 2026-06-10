# Fortress of Honor

A merge-defense game prototype — drag Tetris-style pieces onto the keep grounds, merge
three matching buildings to grow them through five illustrated tiers, then hold the wall
against waves of a medieval horde.

**Live:** https://fortress-of-honor.pages.dev

This is an original, clean-room project: all art is drawn procedurally on an HTML5 canvas
and all code is original. It takes inspiration from the merge-defense *genre* (mechanics
are not copyrightable) but contains none of any other game's assets, code, or names.

## Run it

It's a single self-contained file — no build step.

```sh
open prototype/index.html        # macOS
# or serve the folder:
python3 -m http.server -d prototype 8080   # then visit http://localhost:8080
```

## Gameplay

- **Build phase** — drag pieces from the tray onto the 7×4 grid. Tap a selected piece to rotate.
- **Merge** — 3+ connected buildings of the same type *and* tier fuse into the next tier (max 5),
  cascading. Each tier is a distinct structure.
- **Three lines:**
  - **Archer Tower** (green) — fast single-target: post → watchtower → keep → bastion → citadel
  - **Cannon Keep** (red) — splash damage: mortar pit → bunker → bastion → twin fort → artillery castle
  - **Frost Spire** (blue) — slows enemies: rune stone → obelisk → spire → crystal tower → cathedral
- **Foes** — goblins, hooded scouts, armored ogres, and a dark-knight boss every 5th wave.
- **Economy** — earn gold from kills and wave-clear bonuses; spend on REROLL or sell buildings.

## Structure

```
prototype/index.html   # the entire game (canvas + JS, no dependencies)
screenshots/           # reference images for art direction
```

## Deploy

Hosted on Cloudflare Pages (project `fortress-of-honor`):

```sh
npx wrangler pages deploy prototype --project-name fortress-of-honor --branch main
```
