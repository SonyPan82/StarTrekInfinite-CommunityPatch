# CP26 — Audit d'Équilibre
**Date :** 2026-05-20  
**Périmètre :** Ferengi Alliance (seul empire jouable ajouté par CP26 v1.6.x)  
**Verdict global : MOD JOUABLE mais déséquilibré sur plusieurs points critiques.**

---

## Méthodologie

Tous les chiffres sont tirés directement du code source :
- `common/governments/civics/aa_ferengi_civics.txt`
- `common/traits/aa_ferengi_traits.txt`
- `common/country_types/aa_ferengi_country_type.txt`
- `common/static_modifiers/cp26_ferengi_branch_modifiers.txt`
- `common/edicts/cp26_ferengi_branch_edict.txt`
- `common/diplomatic_actions/cp26_ferengi_branch_actions.txt`
- `events/cp26_ferengi_branch_events.txt`
- `events/zz_cp26_major_start_balance.txt`
- `events/aa_ferengi_start_events.txt`
- `prescripted_countries/_04_ferengi.txt`
- `common/solar_system_initializers/aa_ferengi_initializer.txt`
- `common/defines/zz_cp26_ai_defines.txt`
- Vanilla STI : `common/country_types/00_country_types.txt`, `civics/03_corporate_civics.txt`

---

## SECTION 1 — Bugs Fonctionnels (Priorité Critique)

### 🔴 BUG CRITIQUE — Research Exchange ne produit rien

**Fichier :** `events/cp26_ferengi_branch_events.txt`, event `cp26_ferengi_branch.100`, ligne 1605

```
add_resource = { research = 10 }
```

**Problème :** `research` n'est pas une ressource valide dans le scripting Stellaris/STI.
Les ressources recherche correctes sont `physics_research`, `society_research`, `engineering_research`.
Cette ligne est silencieusement ignorée par le moteur — elle ne produit **zéro** science.

**Impact joueur :** Le joueur qui choisit "Research Exchange" dépense :
- 75 influence + 600 minerals
- Pour un retour **nul** — la branche est totalement inutile.

**C'est le bug le plus grave du mod.** Un joueur ne comprendra pas pourquoi sa production de recherche n'augmente pas.

**Correction :** Remplacer par une répartition équitable. Exemple :
```
add_resource = { physics_research = 4 }
add_resource = { society_research = 3 }
add_resource = { engineering_research = 3 }
```

---

### 🟡 ANOMALIE — Production `research = 20` dans le country_type

**Fichier :** `common/country_types/aa_ferengi_country_type.txt`, ligne 210

```
produces = {
    energy = 35
    minerals = 35
    research = 20     ← invalide ?
    ...
}
```

Même remarque : si le moteur STI n'accepte pas `research` comme ressource stockpile country-level, cette production mensuelle de base est également nulle. À confirmer en jeu avec le débogueur (console `observe` → vérifier revenus mensuels).

**Comparaison vanilla default :**
```
produces = {
    physics_research = 10
    society_research = 10
    engineering_research = 10
    ...
}
```
La version vanilla répartit sur 3 catégories. La version Ferengi utilise un identifiant unique. Si cela fonctionne dans STI (ce moteur a peut-être une ressource `research` unifiée), le bug n'existe que dans les events. À vérifier impérativement.

---

## SECTION 2 — Balance Économique des Succursales

### Tableau ROI — Payback par type de succursale

| Type              | Coût                    | Revenu mensuel | Payback (mois) | Verdict     |
|-------------------|-------------------------|----------------|----------------|-------------|
| Trade Forum       | 50 inf + 400 min        | +12 energy     | ~33 mois       | Lent        |
| Mining Consortium | 50 inf + 500 min        | +12 minerals   | ~42 mois       | Très lent   |
| Energy Exchange   | 50 inf + 500 min        | +16 energy     | ~31 mois       | Lent        |
| Research Exchange | 75 inf + 600 min        | **0** (bug)    | ∞              | **Cassée**  |
| Security Ctrs     | 50 inf + 250 alloys     | +4 alloys      | ~63 mois       | Injouable   |

_Note : minerals converti en energy_equivalent 1:1 pour simplifier la comparaison._

**Problèmes identifiés :**

1. **Security Contractors est le pire investissement du jeu.** 63 mois pour récupérer 250 alloys en retour de 4/mois. En STI, les alloys sont rares et très demandés pour la construction navale. Ce type de branche n'est jamais rentable en pratique.

2. **Les montants sont fixes et ne scalent pas.** Que la partie soit à l'année 2350 ou 2400, la branche Trade Forum produit toujours 12 energy/mois. Aucune mécanique de croissance. En late game, 12 energy/mois est négligeable — les empires majeurs produisent des milliers d'unités par mois.

3. **Le payback minimum est 31 mois (~2,5 ans in-game).** C'est long mais acceptable pour du mid-game. Pour du early-mid game, c'est trop.

**Suggestion d'équilibrage :**

Augmenter les revenus mensuels x2 à x3, ou réduire les coûts d'établissement de 30-40% :

| Type              | Coût suggéré            | Revenu suggéré          |
|-------------------|-------------------------|-------------------------|
| Trade Forum       | 30 inf + 250 min        | +20 energy/mois         |
| Mining Consortium | 30 inf + 300 min        | +20 minerals/mois       |
| Energy Exchange   | 30 inf + 300 min        | +25 energy/mois         |
| Research Exchange | 50 inf + 350 min        | +5 phys +3 soc +3 eng   |
| Security Ctrs     | 30 inf + 150 alloys     | +8 alloys/mois          |

---

## SECTION 3 — Asymétrie Joueur vs IA Ferengi

### 🟡 L'IA Ferengi a un trait supplémentaire

**Fichier :** `common/solar_system_initializers/aa_ferengi_initializer.txt`, ligne 172

L'IA créée en jeu (initializer) reçoit **3 traits** :
```
trait = "trait_thrifty"
trait = "trait_eye_for_business_ferengi"
trait = "trait_traditionalists_ferengi"
```

Le joueur Ferengi prescrit (`prescripted_countries/_04_ferengi.txt`, ligne 24) reçoit **2 traits** :
```
trait = "trait_eye_for_business_ferengi"
trait = "trait_traditionalists_ferengi"
```

**`trait_thrifty` (vanilla) :** +1 trade value per pop.  
Sur 16 pops de départ, c'est +16 trade value/mois pour l'IA, absent pour le joueur.

**Ce n'est pas intentionnel** — le joueur est plus faible économiquement que son propre clone IA. Correctif : ajouter `trait_thrifty` à la définition joueur, ou retirer de l'initializer IA.

---

## SECTION 4 — Absence de Balance de Départ

### 🟡 Ferengi est le seul empire sans bonus de départ

**Fichier :** `events/zz_cp26_major_start_balance.txt`

Les 4 empires majeurs reçoivent des ressources de départ :
| Empire       | Bonus de départ                                |
|--------------|------------------------------------------------|
| UFP          | +15000 energy, +900 min, +400 food + modifiers |
| Klingon      | +500 energy, +800 min, +200 food               |
| Cardassian   | +500 energy, +200 min, +450 food               |
| Romulan      | Aucun (déjà le plus confortable, intentionnel) |
| **Ferengi**  | **Rien**                                       |

Les Ferengi n'ont aucun runway pour les premiers mois. Sachant que leur mécanique de branch office nécessite 400-600 minerals par branche, et que le jeu démarre avec une économie de croissance, l'absence de tampon initial signifie que le joueur Ferengi doit attendre de nombreuses années avant de pouvoir établir sa première succursale.

**Suggestion :** Ajouter au moins un stockpile de départ :
```
if = {
    limit = { has_country_flag = ferengi }
    add_resource = { energy = 400  minerals = 600  influence = 50 }
}
```

---

## SECTION 5 — Balance des Civiques Ferengi

### 🟠 `civic_ferengi_rules_of_acquisition` est surpuissant

**Fichier :** `common/governments/civics/aa_ferengi_civics.txt`

```
trade_value_mult = 0.3
country_energy_produces_mult = 0.1
```

**Comparaison vanilla megacorp civics** (`03_corporate_civics.txt`) :
- Le civic corporatiste donnant le plus de trade value en vanilla : `trade_value_mult = 0.10`
- Le bonus de CP26 est **3× plus élevé** que le maximum vanilla.

Combiné à `ethic_fanatic_materialist` (autre +30% trade value classiquement), les Ferengi peuvent atteindre des multiplicateurs de commerce très au-dessus de tout empire standard.

**Note atténuante :** `potential = { always = no }` signifie que ce civic ne peut pas être pris par d'autres empires ni réattribué — c'est exclusif Ferengi. C'est acceptable pour un empire thématique unique. Mais le chiffre de +30% reste gonflé si les routes commerciales sont importantes dans STI.

**Verdict :** Potentiellement OP en late game si STI a des routes commerciales riches. À surveiller en jeu. Réduire à +20% serait plus raisonnable.

---

### 🟢 `civic_ferengi_grand_nagus` est équilibré

```
country_unity_produces_mult = 0.10
pop_political_power = -0.25
```

+10% unity est modeste. -25% pop political power affaiblit les factions (moins de demandes). Pour un empire autoritaire-corporatiste, ce tradeoff est neutre à légèrement positif. Équilibré.

---

## SECTION 6 — Traits d'Espèce Ferengi

### Bilan global : équilibré pour 1 trait positif + 1 négatif

| Trait                          | Effet                                           | Coût |
|-------------------------------|-------------------------------------------------|------|
| `trait_eye_for_business_ferengi` | +1 trade value/pop, +5% specialist output   | +2   |
| `trait_traditionalists_ferengi`  | -10% research output from jobs              | -1   |

- Le -10% recherche pénalise légèrement l'avance tech, thématiquement correct (Ferengi ne sont pas des scientifiques).
- Le +5% specialist est un bonus discret mais présent sur TOUS les spécialistes.
- Net : légèrement positif à neutre. Raisonnable.

---

## SECTION 7 — Départ et Système Initial

### 🟢 Système de départ Ferengi — standard

- **Ferenginar :** taille 16, continental/class M. Standard pour un empire majeur.
- **16 pops de départ :** Standard.
- **Districts :** 2 city + 1 generator + 1 industrial + 1 farming. Équilibre correct.
- **Flotte :** 1 corvette (Ferengi Marauder) + 1 destroyer (D'Kora Marauder).  
  Technologie unlock `tech_destroyer_unlock` fournie d'office. Comparable aux autres empires.
- **Starport avec shipyard :** Standard.

---

## SECTION 8 — Production de Base (country_type)

### Comparaison ferengi_alliance vs vanilla default

| Ressource   | Ferengi Alliance | Vanilla Default | Delta     |
|-------------|------------------|-----------------|-----------|
| Energy      | 35/mois          | 20/mois         | +75%      |
| Minerals    | 35/mois          | 20/mois         | +75%      |
| Research    | 20/mois (?)      | 30/mois (total) | –         |
| Influence   | 3/mois           | ~3/mois         | ≈ égal    |
| Unity       | 10/mois          | 5/mois          | +100%     |
| Alloys      | 10/mois          | 5/mois          | +100%     |
| Food        | 20/mois          | 10–15/mois      | +50%      |

**La production de base ferengi_alliance est significativement supérieure au vanilla default.**  
C'est une décision de conception (empire jouable qui doit survivre) mais elle dépasse la production d'un empire standard de façon non-négligeable, en particulier sur unity (+100%) et alloys (+100%).

**Note :** La comparaison n'est pas parfaite — le vanilla default a plusieurs blocs `produces` conditionnels selon la situation. Les 35/35 Ferengi sont permanents et inconditionnels.

---

## SECTION 9 — Mécanique de Succursale — Design Global

### Ce qui fonctionne bien

- **Une seule branche par empire partenaire** : empêche le spam.
- **Nettoyage automatique** si le pacte commercial est rompu : la branche s'auto-supprime. Propre.
- **Acceptation automatique** (`auto_accepted = yes`) : thématique (les Ferengi s'imposent).
- **Conditions d'accès raisonnables** : pacte commercial requis, capital upgradé, pas de mégacorp en face.
- **Limite de 23 empires partenaires** dans l'UI : suffisant pour une partie standard (4 empires majeurs + ~15 mineurs).

### Ce qui manque

- **Aucune mécanique de fermeture volontaire** : le joueur ne peut pas récupérer son investissement ni choisir de se retirer d'une relation commerciale non rentable.
- **Aucun impact diplomatique** : établir une branche ne modifie pas les relations avec l'empire hôte. Aucune friction, aucune récompense. L'hôte reçoit des jobs gratuits sans contrepartie ni risque.
- **Les modifiers bénéficient PLUS à l'hôte qu'au Ferengi.** L'hôte reçoit 1 job miner/technician/researcher/soldier gratuit sur sa planète — c'est une aide substantielle. Le Ferengi reçoit 12-16 resources/mois. L'asymétrie peut rendre l'empire hôte plus fort que le Ferengi lui-même sur le long terme.

---

## SECTION 10 — AI Balance

### 🟢 AI Ferengi correctement configurée

- `ferengi_alliance` country_type a son propre `ship_data` AI avec fractions pour chaque design de vaisseau.
- `hostile_threshold_mult` x2.5 vs Cardassia/Klingon mineurs : empêche l'IA de déclarer des guerres inconsidérées contre les empires majeurs.
- Modules complets : diplomacy, economy, military, expansion.
- `ai_weight = 0` sur l'édit de gestion des succursales : l'IA n'activera pas le menu (normal — UI only).

**Problème :** L'IA Ferengi ne peut pas utiliser la mécanique de branch office (elle est `is_ai = no` gated). C'est probablement intentionnel (trop complexe à scripter pour l'IA), mais cela signifie que l'aspect "empire commercial" de l'IA est entièrement cosmétique — elle ne génère pas de branches, uniquement le joueur peut le faire.

---

## SECTION 11 — Defines AI globaux

**Fichier :** `common/defines/zz_cp26_ai_defines.txt`

Les modifications sont globales (affectent TOUS les empires, pas que Ferengi) :
- `AI_FREE_JOBS_DISTRICT_BUILD_CAP = 0` (van: 1) — IA build plus agressivement
- `AI_DEFICIT_SCORE_MULT = 150` (van: 100) — IA réagit plus vite aux déficits
- `AI_MAX_DISTANCE = 230` (van: 200) — IA un peu plus agressive géographiquement
- `WAR_ATTACK_CLAIM_PRIO = 15.0` (van: 11.0) — IA plus agressive en guerre

Ces modifications sont **globales et raisonnables**. Elles rendent l'IA légèrement plus compétente sans la rendre brutale. Les commentaires explicatifs sont clairs (notamment la correction du crash). Équilibré.

---

## RÉSUMÉ EXÉCUTIF

| Catégorie                          | Verdict       | Sévérité    |
|------------------------------------|---------------|-------------|
| Research Exchange (bug ressource)  | ❌ Cassé       | Critique    |
| ROI des succursales                | 🟠 Trop faible | Majeur      |
| Security Contractors               | 🔴 Injouable   | Majeur      |
| Asymétrie joueur vs IA (trait)     | 🟡 Incohérent  | Mineur      |
| Absence de balance de départ       | 🟡 Pénalisant  | Mineur      |
| civic_rules_of_acquisition +30%    | 🟠 Surpuissant | Modéré      |
| Production de base country_type    | 🟡 Gonflée     | Informatif  |
| Système de départ / flotte         | ✅ Correct     | —           |
| AI configuration                   | ✅ Correct     | —           |
| Defines AI globaux                 | ✅ Raisonnable | —           |
| Nettoyage auto des branches        | ✅ Propre      | —           |

### Ce qui doit être corrigé avant release jouable :

1. **CRITIQUE** — Corriger `add_resource = { research = 10 }` → `physics_research = 4 + society_research = 3 + engineering_research = 3`
2. **MAJEUR** — Augmenter les revenus mensuels des succursales (x2 minimum) OU réduire les coûts
3. **MAJEUR** — Security Contractors : 4 alloys/mois pour 250 alloys de coût est injouable. Passer à 10-12/mois ou réduire le coût à 100-150 alloys
4. **MINEUR** — Ajouter `trait_thrifty` au joueur Ferengi (`_04_ferengi.txt`) pour égaliser joueur/IA
5. **MINEUR** — Ajouter un stockpile de départ dans `zz_cp26_major_start_balance.txt`

### Ce qui peut attendre :

6. Réduire `trade_value_mult` de 0.3 à 0.2 (à tester en jeu d'abord)
7. Ajouter une mécanique de fermeture volontaire de branche
8. Ajouter un impact diplomatique lors de l'établissement d'une branche (opinion +5 chez l'hôte ?)
9. Vérifier si `research = 20` dans le country_type est valide dans ce moteur STI

---

## Notes sur le reste du mod (hors Ferengi)

Seul le contenu Ferengi a été audité en profondeur. Le reste du mod (UFP, Klingon, Romulan, Cardassian, empires mineurs) n'est pas couvert par ce document. Les defines AI globaux ont été inclus car ils affectent toutes les parties.
