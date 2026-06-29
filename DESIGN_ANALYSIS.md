# Fortress of Honor — Design Analysis
### Making it more structured & enjoyable through established board‑game patterns

*Lenses: Reiner Knizia, Klaus Teuber, Wolfgang Kramer — plus Rosenberg, Feld, Garfield, Kramer/Ulrich "El Grande", Sid Sackson.*

---

## 0. Where the game is today

A single‑player **merge‑defense** with a campaign: each turn you **Build → Upgrade → Fight**. You draft up to 4 buildings/round into a 7×4 grid, merge same‑type to higher tiers, then a wave assaults the wall. Five building roles (Watchtower shoots; Archery/Barracks/Monastery produce Archers/Lancers/Monks; Mine makes gold), a war party that sorties, and meta progression (champions, gear, gems, honor, badges). Difficulty scales with your base strength. Solid foundations — the question is how to make the **decisions tenser and clearer**.

The recurring lesson from all three designers: **the game is the set of interesting decisions, and an interesting decision is one where every option costs you something.** Right now many FoH choices are additive ("more is better"). The work is to introduce *scarcity, opportunity cost, and legible tension.*

---

## 1. Reiner Knizia — *scarcity, multiple currencies, "you can't do everything," elegant math*

**Signature patterns**
- **Lowest‑track scoring** (Ingenious): your score is your *weakest* color → you must spread, not specialize.
- **Tight auctions** (Modern Art, Ra): a fixed pool of value, everyone bids, the tension is purely in *opportunity cost*.
- **Press‑your‑luck banking** (Ra): take the sun‑disk now or gamble for more.
- **"Theme is a coat of paint"**: the decision space is the game; keep it clean and mathematical.

**Apply to FoH**
1. **Three pillars that must be balanced — make the assault punish your weakest.**
   Offense (towers), Army (producers), Economy (mines). A Knizia "lowest counts" move: each wave the enemy **probes your weakest flank** — e.g., if you have no melee, an assassin/breacher targets the hero; if no ranged, fliers/archers chip freely; if no economy, you can't repair. You already have seeds (assassin vs. no‑melee, archers vs. no‑ranged). *Formalize it*: telegraph "This wave favors **Siege** — your wall is your weak point." This converts "build more of my favorite" into "shore up my weakness."
2. **A real build budget = an auction against yourself.** 4 buildings/round + population cap is scarcity, but it's currently soft. Sharpen: show the **opportunity cost** ("place this Mine and you give up a Tower this round"). Consider a single **resource you spend across Build *and* Upgrade** (see Kramer AP, §3) so placing vs. merging is a genuine trade.
3. **Press‑your‑luck "Keep Defending."** You already let players continue a cleared land endlessly, and the treasury compounds (with plunder risk). Make this an explicit Ra‑style decision each post‑win wave: **Bank** (leave with the reward) **or Risk** (one more wave for a growing multiplier; lose it all if the wall falls). Clean, tense, optional.
4. **Elegant legibility.** Knizia games teach in one sentence. The new Build→Upgrade→Fight bar is exactly this — keep ruthlessly pruning UI noise.

---

## 2. Klaus Teuber — *engine‑building, the robber (rubber‑banding), modular setup, interaction*

**Signature patterns**
- **Compounding engine** (Catan): early investment snowballs.
- **The robber / "gang up on the leader"**: the game pushes back on whoever's ahead → comebacks, no runaway.
- **Variable setup**: re‑randomized board every play → replayability.
- **Trade/negotiation**: in solo, this becomes *negotiating with the system's economy.*

**Apply to FoH**
1. **Make the rubber‑band legible (the robber).** `enemyPowerHp/Dps` already scales to your base strength — this *is* Catan's leader‑targeting. Surface it as a **Threat meter** ("Your might draws a fiercer host"), so the player feels the pushback is a fair, readable rule, not random spikes. Reward overcoming it (bonus stars/gems for winning at high threat).
2. **Lean into the merge engine as the snowball** — but gate it so it can't trivialize (the pop cap + 4/round are the brakes; keep them). A satisfying engine needs a visible **ramp**: "this Lv6 Watchtower took 8 buildings to make."
3. **Variable setup per land (modular boards).** Each land should re‑randomize meaningfully: obstacle layout (done), **a land modifier** ("rocky: 2 plots blocked", "fertile: mines +50%", "exposed: wall starts at 70%"), and its signature foe. This is Catan's variable number tokens — huge replayability for low cost.
4. **Negotiation with the economy = the gem↔gold exchange + the compounding treasury.** Keep these; they're your "trades."

---

## 3. Wolfgang Kramer — *the scoring track & constant tension, area majority (El Grande), action‑point allowance (Tikal)*

**Signature patterns**
- **The Kramer track**: a shared, ever‑present score/tension track.
- **Area majority** (El Grande): control of regions matters; placement is a spatial fight.
- **Action‑point allowance** (Tikal/Torres): a budget of points spent freely across action types → flexible, agonizing choices.

**Apply to FoH**
1. **Area majority / coverage as the spatial puzzle.** The battlefield has lanes and a wall; towers have *limited range* (you added this). Turn placement into an El Grande‑style coverage decision: **lanes** the enemy marches down, **overlapping tower fields**, **chokepoints** at the gate/breach. "Where" should matter as much as "what." A simple first step: show range rings (done) + a faint **threat heatmap** of where enemies path.
2. **Action‑Point Allowance for the turn.** Generalize "4 buildings" into **N action points/turn** spent on: *place* (1), *merge* (1), *repair wall* (1), *change tower element* (1), *sell* (refund). Now Build and Upgrade draw from one budget → every action has opportunity cost (pure Kramer/Tikal). Keeps the new 3‑step flow but gives it teeth.
3. **A persistent tension track.** The 8‑beat **wave rhythm** is already a Kramer track — keep it visible (you telegraph it). The campaign map is a second track. Consider a per‑land **"hold the line" track**: survive N waves, each notch raises reward and threat.

---

## 4. Supporting lenses

- **Uwe Rosenberg (worker placement, feeding pressure).** The wall is your "feeding" obligation: each turn it decays / costs upkeep to hold, so you can't pour everything into offense. Repair = a worker action competing for your AP. *Tight economies make every coin a decision.*
- **Stefan Feld (point salad + luck mitigation).** You have many reward streams (gold/gems/stars/honor) — good point salad. Ensure RNG (tower misses, rare specials) is **mitigable** by choices (a tower element or upgrade that "never misses"), so luck adds texture without feeling unfair.
- **Richard Garfield (collection + combos).** The Codex can grow from a bestiary into a light **synergy layer**: building *adjacency/synergy bonuses* (Tower next to Monastery gains frost rounds; 3 Mines = a "vein" bonus). Combos are the reason to build *this* board, not just the strongest pieces.
- **Sackson/“I’m the Boss” & negotiation** → not applicable solo, but the *risk/reward* spirit lives in the press‑your‑luck endless mode.

---

## 5. Prioritized recommendations

**Tier 1 — high impact, low cost (do first):**
1. **Threat meter** that visualizes the rubber‑band (Teuber). *Makes scaling feel fair + creates a target to beat.*
2. **Wave intent telegraph** = "this beat hits your WEAK pillar" (Knizia lowest‑track). *Turns building into shoring‑up.*
3. **Press‑your‑luck "Bank or Risk"** on the post‑win endless (Knizia/Ra). *Optional depth, big tension.*
4. **Land modifiers** per node — 1 random twist each land (Teuber variable setup). *Cheap replayability.*

**Tier 2 — medium cost, deepens the core:**
5. **Action‑Point turn budget** unifying place/merge/repair/element (Kramer AP). *Every action an opportunity cost.*
6. **Lanes + coverage** so tower placement is spatial majority (Kramer/El Grande). *“Where” becomes a decision.*
7. **Wall upkeep / feeding** so defense competes with offense for AP (Rosenberg).

**Tier 3 — long‑term richness:**
8. **Building synergies / adjacency** (Garfield combos). *Reason to compose a board.*
9. **Star rating rewards balance** across the three pillars (Knizia lowest‑counts). *Anti‑specialization.*

---

## 6. What to avoid
- **Additive‑only upgrades** ("+10% everything"): they remove decisions. Prefer *trade‑offs* (Fire tower: +splash, −single‑target; Mine: gold but no defense).
- **Unmitigable randomness** spikes — keep RNG mild and answerable.
- **Feature creep over legibility** — every system must survive the "explain in one sentence" test (Knizia). The Build→Upgrade→Fight bar is the gold standard; hold new systems to it.

---

*Summary:* The bones are good. The leap from "satisfying toy" to "great game" is **opportunity cost everywhere** — one budget across actions (Kramer), assaults that punish your weakest pillar (Knizia), a visible rubber‑band (Teuber), and placement that's a spatial puzzle (El Grande). Start with the four Tier‑1 items: they're cheap, they're legible, and they convert "build more" into "make hard choices."
