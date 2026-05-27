# CP26 — Mod Info Index
Last updated: 2026-05-26

This folder contains all design docs, fix notes, and reference material for the CP26 mod.
Everything in `archive/` is historical (already implemented or superseded) — safe to ignore.

---

## Root files

| File | Content | Status |
|------|---------|--------|
| [NOTES_DESIGN.txt](NOTES_DESIGN.txt) | Ferengi quest tree proposal + AI command cap notes | Active |
| [VERSIONING.txt](VERSIONING.txt) | Version history v1.0 → current, commit log, merge strategy | Active |

---

## design/ — Reference docs & living specs

### Arcs & Gameplay
| File | Content | Status |
|------|---------|--------|
| [CP26_BORG_CRISIS.txt](design/CP26_BORG_CRISIS.txt) | HRB Borg crisis audit — 3-phase fix roadmap (Phase A–C) | Roadmap |
| [CP26_Synthetic_Crisis_Design.txt](design/CP26_Synthetic_Crisis_Design.txt) | A500 Mars synthetic crisis — 6-phase design spec | Spec |
| [CP26_GAME_START_ARC_OPTIONS.txt](design/CP26_GAME_START_ARC_OPTIONS.txt) | Game-start popup (cp26_options.1) arc toggles | Implemented |
| [CP26_WAR_AND_VASSALIZATION_WORKLOG.txt](design/CP26_WAR_AND_VASSALIZATION_WORKLOG.txt) | War/diplomacy system — current state worklog | Active |
| [CP26_WAR_REWORK_OPTIONS_2026-05-17.txt](design/CP26_WAR_REWORK_OPTIONS_2026-05-17.txt) | War rework options analysis — recommends Option C | Strategic ref |
| [FERENGI_QUEST_TREE_PROPOSAL.txt](design/FERENGI_QUEST_TREE_PROPOSAL.txt) | Ferengi quest tree — 15 nodes, 5 branches, full spec | Ready to implement |

### Ships & Fleets
| File | Content | Status |
|------|---------|--------|
| [CP26_MAJOR_FACTIONS_TECH_TREE_PLAN_2026-05-26.txt](design/CP26_MAJOR_FACTIONS_TECH_TREE_PLAN_2026-05-26.txt) | Tech tree plan for Klingon/Romulan/Cardassian/Ferengi | Active standard |
| [CP26_SHIP_FLEET_CAP_SYSTEM_2026-05-14.txt](design/CP26_SHIP_FLEET_CAP_SYSTEM_2026-05-14.txt) | 36-ship cap, command_limit base/max, doctrine spec | Implemented |
| [CP26_WARP_MOVEMENT_DESIGN.txt](design/CP26_WARP_MOVEMENT_DESIGN.txt) | Warp range bypass + speed scaling tech tree | Implemented |

### Empires & Species
| File | Content | Status |
|------|---------|--------|
| [CP26_HOW_TO_ADD_CONTENDER_EMPIRE.txt](design/CP26_HOW_TO_ADD_CONTENDER_EMPIRE.txt) | 13-step guide to adding new major empires | Reference template |
| [CP26_MINOR_FACTION_BALANCE_LORE.txt](design/CP26_MINOR_FACTION_BALANCE_LORE.txt) | Tholian/Gorn/Breen/Orion balance adjustments | Implemented |
| [CP26_MINOR_SPECIES_CLASSES.txt](design/CP26_MINOR_SPECIES_CLASSES.txt) | 20 minors → 3 archetype classes (Phase 1 done) | Phase 2 pending |
| [CP26_MINOR_SPECIES_TRAITS.txt](design/CP26_MINOR_SPECIES_TRAITS.txt) | 60 unique traits for 20 minors — full spec | Implemented |

### AI
| File | Content | Status |
|------|---------|--------|
| [CP26_AI_EXPANSION_ANALYSIS.txt](design/CP26_AI_EXPANSION_ANALYSIS.txt) | Root cause of AI stagnation — 5 factors identified | Reference |
| [CP26_AI_IMPROVEMENT_PLAN.txt](design/CP26_AI_IMPROVEMENT_PLAN.txt) | AI roadmap Level 1–6 (Level 1 done, 2–6 planned) | Roadmap |
| [CP26_AI_SHIPYARD_FIX_EXPLANATION.txt](design/CP26_AI_SHIPYARD_FIX_EXPLANATION.txt) | cp26_ai_shipyard.1 — capital starport/shipyard fix | Implemented |

### Map & Systems
| File | Content | Status |
|------|---------|--------|
| [CP26_GALAXY_RADIUS_ENGINE_LIMIT.txt](design/CP26_GALAXY_RADIUS_ENGINE_LIMIT.txt) | **CRITICAL** — engine hard limit radius ≤ 500 (SIGSEGV above) | Constraint |
| [CP26_GORN_STRICT_SPAWN_EXPLANATION.txt](design/CP26_GORN_STRICT_SPAWN_EXPLANATION.txt) | Gorn spawn tightening — anchor initializers, 2-jump buffer | Implemented |
| [CP26_RURA_PENTHE_SYSTEM_2026-05-19.txt](design/CP26_RURA_PENTHE_SYSTEM_2026-05-19.txt) | Rura Penthe — colonized planet inside Beta Penthe | Implemented |
| [CP26_SOL_NOVA_REPLACEMENT_2026-05-17.txt](design/CP26_SOL_NOVA_REPLACEMENT_2026-05-17.txt) | Terra Nova replacing Suliban in Sol system | Implemented |

### Debug & Tutorials
| File | Content | Status |
|------|---------|--------|
| [CP26_DEBUG_CONSOLE_ARC_BAJOR.txt](design/CP26_DEBUG_CONSOLE_ARC_BAJOR.txt) | Console testing guide — Bajor Liberation arc | Active |
| [CP26_DEBUG_CONSOLE_ARC_MARS.txt](design/CP26_DEBUG_CONSOLE_ARC_MARS.txt) | Console testing guide — A500 Mars + Romulan Synth | Active |
| [CP26_TUTORIAL_MISSION_TREE_2026-05-26.txt](design/CP26_TUTORIAL_MISSION_TREE_2026-05-26.txt) | Tutorial: add/modify a quest tree node (FR) | Reference |
| [CP26_TUTORIAL_MISSION_TREE_2026-05-26_EN.txt](design/CP26_TUTORIAL_MISSION_TREE_2026-05-26_EN.txt) | Tutorial: add/modify a quest tree node (EN) | Reference |

### Music
| File | Content | Status |
|------|---------|--------|
| [CP26_ST_SOUNDTRACK_INTEGRATION_2026-05-18.txt](design/CP26_ST_SOUNDTRACK_INTEGRATION_2026-05-18.txt) | Music integration spec — vanilla + workshop merge | Implemented |

---

## fixes/ — Active fix notes

| File | Content |
|------|---------|
| [CP26_FIX_A_VOIR.txt](fixes/CP26_FIX_A_VOIR.txt) | **Active backlog** — known issues, pending and resolved |
| [CP26_CRASH_FIXES.txt](fixes/CP26_CRASH_FIXES.txt) | Crash fix reference (undated, permanent reference) |
| [CP26_CONSTRUCTION_QUEUE_CRASH_FIX.txt](fixes/CP26_CONSTRUCTION_QUEUE_CRASH_FIX.txt) | Construction queue crash fix |
| [CP26_BORG_AND_SHIP_SIZE_LOCALIZATION_FIX.txt](fixes/CP26_BORG_AND_SHIP_SIZE_LOCALIZATION_FIX.txt) | Borg + ship size loc fix |
| [CP26_BORG_CRISIS_SAFE_CHAIN_PATCH_2026-05-27.txt](fixes/CP26_BORG_CRISIS_SAFE_CHAIN_PATCH_2026-05-27.txt) | Borg crisis situation log + The Freed chain patch |
| [CP26_HERO_SHIP_BALANCE_FIX.txt](fixes/CP26_HERO_SHIP_BALANCE_FIX.txt) | Hero ship balance fix |
| [CP26_MINOR_BATTLESHIP_SLOT_FIX.txt](fixes/CP26_MINOR_BATTLESHIP_SLOT_FIX.txt) | Minor empire battleship slot fix |
| [CP26_MISSION_EVENTS_MINOR_EMPIRES_FIX.txt](fixes/CP26_MISSION_EVENTS_MINOR_EMPIRES_FIX.txt) | Mission event fix for minor empires |
| [CP26_SOLDIER_CAMP_REMUS_FIX.txt](fixes/CP26_SOLDIER_CAMP_REMUS_FIX.txt) | Remus soldier camp fix |
| [CP26_FIX_country.27500_EXPLANATION.txt](fixes/CP26_FIX_country.27500_EXPLANATION.txt) | country.27500 error explanation |
| [CP26_FERENGI_SHIPSET_EXPANSION_2026-05-26.txt](fixes/CP26_FERENGI_SHIPSET_EXPANSION_2026-05-26.txt) | Ferengi shipset expansion (today) |
| [CP26_KLINGON_ROMULAN_CARDASSIAN_VISIBLE_HULL_BRANCHES_2026-05-26.txt](fixes/CP26_KLINGON_ROMULAN_CARDASSIAN_VISIBLE_HULL_BRANCHES_2026-05-26.txt) | Hull branch visibility fix (today) |
| [CP26_ROMULAN_SCIMITAR_MISSION_AND_TECH_2026-05-26.txt](fixes/CP26_ROMULAN_SCIMITAR_MISSION_AND_TECH_2026-05-26.txt) | Romulan Scimitar mission + tech (today) |

---

## changelog/

| File | Content |
|------|---------|
| [CHANGELOG_INTERNE.txt](changelog/CHANGELOG_INTERNE.txt) | Full internal changelog |
| [CHANGELOG_STEAM_2026-05-25.txt](changelog/CHANGELOG_STEAM_2026-05-25.txt) | Steam changelog — last public release |
| [CP26_MEGASTRUCTURE_STANDARDIZATION_CHANGELOG_2026-05-22.txt](changelog/CP26_MEGASTRUCTURE_STANDARDIZATION_CHANGELOG_2026-05-22.txt) | Megastructure standardization changes |
| [CP26_OCCUPIED_STATE_REWORK_CHANGELOG_2026-05-21.txt](changelog/CP26_OCCUPIED_STATE_REWORK_CHANGELOG_2026-05-21.txt) | Occupied state rework changes |
| [CP26_WORKSHOP_MEGASTRUCTURE_FUSION_CHANGELOG_2026-05-22.txt](changelog/CP26_WORKSHOP_MEGASTRUCTURE_FUSION_CHANGELOG_2026-05-22.txt) | Workshop megastructure fusion changes |

---

## interface/

| File | Content |
|------|---------|
| [BINARY_INFO.txt](interface/BINARY_INFO.txt) | Binary/DDS format notes |
| [CUSTOM_ICONS.txt](interface/CUSTOM_ICONS.txt) | Custom icon implementation guide |
| [EMPIRE_SELECTION_SCREEN.md](interface/EMPIRE_SELECTION_SCREEN.md) | Empire selection screen layout |
| [HOW_TO_INTERFACE_BUTTON_EFFECTS.txt](interface/HOW_TO_INTERFACE_BUTTON_EFFECTS.txt) | GUI button effect scripting |

---

## release/

| File | Content |
|------|---------|
| [CREDITS.txt](release/CREDITS.txt) | Credits |
| [STEAMDESC_EN.txt](release/STEAMDESC_EN.txt) | Steam description (English) |
| [STEAMDESC_FR.txt](release/STEAMDESC_FR.txt) | Steam description (French) |
| [CP26_STEAM_CHANGELOG_SHORT.txt](release/CP26_STEAM_CHANGELOG_SHORT.txt) | Short Steam changelog (EN) |
| [CP26_STEAM_CHANGELOG_SHORTFR.txt](release/CP26_STEAM_CHANGELOG_SHORTFR.txt) | Short Steam changelog (FR) |
| [CP26_RELEASE_NOTE_FERENGI_V2_PLATFORM_2026-05-22.txt](release/CP26_RELEASE_NOTE_FERENGI_V2_PLATFORM_2026-05-22.txt) | Ferengi v2 platform release note |
| [CP26_RELEASE_NOTE_MEGASTRUCTURE_STANDARDIZATION_2026-05-22.txt](release/CP26_RELEASE_NOTE_MEGASTRUCTURE_STANDARDIZATION_2026-05-22.txt) | Megastructure standardization release note |
| [CP26_RELEASE_NOTE_WORKSHOP_MEGASTRUCTURE_FUSION_2026-05-22.txt](release/CP26_RELEASE_NOTE_WORKSHOP_MEGASTRUCTURE_FUSION_2026-05-22.txt) | Workshop megastructure fusion release note |

---

## archive/

Historical session notes — already implemented or superseded. Safe to ignore.
- `archive/fixes/` — 42 dated fix logs (2026-05-20 → 2026-05-25)
- `archive/audits/` — 13 session audit snapshots
- `archive/design/` — 4 superseded design docs (radius 600/760 experiments, Rura Penthe v1, Ferengi/Orion brainstorm)
