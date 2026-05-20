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

### Layout en zones séparées (design actuel — v1.5.x)

La carte est divisée en **4 zones verticales non superposées**. Le logo n'empiète plus sur le portrait.

```
CARTE 302×440 (prescripted_empire_design_entry, clipping=yes)
│
│  ┌─────────────────────────────┐  card y=0
│  │   ZONE LOGO                 │
│  │   empire_flag               │  scale=0.38 → ~82px
│  │   x=110, y=0                │
│  └─────────────────────────────┘  card y=82
│
├── entry_container (262×420, x=20 y=10, clipping=yes)
│   │
│   │  ┌──────────────────────┐  entry y=0  / card y=10
│   │  │  background          │  GFX_selectionscreen
│   │  │  (fond de carte)     │
│   │  │                      │
│   │  │  ZONE PORTRAIT       │  entry y=70 / card y=80
│   │  │  portrait_window     │  262×148, scale=0.52
│   │  │  (tête + épaules)    │
│   │  │                      │  entry y=218 / card y=228
│   │  ├──────────────────────┤
│   │  │  ZONE NOM            │  entry y=220 / card y=230
│   │  │  empire_name 262×45  │  texte + ligne dorée
│   │  ├──────────────────────┤  entry y=265 / card y=275
│   │  │  ZONE GOV/CIVICS     │  entry y=267 / card y=277
│   │  │  government_and_     │  262×123
│   │  │  ethics              │  traits y=26, civics y=60
│   │  │                      │  civics end : entry y=419 ✓
│   │  └──────────────────────┘  entry y=420 / card y=430
│
├── selected_overlay (284×408, x=8 y=25) — cadre de sélection
│
└── empire_flag (voir zone logo ci-dessus)
```

### Règle critique : les civics doivent rentrer dans entry_container

La `smoothListboxType` `civics` est à `y=60` dans `government_and_ethics`, hauteur `92px`.
Formule : `gov_y + 60 + 92 ≤ entry_container_height`

Avec les valeurs actuelles : `267 + 60 + 92 = 419 ≤ 420` ✓

**PIÈGE :** Si on descend `government_and_ethics` sans agrandir `entry_container`,
les civics sont coupés par le `clipping=yes` de `entry_container` → icônes manquantes.

### Ordre de rendu (z-order)

Les éléments sont rendus dans l'ordre de déclaration dans le fichier.
`empire_flag` est déclaré **après** `entry_container` → il s'affiche **par-dessus** le portrait.
Si on veut que le portrait passe devant le logo, inverser l'ordre de déclaration.

**Important :** `clipping=yes` sur les deux niveaux (`prescripted_empire_design_entry` ET
`entry_container`) est obligatoire. Sans ça, les sprites larges (`GFX_label_gradiant`,
`GFX_government_authority_background`) débordent et le moteur calcule les slots sur les
dimensions rendues, pas les dimensions déclarées.

### Pourquoi on ne peut pas supprimer le fond `GFX_selectionscreen`

Sans ce sprite, le moteur STI affiche les éléments 3D (vaisseaux, stations) directement
derrière chaque slot de carte. La carte perd son identité visuelle et les éléments 3D
du jeu passent à travers. Le fond est obligatoire.

### Formules de mise à l'échelle (si on change la largeur W)

```
prescripted_empire_design_entry width = W
empire_list_width_min_max y           = W

entry_container width    = W - 40          (marge 20px chaque côté)
portrait_window width    = W - 40
empire_name width        = W - 40,  maxWidth = W - 40
government_and_ethics    = W - 40
traits size.x            = W - 40
civics size.x            = W - 56          (8px gauche + 8px scrollbar)
unique_mechanic maxWidth  = W - 40
origin_window width      = W - 60,  maxWidth = W - 64
authority name maxWidth  = W - 52
selected_overlay width   = W - 18          (8px gauche + 10px droite)
empire_flag x            = (W - 216×scale) / 2   (centrage dynamique)
species_preview_civic    = W - 56
civic text maxWidth      = W - 116         (50px icône + marges)
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

## Valeurs actuelles (2026-05-20)

```
Largeur carte              : 302px
Hauteur carte              : 490px
empire_list_width_min_max  : { x=440  y=302 }
spacing liste              : 0px
Cards visibles             : 5 (toutes visibles simultanément)
Résolution testée          : 1512×982 (MacBook Pro 14" Retina 2×)
Contrainte écran           : 5 × y ≤ 1512px → y_max = 302px

--- Layout interne (zones séparées) ---
entry_container        : x=20  y=10   width=262  height=470
empire_flag            : x=110 y=0    scale=0.38  (~82×82px)
portrait_window        : y=65  (entry) height=148  scale=0.52  end=213
portrait_name_divider  : y=214 (entry) ligne or GFX_ST_Line_Gold_Empire
empire_name            : y=216 (entry) height=46              end=262
government_and_ethics  : y=264 (entry) height=178             end=442
  ├─ authority row     : y=2   scale=1.0 (icônes plein format)
  ├─ ethics (traits)   : y=34  height=40  spacing=6
  ├─ ethics_civics_div : y=76  ligne or GFX_ST_Line_Gold_Empire
  └─ civics            : x=56  y=80  size=150×95  end=gov y=175 ≤ 178 ✓
origin_window          : y=442 (entry) height=28               end=470 ✓
selected_overlay       : x=8   y=25   width=284  height=458

--- Fond de carte ---
GFX_cp26_card_bg (défini dans CP26/interface/cp26_empire_selection.gfx)
  → texturefile = gfx/interface/frontend/pre_scripted.dds
  → corneredTileSpriteType, borderSize={8,8}
  Remplace l'usage de GFX_selectionscreen (bake dans le binaire STI, non accessible)
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
