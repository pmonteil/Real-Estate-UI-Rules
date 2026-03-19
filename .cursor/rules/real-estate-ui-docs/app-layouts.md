# Layouts et conventions d'application

> Conventions de construction des pages de l'application.

---

## 0. Éléments fixes (ne scrollent pas)

Les éléments suivants sont **fixes** et ne bougent jamais lors du scroll de la page :

| Élément | Position | Détails |
|---------|----------|---------|
| **Topbar** (`UiTopbarOffice`) | `position: fixed; top: 0; left: 0; right: 0;` | `z-index: 1200`, hauteur 44px. Toujours visible en haut. |
| **Menu latéral** (`UiMenuOffice`) | `position: fixed; top: 44px; left: 0; bottom: 0;` | `z-index: 1100`, largeur 206px (60px plié). Ne scroll jamais avec la page. |
| **Header de page** (`UiContainersHeader`) | `position: sticky; top: 44px;` | `z-index: 50`. Reste collé sous la topbar au scroll. Son contenu interne respecte le `max-width: 1400px`. |

La zone de contenu scrollable est celle à droite du menu et sous le header.

---

## 1. Container de page

### Max-width

Toute page doit avoir un conteneur principal limité à **`max-width: 1400px`** centré horizontalement, sauf exception justifiée (builder plein écran, page avec sidebar de configuration). Cette contrainte s'applique aussi au **contenu interne du `UiContainersHeader`** (voir sa doc).

```scss
.page-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 2rem;
  width: 100%;
}
```

### Exceptions de max-width autorisées

| Contexte | Max-width | Justification |
|----------|-----------|---------------|
| Page standard (liste, dashboard, catalogue) | `1400px` | Défaut |
| Formulaire centré (config, wizard step) | `700–900px` | Confort de lecture |
| Builder / éditeur (email, widget) | `none` | Besoin de tout l'espace |

### Padding du conteneur

- **Horizontal** : `2rem` (32px) — standard. `1rem` en mobile (< 768px).
- **Vertical** : `1.5rem` en haut, `4rem` en bas (marge de respiration pour barres de sauvegarde flottantes).

```scss
.page-container {
  padding: 1.5rem 2rem 4rem;

  @media (max-width: 768px) {
    padding: 1rem 1rem 4rem;
  }
}
```

---

## 2. Alignement vertical

### Principe fondamental

Tous les éléments de la page doivent être **alignés verticalement sur le même axe gauche**. Le bouton retour du `UiContainersHeader`, les titres de section, les filtres, les premières colonnes de tableau — tout doit être aligné sur la même marge gauche.

```
┌──────────────────────────────────────────────────┐
│ ← Retour   Titre page       [Tabs]    [Actions] │  ← Header
├──────────────────────────────────────────────────┤
│ 🔍 Search  [Filtre 1] [Filtre 2]       [Action] │  ← Toolbar
├──────────────────────────────────────────────────┤
│ ▮ Colonne 1   │ Colonne 2  │ Colonne 3          │  ← Table
│ Donnée 1      │ Donnée 2   │ Donnée 3           │
└──────────────────────────────────────────────────┘
▲
└── Même axe gauche pour tout
```

Pour obtenir cet alignement, le `padding-left` du conteneur de page doit correspondre au `padding-left` du header. Le header (`UiContainersHeader`) et le contenu partagent la même grille de padding horizontal.

---

## 3. Séparation des sections

### Règle : bordures, jamais de shadow

Les sections de page sont séparées par des **bordures**, pas par des ombres. Les ombres sont réservées aux éléments flottants (drawers, modales, dropdowns, barres de sauvegarde).

```scss
// Séparation entre sections
border-bottom: 1px solid var(--border-default);

// Séparation légère (sous-sections)
border-bottom: 1px solid var(--border-default-light);
```

### Quand utiliser quoi

| Élément | Séparation | Variable |
|---------|-----------|----------|
| Header ↔ Contenu | `border-bottom` | `--border-default-light` |
| Toolbar ↔ Contenu | `border-bottom` | `--border-default-light` |
| Section ↔ Section | `border-bottom` ou `border-top` | `--border-default` |
| Sous-section ↔ Sous-section | `border-top` | `--border-default-light` |
| Carte (dans un grid) | `border` complet | `--border-default-light` |
| Drawer / Modale | `box-shadow` | Voir guide principal |

### Fond des sections

- **Page** : `var(--alias-neutral-white)` — fond blanc.
- **Titres de section** : `var(--surface-default)` — toujours fond blanc. Ne jamais utiliser de fond gris sur les titres de section.
- **Champs de formulaire** : `var(--surface-field)`.
- Ne pas utiliser de gris franc comme fond de page.

---

## 4. Hiérarchie typographique

Respecter strictement la hiérarchie du Design System (valeurs Figma).

### Échelle des titres

| Niveau | Taille | Graisse | Interligne | Usage |
|--------|--------|---------|------------|-------|
| **H1 (Display)** | `40px` | Bold (700) | `50px` | Titres "hero" / displays. Très rare. Ex: "Modelo InTouch" sur la vue d'ensemble. |
| **H2** | `20px` | Bold (700) | `22px` | **Titre de page** dans le header. Ex: "Estimation", "Biens immobiliers". C'est le titre dans `UiContainersHeader`. |
| **H3** | `16px` | Semi Bold (600) | `20px` | **Titre de section**. Toujours accompagné d'une icône Tabler à gauche. Ex: titre d'accordéon, titre de bloc de configuration. |
| **H4** | `14px` | Semi Bold (600) | `18px` | **Sous-titre de section** ou label de groupe de champs. |

### Échelle du texte courant

| Style | Taille | Graisse | Interligne | Usage |
|-------|--------|---------|------------|-------|
| **Body** | `14px` | Regular (400) | `14px` | Texte courant, descriptions, contenu de cellules de tableau. |
| **Link** | `14px` | Regular (400) | `14px` | Liens texte. Décoration : `underline`. |
| **Body small** | `12px` | Regular (400) | `12px` | Texte secondaire, légendes, méta-données, compteurs. |
| **Caption** | `10px` | Regular (400) | `10px` | Petits labels, badges, indicateurs discrets. |
| **Mentions** | `10px` | Regular (400) | `10px` | Mentions, annotations. |

### Couleurs de texte

| Usage | Variable |
|-------|----------|
| Titres (H1–H4) | `var(--text-headings)` |
| Corps de texte | `var(--text-body)` |
| Texte secondaire / descriptions | `var(--text-body-secondary)` |
| Placeholder | `var(--text-placeholder)` |
| Liens / actions | `var(--text-action)` |
| Erreur | `var(--text-error)` |

### Interdictions typographiques

- **Jamais** de tailles en dur (`18px`, `1.125rem`, `0.8125rem`) — toujours utiliser les variables ou les tailles du tableau ci-dessus.
- **Jamais** de couleurs en dur pour le texte (`#6b7280`, `#2e3862`) — toujours `var(--text-*)`.
- **Jamais** de `font-weight: 500` isolé — utiliser `400` (Regular), `600` (Semi Bold) ou `700` (Bold) uniquement.

---

## 5. Structure type d'une page

### 5.1 Page de liste (catalogue, tableau)

```
┌─ UiContainersHeader ─────────────────────────────┐
│  ← Titre page (H2)         [Tabs]     [Actions]  │
└──────────────────────────────────────────────────┘
  border-bottom: 1px solid var(--border-default-light)

┌─ Toolbar ────────────────────────────────────────┐
│  [UiFilter] [UiFilter] [UiFilter]   🔍 Recherche │
└──────────────────────────────────────────────────┘
  margin-bottom: 1.5rem

┌─ Contenu (max-width: 1400px, margin: 0 auto) ───┐
│  ┌─ Section ───────────────────────────────────┐  │
│  │  📦 Titre section (H3)          [Compteur]  │  │
│  │  ──────────────────────────────────────────  │  │
│  │  Grille de cartes / Tableau UiTable         │  │
│  └─────────────────────────────────────────────┘  │
│                                                   │
│  ┌─ Section ───────────────────────────────────┐  │
│  │  📧 Titre section (H3)          [Compteur]  │  │
│  │  ──────────────────────────────────────────  │  │
│  │  Grille de cartes / Tableau UiTable         │  │
│  └─────────────────────────────────────────────┘  │
└───────────────────────────────────────────────────┘
```

### 5.2 Page de configuration / formulaire

```
┌─ UiContainersHeader ─────────────────────────────┐
│  ← Titre (H2)  [Pill statut]      [Enregistrer]  │
└──────────────────────────────────────────────────┘

┌─ Contenu centré (max-width: 700–900px) ──────────┐
│                                                   │
│  ┌─ Bloc de config ────────────────────────────┐  │
│  │  ⚙ Titre section (H3)                       │  │
│  │  border-bottom: --border-default-light       │  │
│  │  ┌─ body ─────────────────────────────────┐  │  │
│  │  │  UiLabel + UiInput                     │  │  │
│  │  │  UiLabel + UiSelect                    │  │  │
│  │  └────────────────────────────────────────┘  │  │
│  └─────────────────────────────────────────────┘  │
│                gap: 1.5rem                        │
│  ┌─ Bloc de config ────────────────────────────┐  │
│  │  🎨 Titre section (H3)                      │  │
│  │  ...                                        │  │
│  └─────────────────────────────────────────────┘  │
│                                                   │
└───────────────────────────────────────────────────┘

┌─ Barre de sauvegarde (fixed bottom, 100% du contenu) ──┐
│  "Modifications non enregistrées"    [Annuler] [Sauvegarder] │
└────────────────────────────────────────────────────────┘
⚠ La barre de sauvegarde s'étire sur 100% de la zone de contenu
  uniquement — elle ne doit PAS couvrir le menu latéral.
```

### 5.3 Page avec stepper (wizard)

```
┌─ UiContainersHeader ─────────────────────────────┐
│  ← Titre        [① Étape 1 ─── ② Étape 2]       │
└──────────────────────────────────────────────────┘

┌─ Contenu centré (max-width adapté à l'étape) ────┐
│  Contenu de l'étape courante                      │
└───────────────────────────────────────────────────┘

┌─ Footer d'actions (sticky ou en bas) ────────────┐
│  [← Retour]                  [Suivant →]          │
└──────────────────────────────────────────────────┘
```

---

## 6. Titres de section — Pattern standard

Chaque section à l'intérieur d'une page doit avoir un titre **H3** accompagné d'une **icône Tabler** qui représente la section. C'est le pattern "accordéon" utilisé dans la bibliothèque de modèles.

```vue
<div class="section-header">
  <IconPhoto :size="20" :stroke-width="2" class="section-header__icon" />
  <h3 class="section-header__title">Logo de l'agence</h3>
</div>
```

```scss
.section-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.5rem 1.25rem;
  background: var(--surface-default); // Toujours fond blanc, jamais de gris
  border-bottom: 1px solid var(--border-default-light);
}

.section-header__icon {
  color: var(--icon-secondary);
  flex-shrink: 0;
}

.section-header__title {
  margin: 0;
  font-size: 16px; /* H3 */
  font-weight: 600;
  color: var(--text-headings);
  line-height: 20px;
}
```

### Compteurs dans les titres de section

Si la section contient une collection (liste de cartes, catégorie), afficher un compteur discret à droite du titre.

```vue
<div class="section-header">
  <IconMail :size="20" :stroke-width="2" class="section-header__icon" />
  <h3 class="section-header__title">Modèles d'email</h3>
  <span class="section-header__count">{{ items.length }}</span>
</div>
```

```scss
.section-header__count {
  font-size: 12px;
  font-weight: 600;
  color: var(--text-body-secondary);
  background: var(--surface-default-bis);
  border: 1px solid var(--border-default);
  padding: 2px 8px;
  border-radius: 10px;
  line-height: 1;
}
```

---

## 7. Toolbar / Barre de filtres

La toolbar se place entre le header et le contenu. Elle contient les filtres à gauche et les actions à droite.

```scss
.page-toolbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  padding: 1rem 0;
  flex-wrap: wrap;
}

.page-toolbar__filters {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.page-toolbar__actions {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-left: auto;
}
```

---

## 8. Grilles de cartes

```scss
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1rem;
}
```

Les cartes utilisent `border: 1px solid var(--border-default-light)` et `border-radius: 12px`. Pas de shadow au repos. Shadow uniquement au hover si nécessaire : `0 4px 20px rgba(61, 100, 237, 0.08)`.

---

## 9. Espacement entre blocs

| Entre quoi | Espacement |
|-----------|-----------|
| Header ↔ Toolbar | `0` (collés, séparés par border) |
| Toolbar ↔ Contenu | `1.5rem` |
| Section ↔ Section | `2rem` |
| Sous-section ↔ Sous-section | `1rem` |
| Champs de formulaire | `1rem` (gap dans le body) |
| Label ↔ Champ | `0.5rem` |
| Titre de section ↔ Body de section | `0` (séparés par border) |

---

## 10. Responsive

### Breakpoints

| Breakpoint | Cible |
|-----------|-------|
| `max-width: 1200px` | Mode compact (menus, réduction de colonnes) |
| `max-width: 900px` | Tablette (1 colonne, toolbar empilée) |
| `max-width: 768px` | Mobile (padding réduit, menu collapsed) |

### Comportements responsive

- Le `max-width: 1400px` du conteneur reste. Le contenu se réduit naturellement.
- Les grilles passent de multi-colonnes à 1 colonne.
- La toolbar empile filtres et actions sur deux lignes (`flex-wrap: wrap`).
- Le padding horizontal passe de `2rem` à `1rem`.
- Les drawers passent en plein écran mobile.

---

## 11. Récapitulatif des interdictions

| Interdit | Faire à la place |
|----------|-----------------|
| `box-shadow` sur une section de page | `border` avec `--border-default` |
| Couleur de texte en dur (`#6b7280`) | `var(--text-body-secondary)` |
| Taille de police custom (`1.125rem`) | Utiliser l'échelle H2/H3/H4/Body/Small/Caption |
| `font-weight: 500` | `400` ou `600` uniquement |
| `max-width` > 1400px sur page standard | `1400px` max, sauf builder/éditeur |
| Titre de section sans icône | Toujours ajouter une icône Tabler |
| Sections désalignées du header | Même padding horizontal partout |
| Fond gris sur les titres de section | `var(--surface-default)` — toujours blanc |
| Fond gris foncé sur les pages | `var(--alias-neutral-white)` uniquement |
