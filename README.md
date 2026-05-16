# Community Patch 2026 — Star Trek: Infinite

Unified community patch for **Star Trek: Infinite**. Merged mods, crash fixes, rebalancing, and new narrative content — all in one stable, actively maintained package.

> *Star Trek: Infinite deserved better. We're on it.*

---

## What This Is

CP26 started as a compatibility layer between Workshop mods and grew into a full maintenance and expansion project. The game was abandoned before completion. This patch picks up where the developers left off.

**It is a work in progress and will keep evolving.**

---

## New Content

### Arc — Utopia Planitia Rebellion *(Testing)*
Narrative arc for the UFOP, inspired by *Star Trek: Picard*.

- New technology: **A500 Synthetic Androids** — deployable as industrial workers on colonies
- New planetary decision: **Deploy A500 Units** — adds 3 synthetic pops and a production bonus
- If deployed on **Mars**: the Rebellion arc triggers after 5 in-game years
- Two resolution paths: **military** (troop transport + ground combat) or **diplomatic** (science vessel)
- Rewards: Neo-Constitution, Sagan, Duderstadt and Odyssey ship designs added to the research pool
- Mission log tracking in the situation log until resolution
- Post-arc management: species profile *Synthetic Worker* (0 housing, 0 amenities, migration locked) and edict *Ban Synthetic Workers* (permanent, irreversible)
- Can be disabled in the game-start popup

### Romulan Objective — Synth Project *(Testing)*
- When the Federation activates A500 units, a 90-day window opens to send a spy ship to Earth
- Success unlocks a national decision to trigger a synthetic rebellion on an enemy planet

---

## Ship Classes

### New UFOP Classes (unlocked via A500 arc)

| Class | Role | Tier |
|---|---|---|
| Neo-Constitution | Versatile exploration cruiser | 2 |
| Sagan / Stargazer | Fast powerful explorer | 2+ |
| Duderstadt | Advanced heavy cruiser | 3 |

### Integrated Classes

- Ambassador / Akira / Constellation / Luna / New Orleans / Odyssey
- California Class (support)
- USS Voyager (hero) / Enterprise-E (fixed)
- K'Vort / Kamarag / Raptor (Klingon)
- USS Lantree

### Canonical Ship Sizes

Full size overhaul across all factions. Reference: **Luna = 454m → size 5**.

All ships rescaled proportionally with coherent per-faction visual progression:
UFOP, Klingon, Romulan, and Cardassian ships corrected. Notable changes: D'deridex (1042m) significantly increased, Sovereign and Negh'Var kept as flagships with clear visual distinction from lower tiers.

---

## AI & Diplomacy

- AI empires start with a shipyard
- AI evaluates military strength before declaring war, adapted to galaxy size
- Vassalization wars and liberation wars restored
- "Propose Vassalization" diplomatic action restored
- Minor factions with lore-faithful personalities and behaviors
- First Contact bonuses fixed

---

## Balance

- Hero ships rebalanced
- Minor faction distinct combat identities: Gorn (resilient), Tholian (shield regen), Orion (fast), Breen (heavy)
- Military fleet cap system — coherent limits across all factions
- Planetary economy rebalanced

---

## Stability & Crash Fixes

11+ documented crash fixes including:

- **Combat freeze** (infinite loop on ships with 0 crew — negative fire rate)
- **Galaxy generation crash** (SIGFPE — missing Bajoran system initializer)
- SIGSEGV crashes in planet/construction/district/diplomatic action systems
- Event and save stability improvements

All fixes documented in [`Mod info/CP26_CRASH_FIXES.txt`](Mod%20info/CP26_CRASH_FIXES.txt).

---

## Factions & Restored Content

- Breen Confederacy / Gorn Hegemony / Orion Empire / Tholian Assembly
- Remans and Remus / Denobulans / Risans
- Colonized Terra Nova, Vega, Mars, Luna
- Restored Federation colonies

---

## Game-Start Options

Configure at launch via popup:

| Option | Default |
|---|---|
| Pirates | On |
| Diplomatic Vassalization | On |
| Supernova Quest | On |
| A500 Synthetic Rebellion arc | On |

---

## Installation

Enable **only Community Patch 2026** in the launcher.  
Do not enable included source mods separately — CP26 already merges and overrides their content. Loading both will reintroduce conflicts.

**Recommended:** Star Trek: Infinite 1.0.7 + CP26 only.

---

## Integrated Mods

CP26 integrates content from:

Ongoing Bug Fixes · Integration Adjustments · Nebula Refinery Expanded · Federation of War · USS Voyager Event Fix · Enterprise-E Fixed · Ambassador Class · Akira Class · Constellation Class · New Orleans Class · USS Lantree · Canonized Map and Events · Colonized Terra Nova · Colonized Vega · Mars and Luna · Extended Name Lists · The Breen Confederacy · The Gorn Hegemony · The Orion Empire · The Tholian Assembly · Remans and Remus · Risa and Risans · Denobula and Denobulans · Data Is Immortal!

Full credits: [CREDITS.txt](CREDITS.txt)

---

## Contributing

Bug reports, balance feedback, and localisation contributions are welcome.

When reporting an issue, include:
- Game version and enabled mods
- Faction played
- New game or existing save
- Relevant lines from `error.log`

---

*All original mod content belongs to its Workshop authors. CP26 exists to preserve, fix, and connect this work for Star Trek: Infinite players.*

**Engage.**
