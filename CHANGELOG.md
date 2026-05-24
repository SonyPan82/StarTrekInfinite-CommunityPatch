# CP26 — Changelog

---

## [1.6.9] — 2026-05-24

### Musée de la Flotte — Enterprise-D & Enterprise-E (stages visuels)

**Contexte** : L'Enterprise-D (Galaxy class) et l'Enterprise-E (Sovereign class) sont maintenant physiquement visibles dans le Musée de la Flotte après leur décommissionnement, via un système de stages de mégastructure débloqués par événement.

#### Mégastructure (`common/megastructures/cp26_fleet_museum.txt`)
- Ajout de `cp26_fleet_museum_stage2` : stage visuel activé après le décommissionnement de l'Enterprise-D. Coût : 50 influence + 500 minéraux + 100 dilithium. Temps de construction : 180 jours. Bloqué tant que le flag `cp26_museum_galaxy_unlock` est absent.
- Ajout de `cp26_fleet_museum_stage3` : stage visuel activé après le décommissionnement de l'Enterprise-E. Même coût. Bloqué tant que `cp26_museum_sovereign_unlock` est absent.
- Les deux stages utilisent `upgrade_from` pour former une chaîne linéaire : musée de base → stage2 (Galaxy) → stage3 (Sovereign).

#### Événements popup (`events/cp26_enterprise_odyssey_events.txt`)
- Ajout de `namespace = cp26_museum`.
- `cp26_museum.1` : popup proposé au joueur après la fin de la quête Enterprise Galaxy (décommissionnement Enterprise-D). Option A pose le flag `cp26_museum_galaxy_unlock` et déverrouille le bouton d'amélioration du musée. Option B ignore.
- `cp26_museum.3` : popup proposé 30 jours après la fin du projet Odyssey Refit (décommissionnement Enterprise-E). Option A pose `cp26_museum_sovereign_unlock`. Option B ignore.
- Dans `cp26_enterprise_odyssey.2` : ajout d'un déclenchement conditionnel de `cp26_museum.3 days = 30` si le musée existe (`cp26_fleet_museum_spawned`).

#### Quest tree (`common/quest_tree_nodes/qt_ufop_b7.txt`)
- `on_finished` de la quête Enterprise Galaxy envoie `cp26_museum.1 days = 30` si le musée existe.

#### Entités visuelles (`gfx/models/ships/ufop/fleet_museum/sti_fleet_museum.asset`)
- Ajout de `sti_museum_enterprise_galaxy_entity` : mesh `cp26_museum_galaxy_mesh`, scale 0.40, animation idle.
- Ajout de `sti_museum_enterprise_sovereign_entity` : mesh `cp26_museum_sovereign_mesh`, scale 0.70, animation idle.
- Ajout de `sti_fleet_museum_stage2_entity` : musée + Galaxy en `part6`.
- Ajout de `sti_fleet_museum_stage3_entity` : musée + Galaxy en `part6` + Sovereign en `part1`.
- Constitution Refit : scale augmenté 0.70 → 1.00.

#### Meshes et textures (nouveaux assets CP26)
- `gfx/models/ships/ufop/fleet_museum/museum_ships/galaxy/` : mesh Galaxy class (`cp26_museum_galaxy.mesh`) + toutes les textures associées, copiés depuis STNHFR.
- `gfx/models/ships/ufop/fleet_museum/museum_ships/sovereign/` : mesh Sovereign class (`cp26_museum_sovereign.mesh`) + textures, copiés depuis STNHFR.
- `gfx/models/ships/ufop/fleet_museum/museum_ships/cp26_museum_ships.gfx` : définitions pdxmesh pour `cp26_museum_galaxy_mesh` et `cp26_museum_sovereign_mesh`.

#### Special projects supprimés
- `MUSEUM_ADD_GALAXY` et `MUSEUM_ADD_SOVEREIGN` dans `00_projects_hornblower.txt` supprimés (système remplacé par popup + flag + upgrade de mégastructure).

---

### Libération de Bajor — Arc événementiel 2369

**Contexte** : En 2369, chaque grand empire reçoit un événement unique relatif à la libération de Bajor du joug cardassien. L'arc est désactivé par défaut (option TESTING) et peut être activé dans les options de démarrage.

#### Événements (`events/cp26_bajor_liberation_events.txt`) — nouveau fichier
- `cp26_bajor.1` — **Fédération** : 3 choix — non-intervention (+100 influence), intervention militaire (−1 500 alliages, +200 influence, +500 unité → libération), aide humanitaire (−1 000 nourriture, +50 influence, +150 unité, 30 % chance libération).
- `cp26_bajor.2` — **Union Cardassienne** : écraser (−2 000 alliages −1 000 énergie −300 unité, garnison de répression sur Bajor), renforcer (−2 000 énergie −500 alliages), se retirer (−200 influence → libération + opinions favorables Fédération/Klingons).
- `cp26_bajor.3` — **Empire Klingon** : soutenir Bajor (−1 000 alliages +150 influence → libération), casus belli contre les Cardassiens (+250 influence), ignorer (+100 unité).
- `cp26_bajor.4` — **Empire Romulien** : soutien clandestin (−500 énergie, modificateur sabotage sur Bajor 720 jours), observer (+200 influence +500 recherche physique), intervenir officiellement (−1 200 alliages → libération).
- `cp26_bajor.5` — **Alliance Ferengi** : vendre aux Bajorans (+3 000 énergie → libération), vendre aux Cardassiens (+3 000 énergie, opinion favorable), vendre aux deux (+5 000 énergie, opinions négatives générales).
- `cp26_bajor.10` — effet commun masqué : pose le flag global `cp26_bajor_liberated`, transfère Bajor au pays bajoran si occupé par les Cardassiens.
- Déclenchement : pulse annuel, `date > 2369.1.1` et `NOT { date > 2370.1.1 }`, flag de garde par empire pour ne tirer qu'une seule fois.

#### Modificateurs d'opinion (`common/opinion_modifiers/cp26_bajor_opinions.txt`) — nouveau fichier
- `cp26_bajor_supported_liberation` : +40 opinion, 120 mois.
- `cp26_bajor_opposed_liberation` : −80 opinion, 120 mois.
- `cp26_bajor_armed_cardassians` : +60 opinion, 60 mois.
- `cp26_bajor_armed_rebels` : −40 opinion, 60 mois.
- `cp26_bajor_humanitarian_aid` : +25 opinion, 72 mois.

#### Modificateurs de planète (`common/static_modifiers/cp26_bajor_modifiers.txt`) — nouveau fichier
- `cp26_bajor_suppressed_garrison` : −20 stabilité, −15 % production (garnison cardassienne, 1 800 jours).
- `cp26_bajor_romulan_sabotage` : −10 stabilité, −10 % production (sabotage romulien, 720 jours).

#### Options de démarrage (`events/cp26_game_start_options.txt`)
- Ajout du toggle **Libération de Bajor (TESTING)** : ACTIVÉ / DÉSACTIVÉ.
- **Désactivé par défaut** : le flag `cp26_block_bajor_arc` est posé automatiquement dans le `immediate` de l'événement au premier lancement. Le joueur doit cliquer "DÉSACTIVÉ" pour activer l'arc.

#### Localisation (EN + FR)
- Toutes les clés Bajor ajoutées dans `zz_cp26_patch_l_english.yml` et `zz_cp26_patch_l_french.yml` : titre commun, 5 descriptions d'empire, toutes les options et tooltips, noms et descriptions des modificateurs de planète, labels du toggle.

---

## [1.6.8] — 2026-05-24

### Gorn — Portraits
- **`gfx/portraits/portraits/portraits_gorn.txt`** : `clothes_selector` changé de `"reptilian_massive_clothes_13"` → `"no_texture"` sur `gorn_01`. Les Gorn n'affichent plus les uniformes génériques Stellaris.

### Gorn — Ready Room (contact perso)
- **`interface/aa_gorn_stuff.gfx`** : Ajout des sprites manquantes pour l'alias `_01` utilisé par le moteur lors de la recherche de salle de contact :
  - `GFX_ST_empty_room_gorn_01`
  - `GFX_ST_capital_empty_room_gorn_01`
  - `GFX_ST_empty_room_gorn_01_loadgame`

### Limite de commandement des flottes — Refonte
**Objectif** : déplacer la progression de limite de commandement des techs vers les bâtiments militaires, rendre la montée en puissance plus tactique et dépendante du développement planétaire.

#### Bâtiments militaires (`common/buildings/20_hornblower_military_buildings.txt`)
- Nouveau fichier CP26 (copie + modifications du jeu de base).
- **`building_stronghold`** (Base Militaire) : ajout `country_command_limit_add = 4` via `country_modifier`.
- **`building_fortress`** (QG Militaire) : ajout `country_command_limit_add = 8` via `country_modifier`.

#### Techs société (`common/technology/00_hornblower_soc_techs.txt`)
- Nouveau fichier CP26 (copie + modifications du jeu de base).
- `tech_fleet_command_limit_tier_1` : `country_command_limit_add` **15 → 10**.
- `tech_fleet_command_limit_tier_2` : `country_command_limit_add` **25 → 15**.
- `tech_fleet_command_limit_tier_3` : **supprimée**.
- `tech_fleet_command_limit_tier_4` : **supprimée**.

#### Tech répétable (`common/technology/00_hornblower_repeatable_techs.txt`)
- Nouveau fichier CP26 (copie + modifications du jeu de base).
- `tech_soc_repeatable_3` : `country_command_limit_add = 10` **supprimé** (garde `starbase_defense_platform_capacity_add = 2`).
- Prerequisite mis à jour : `tech_fleet_command_limit_tier_4` → `tech_fleet_command_limit_tier_2`.

#### Résumé du budget de commandement post-refonte
| Source | Valeur |
|---|---|
| Base | +20 |
| Tech T1 | +10 |
| Tech T2 | +15 |
| Base Militaire (par planète) | +4 |
| QG Militaire (par planète, mineurs) | +8 |
| Cap absolu | 200 |

#### Fichiers `.mod` mis à jour
- **`descriptor.mod`** et **`CP26.mod`** : ajout de trois nouvelles entrées `replace_path` :
  - `common/buildings/20_hornblower_military_buildings.txt`
  - `common/technology/00_hornblower_repeatable_techs.txt`
  - `common/technology/00_hornblower_soc_techs.txt`

---

## [1.6.7] — sessions précédentes

- Ferengi, Gorn, Tholian : ajout des races, portraits, systèmes solaires, bâtiments, événements de vassalisation.
- Fontes de carte Ferengi.
- Corrections de bugs divers (voir commits git).
