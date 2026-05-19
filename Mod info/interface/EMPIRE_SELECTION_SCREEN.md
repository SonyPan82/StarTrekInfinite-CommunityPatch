# Empire Selection Screen — Guide d'implémentation CP26

## Contexte

Le fichier qui gère l'écran de sélection d'empire est :
`interface/select_empire_design.gui`

CP26 utilise une liste **horizontale** (scrollable gauche-droite) à la place de la liste verticale vanilla STI. Cette modification repose sur un paramètre STI-exclusif (`horizontal = 1` sur le `smoothListboxType`) et sur des `positionType` lus par le moteur C++.

---

## Découvertes clés sur le moteur C++ (STI)

### positionType lus par le C++ (noms hardcodés)

Seuls ces 4 noms sont reconnus par le moteur. Tout autre nom est ignoré :

| Nom | Rôle |
|-----|------|
| `empire_list_width_min_max` | Voir ci-dessous — critique |
| `back_button_offset_x_min_max` | Offset du bouton Back selon la résolution |
| `empire_list_margin_bottom` | Marge basse de la liste |
| `empire_list_fade_width` | Largeur des dégradés de fondu gauche/droite |

### empire_list_width_min_max — le plus important

```
positionType = {
    name = "empire_list_width_min_max"
    position = { x = 440  y = 440 }
}
```

**Pour une liste VERTICALE (vanilla STI) :**
- `x` = largeur du conteneur liste (= largeur des cartes)
- `y` = hauteur max par slot

**Pour une liste HORIZONTALE (`horizontal = 1`) :**
- Le moteur **inverse les axes**
- `x` = hauteur de la zone d'affichage de la liste
- `y` = **largeur de chaque slot** (utilisée pour centrer la liste)

> **PIÈGE :** Si `y` ne correspond pas à la largeur réelle des cartes, le moteur centre mal la liste.
> Exemple : y=440 avec cartes de 240px → slots de 440px → 5×440=2200px centrés dans 1512px → UFoP coupée à -344px.

**Règle :** `y` doit toujours être égal à la largeur déclarée de `prescripted_empire_design_entry`.

### Comportement selon la largeur totale

| Total (5 × slot_y) vs écran | Comportement |
|-----------------------------|--------------|
| Total ≤ largeur écran | C++ centre la liste, tout est visible sans scroll |
| Total > largeur écran | Scrollbar active MAIS portée limitée à ~`y` pixels |

> **PIÈGE SCROLL :** Quand total > écran, la scrollbar ne permet PAS de tout voir.
> La portée du scroll semble être limitée à `y` pixels (= largeur d'un slot).
> Exemple : y=440, total=5×440=2200px, scroll max=440px → Ferengi à 1760px inaccessible.
> **Conséquence :** Pour 5 empires sur 1512px, il faut impérativement `5 × y ≤ 1512`.
> Maximum avec spacing=0 : y=302px (5×302=1510px).

### size du smoothListboxType

```
size = { x = 100  y = 100 }
```
Le commentaire dans le fichier indique que `x` est remplacé par `Resolution.x` en runtime. Ne pas mettre de valeur fixe ici.

---

## Structure de la carte `prescripted_empire_design_entry`

```
prescripted_empire_design_entry (440×440, clipping=yes)
│
├── background (GFX_invisible — hitbox)
│
├── entry_container (400×390, position x=20 y=30, clipping=yes)
│   ├── background (GFX_selectionscreen — sprite de fond)
│   ├── portrait_window (400×185) — portrait du dirigeant
│   ├── empire_name (400×50, y=178) — nom de l'empire + ligne dorée
│   ├── government_and_ethics (400×155, y=232)
│   │   ├── unique_mechanic_icon + unique_mechanic_name
│   │   ├── traits (overlappingElementsBoxType)
│   │   ├── civics (smoothListboxType → species_preview_civic)
│   │   └── government_authority_icon + background + name
│   └── origin_window (380×36, y=348) — texte de lore/origine
│
├── selected_overlay (422×408, position x=8 y=25) — cadre de sélection
│
└── empire_flag (GFX_dummy_flag_216, x=139) — emblème flottant
```

**Important :** `clipping=yes` sur les deux niveaux (`prescripted_empire_design_entry` ET `entry_container`) est obligatoire. Sans ça, les sprites naturellement larges (`GFX_label_gradiant`, `GFX_government_authority_background`) débordent et le moteur calcule les slots sur les dimensions rendues, pas les dimensions déclarées.

### Formules de mise à l'échelle

Quand on change la largeur de carte (`W`), mettre à jour :

```
prescripted_empire_design_entry width = W
empire_list_width_min_max y = W

entry_container width    = W - 40          (marge 20px chaque côté)
portrait_window width    = W - 40
empire_name width        = W - 40,  maxWidth = W - 40
government_and_ethics    = W - 40
traits size.x            = W - 40
civics size.x            = W - 56          (8px gauche + 8px scrollbar)
unique_mechanic maxWidth  = W - 40
origin_window width      = W - 60,  maxWidth = W - 64
authority name maxWidth  = W - 52
selected_overlay width   = W - 18         (8px gauche + 10px droite)
empire_flag x            = (W - 162) / 2  (centrage du drapeau 162px)
species_preview_civic    = W - 56
civic text maxWidth      = W - 116        (50px icône + marges)
```

---

## Ajouter un nouvel empire prescrit (checklist)

### 1. Fichier prescripted country

Créer `common/prescripted_countries/_0X_nomempire.txt`

Le préfixe `_0X_` contrôle l'ordre de chargement (et donc l'ordre dans la liste de sélection).

```
nomempire = {
    country_id = "nomempire"
    short_name = "EMPIRE_SHORT_NAME_nomempire"
    adjective   = "PRESCRIPTED_adjective_nomempire"
    ...
    flags = { ... custom_start_screen nomempire }
    initializer = "nomempire_system_initializer"
    graphical_culture = "nomempire"
    ...
}
```

Le flag `custom_start_screen` est requis pour que la carte apparaisse dans l'écran de sélection CP26.

### 2. Clés de localisation obligatoires

Dans `localisation/french/` ET `localisation/english/` :

```yaml
EMPIRE_DESIGN_NOMEMPIRE:0 "Nom affiché"
EMPIRE_SHORT_NAME_nomempire:0 "NOM COURT"
START_SCREEN_NOMEMPIRE:0 "Description affichée sur la carte de sélection."
nomempire_unique_mechanic:0 "Nom du mécanisme unique"
nomempire_unique_mechanic_tooltip:0 "Description du mécanisme unique (tooltip)."
gov_X_nomempire:0 "Nom du gouvernement"
```

> `nomempire_unique_mechanic` doit correspondre exactement à `country_id + "_unique_mechanic"`.
> Si la clé est manquante, le texte brut de la clé s'affiche sur la carte.

### 3. Aucune modification du GUI requise

Le `smoothListboxType` nommé `list` est peuplé automatiquement par le C++ via les `prescripted_countries`. Tant que le flag `custom_start_screen` est présent et que `prescripted_empire_design_entry` est défini dans le GUI, la carte apparaît automatiquement.

---

## Valeurs actuelles (2026-05-19)

```
Largeur carte        : 302px
empire_list_width_min_max : { x=440  y=302 }
spacing liste        : 0px
Cards visibles       : 5 (toutes visibles simultanément)
Résolution testée    : 1512×982 (MacBook Pro 14" Retina 2×)
Contrainte écran     : 5 × y ≤ 1512px → y_max = 302px
```

---

## Empires présents (ordre d'affichage)

| Fichier | Empire | country_id |
|---------|--------|------------|
| `_00_ufop.txt` (ou équivalent) | United Federation of Planets | `ufop` |
| `_01_klingon.txt` | Klingon Empire | `klingon` |
| `_02_romulan.txt` | Romulan Star Empire | `romulan` |
| `_03_cardassian.txt` | Cardassian Union | `cardassian` |
| `_04_ferengi.txt` | Ferengi Alliance | `ferengi` |
