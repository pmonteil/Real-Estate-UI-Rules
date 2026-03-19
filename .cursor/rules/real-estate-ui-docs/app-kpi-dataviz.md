# KPI, Chiffres Clés & Data Visualization

> Conventions pour la génération de cartes KPI, chiffres clés, graphiques et dashboards. L'objectif est un rendu **moderne, épuré et identitaire Septeo** — pas de génération générique multicolore.

---

## 1. Philosophie visuelle

### Principes fondamentaux

- **Sobriété chromatique** : les chiffres et labels sont en couleur **neutre** (`--text-headings`, `--text-body`). Jamais de chiffre en bleu, rouge, violet ou autre couleur vive.
- **Accent orange Septeo** : la seule couleur vive autorisée sur les KPI est l'**orange Septeo** (`--brand-orange-400` / `--alias-accent-default`), utilisée exclusivement pour les sparklines, graphiques de fond, et éléments décoratifs visuels.
- **Pas de multicolore** : une carte KPI = une seule teinte décorative (orange). Pas de bleu + vert + violet sur une même rangée de KPI.
- **Pas de shadow** : délimiter les cartes KPI avec `border: 1px solid var(--border-default)` et `border-radius: 12px`. Jamais de `box-shadow`.
- **Modernité** : les KPI doivent donner une impression de tableau de bord premium, pas de widget générique. Sparklines, micro-graphiques, et indicateurs de tendance sont encouragés.

### Ce qu'on veut vs ce qu'on ne veut pas

| Bien | Mal |
|------|-----|
| Chiffre en `--text-headings` (noir/gris foncé) | Chiffre en bleu, violet ou multicolore |
| Sparkline en orange fondu derrière le chiffre | Pas de graphique, juste un chiffre seul |
| Bordure fine `--border-default` | `box-shadow` sur les cartes KPI |
| Indicateur de tendance discret (↑ +5%) | Gros badge coloré flashy |
| Palette monochrome orange/neutre | Chaque KPI d'une couleur différente |

---

## 2. Carte KPI — Pattern standard

### Structure

```
┌─ KPI Card (border: 1px solid --border-default, radius: 12px) ─────┐
│                                                                     │
│  📊 Label du KPI (body-small, --text-body-secondary)               │
│                                                                     │
│  1 247          ░░▒▓█▓▒░░  ← sparkline orange en fond/à droite    │
│  (H2, --text-headings)                                              │
│                                                                     │
│  ↑ 12.5% vs mois dernier  ← indicateur de tendance                │
│  (caption, couleur contextuelle)                                    │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Anatomie d'une carte KPI

| Élément | Style | Détails |
|---------|-------|---------|
| **Container** | `border: 1px solid var(--border-default); border-radius: 12px; padding: 1.25rem;` | Fond blanc `var(--surface-default)`. Pas de shadow. |
| **Label** | `font-size: 12px; color: var(--text-body-secondary); font-weight: 600;` | Texte court : "Biens actifs", "Visites ce mois", "CA mensuel" |
| **Valeur principale** | `font-size: 28–32px; font-weight: 700; color: var(--text-headings);` | Toujours neutre/noir. Jamais coloré. |
| **Sparkline** | SVG ou canvas, couleur `var(--brand-orange-400)` avec opacité 0.15–0.25 pour le fill | Positionnée à droite du chiffre ou en fond de carte. Area chart avec dégradé vertical (orange → transparent). |
| **Indicateur de tendance** | `font-size: 12px;` | Flèche ↑/↓ + pourcentage. Vert si positif, rouge si négatif, gris si neutre. |
| **Sous-label** | `font-size: 12px; color: var(--text-body-secondary);` | Contexte : "vs mois dernier", "ce trimestre", etc. |

### Icône optionnelle dans le label

Une icône Tabler peut accompagner le label pour identifier visuellement le KPI. L'icône utilise un **fond arrondi** en orange très léger.

```scss
.kpi-card__icon {
  width: 36px;
  height: 36px;
  border-radius: 10px;
  background: var(--surface-light-accent); // Orange très léger
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--alias-accent-default); // Orange Septeo
}
```

### Code de référence

```vue
<div class="kpi-card">
  <div class="kpi-card__header">
    <div class="kpi-card__icon">
      <IconHome :size="18" :stroke-width="1.5" />
    </div>
    <span class="kpi-card__label">Biens actifs</span>
  </div>
  <div class="kpi-card__body">
    <span class="kpi-card__value">1 247</span>
    <div class="kpi-card__sparkline">
      <!-- SVG sparkline area chart en orange -->
    </div>
  </div>
  <div class="kpi-card__trend kpi-card__trend--up">
    <IconTrendingUp :size="14" />
    <span>+12.5%</span>
    <span class="kpi-card__trend-context">vs mois dernier</span>
  </div>
</div>
```

```scss
.kpi-card {
  background: var(--surface-default);
  border: 1px solid var(--border-default);
  border-radius: 12px;
  padding: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.kpi-card__label {
  font-size: 12px;
  font-weight: 600;
  color: var(--text-body-secondary);
  text-transform: uppercase;
  letter-spacing: 0.02em;
}

.kpi-card__body {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 1rem;
}

.kpi-card__value {
  font-size: 28px;
  font-weight: 700;
  color: var(--text-headings);
  line-height: 1;
}

.kpi-card__sparkline {
  width: 80px;
  height: 32px;
  flex-shrink: 0;
}

.kpi-card__sparkline svg {
  width: 100%;
  height: 100%;
}

.kpi-card__sparkline .sparkline-fill {
  fill: var(--brand-orange-400);
  opacity: 0.15;
}

.kpi-card__sparkline .sparkline-stroke {
  stroke: var(--brand-orange-400);
  stroke-width: 1.5;
  fill: none;
  opacity: 0.6;
}

.kpi-card__trend {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 12px;
  font-weight: 500;
}

.kpi-card__trend--up {
  color: var(--alias-success-default);
}

.kpi-card__trend--down {
  color: var(--alias-error-default);
}

.kpi-card__trend--neutral {
  color: var(--text-body-secondary);
}

.kpi-card__trend-context {
  color: var(--text-body-secondary);
  font-weight: 400;
}
```

---

## 3. Rangée de KPI — Layout

Les cartes KPI se placent en **grille horizontale** en haut de page, entre le header et le contenu principal.

```scss
.kpi-row {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1rem;
  margin-bottom: 1.5rem;
}
```

### Variantes de disposition

| Nombre de KPIs | Layout recommandé |
|----------------|-------------------|
| 2–3 | `repeat(auto-fit, minmax(280px, 1fr))` |
| 4 | `repeat(4, 1fr)` |
| 5+ | `repeat(auto-fit, minmax(220px, 1fr))` avec wrap |

### Intégration dans la page

```
┌─ UiContainersHeader ─────────────────────────────┐
│  ← Tableau de bord         [Tabs]     [Actions]   │
└──────────────────────────────────────────────────┘

┌─ KPI Row (grille, max-width: 1400px) ────────────┐
│ ┌─ KPI ──┐ ┌─ KPI ──┐ ┌─ KPI ──┐ ┌─ KPI ──┐    │
│ │ Biens  │ │Visites │ │ CA     │ │ Leads  │    │
│ │ 1 247  │ │ 89     │ │ 42.5k€ │ │ 34     │    │
│ │ ↑12.5% │ │ ↓3.2%  │ │ ↑8.1%  │ │ ↑22%   │    │
│ └────────┘ └────────┘ └────────┘ └────────┘    │
└──────────────────────────────────────────────────┘

┌─ Contenu (graphiques, tableaux...) ──────────────┐
│  ...                                              │
└──────────────────────────────────────────────────┘
```

---

## 4. Sparklines & Micro-graphiques

### Sparkline area (derrière le chiffre)

Le pattern principal : un petit area chart SVG en orange fondu, positionné à droite du chiffre ou en arrière-plan de la carte.

```
Trait :  stroke: var(--brand-orange-400), opacity: 0.6, stroke-width: 1.5
Remplissage :  fill: dégradé vertical de var(--brand-orange-400) opacity 0.2 → transparent
```

### Sparkline comme fond de carte (variante immersive)

Pour un effet plus premium, la sparkline peut occuper toute la largeur basse de la carte en fond, avec une opacité très faible.

```scss
.kpi-card--immersive {
  position: relative;
  overflow: hidden;
}

.kpi-card--immersive .sparkline-bg {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 60%;
  opacity: 0.08;
}
```

### Couleurs des sparklines

| Contexte | Couleur du trait | Couleur du fill |
|----------|-----------------|-----------------|
| **Standard (défaut)** | `var(--brand-orange-400)` opacity 0.6 | `var(--brand-orange-400)` opacity 0.15 |
| **Tendance positive** | `var(--brand-orange-400)` opacity 0.6 | `var(--brand-orange-400)` opacity 0.15 |
| **Tendance négative** | `var(--brand-orange-400)` opacity 0.4 | `var(--brand-orange-400)` opacity 0.08 |

La sparkline reste **toujours en orange**, quelle que soit la tendance. C'est l'indicateur textuel (↑ vert / ↓ rouge) qui communique la direction, pas la couleur du graphique.

---

## 5. Graphiques (Charts)

### Règles générales

- **Palette monochrome** : utiliser des dégradés d'une seule teinte (orange Septeo) avec des variations d'opacité plutôt que plusieurs couleurs distinctes.
- **Pas de multicolore par défaut** : si un graphique a plusieurs séries, utiliser des nuances d'orange (du `--brand-orange-100` au `--brand-orange-600`) ou un duo orange + gris neutre.
- **Bordures, pas de shadows** : les conteneurs de graphique utilisent `border: 1px solid var(--border-default)` et `border-radius: 12px`.
- **Labels discrets** : axes en `--text-body-secondary`, grille en `--border-default-light`.

### Palette de données pour graphiques

Quand un graphique a plusieurs séries, utiliser dans cet ordre :

```scss
// Série principale
$chart-series-1: var(--brand-orange-400);  // Orange Septeo (plein)
// Série secondaire
$chart-series-2: var(--brand-orange-200);  // Orange clair
// Série tertiaire (si besoin)
$chart-series-3: var(--brand-grey-300);    // Gris neutre
// Série quaternaire (rare)
$chart-series-4: var(--brand-grey-200);    // Gris très clair
```

### Area Chart / Stream Chart

Inspiré du pattern "Sales Report" : des couches empilées avec des dégradés monochromes.

```scss
.chart-area {
  // Fond du graphique
  background: var(--surface-default);
  border: 1px solid var(--border-default);
  border-radius: 12px;
  padding: 1.5rem;
}

// Dégradé de fill pour area chart
.chart-area-fill {
  fill: url(#orangeGradient);
}

// SVG gradient
// <linearGradient id="orangeGradient" x1="0" y1="0" x2="0" y2="1">
//   <stop offset="0%" stop-color="var(--brand-orange-400)" stop-opacity="0.3" />
//   <stop offset="100%" stop-color="var(--brand-orange-400)" stop-opacity="0.02" />
// </linearGradient>
```

### Bar Chart

```scss
.bar-default {
  fill: var(--brand-orange-400);
  rx: 4px; // Bords arrondis en haut
}

.bar-secondary {
  fill: var(--brand-orange-200);
}

.bar-hover {
  fill: var(--brand-orange-500);
}
```

### Donut / Ring Chart

Pour les jauges de pourcentage (taux de conversion, objectifs atteints…) :

```scss
.donut-track {
  stroke: var(--border-default);
  stroke-width: 8;
  fill: none;
}

.donut-value {
  stroke: var(--brand-orange-400);
  stroke-width: 8;
  fill: none;
  stroke-linecap: round;
}

.donut-label {
  font-size: 28px;
  font-weight: 700;
  fill: var(--text-headings); // Neutre, jamais coloré
}
```

### Axes et grille

```scss
.chart-axis-label {
  font-size: 11px;
  fill: var(--text-body-secondary);
  font-weight: 400;
}

.chart-grid-line {
  stroke: var(--border-default-light);
  stroke-dasharray: 4 4; // Pointillés discrets
}

.chart-axis-line {
  stroke: var(--border-default);
}
```

---

## 6. Indicateurs de tendance

### Format

```
↑ +12.5% vs mois dernier
│   │        │
│   │        └─ Contexte (gris, --text-body-secondary)
│   └─ Valeur (colorée selon la direction)
└─ Icône directionnelle
```

### Couleurs de tendance

| Direction | Couleur | Icône Tabler |
|-----------|---------|-------------|
| Hausse (positif) | `var(--alias-success-default)` (vert) | `trending-up` |
| Baisse (négatif) | `var(--alias-error-default)` (rouge) | `trending-down` |
| Stable | `var(--text-body-secondary)` (gris) | `minus` |

La couleur de tendance est la **seule** couleur non-orange autorisée dans un KPI. Elle s'applique uniquement à la petite ligne d'indicateur, jamais au chiffre principal ni au graphique.

---

## 7. Dashboard — Layout complet

### Structure type

```
┌─ KPI Row ───────────────────────────────────────────┐
│  [KPI 1]  [KPI 2]  [KPI 3]  [KPI 4]               │
└─────────────────────────────────────────────────────┘
  gap: 1.5rem
┌─ Charts Row (grid 2 colonnes) ──────────────────────┐
│  ┌─ Chart principal ──────┐  ┌─ Chart secondaire ─┐ │
│  │  Area chart / Bar chart │  │  Donut / Répartition│ │
│  │  (2/3 de la largeur)   │  │  (1/3)             │ │
│  └────────────────────────┘  └─────────────────────┘ │
└─────────────────────────────────────────────────────┘
  gap: 1.5rem
┌─ Detail Row (grid 2 colonnes) ──────────────────────┐
│  ┌─ Tableau récent ───────┐  ┌─ Line chart ────────┐ │
│  │  UiTable (derniers     │  │  Évolution temporelle│ │
│  │  éléments)             │  │                      │ │
│  └────────────────────────┘  └──────────────────────┘ │
└─────────────────────────────────────────────────────┘
```

```scss
.dashboard-charts {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 1.5rem;
}

.dashboard-detail {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
}

.chart-card {
  background: var(--surface-default);
  border: 1px solid var(--border-default);
  border-radius: 12px;
  padding: 1.5rem;
}

.chart-card__title {
  font-size: 16px;
  font-weight: 600;
  color: var(--text-headings);
  margin-bottom: 1rem;
}
```

---

## 8. Interdictions

| Interdit | Faire à la place |
|----------|-----------------|
| Chiffre KPI coloré (bleu, vert, violet…) | Toujours `var(--text-headings)` (neutre) |
| Chaque KPI d'une couleur différente | Toutes les sparklines en orange Septeo |
| `box-shadow` sur les cartes KPI | `border: 1px solid var(--border-default)` |
| Graphique multicolore (bleu + rouge + vert) | Nuances monochromes d'orange + gris neutre |
| KPI sans sparkline ni élément visuel | Toujours ajouter un micro-graphique ou icône |
| Placeholder générique "Chart" | Toujours générer un vrai SVG sparkline avec des données |
| Fond gris sur les cartes KPI | `var(--surface-default)` (blanc) |
| Gros badge coloré flashy pour la tendance | Petit indicateur discret (12px) avec flèche |

---

## 9. Bibliothèques recommandées

Pour les graphiques, privilégier dans cet ordre :

1. **SVG inline** pour les sparklines simples (quelques points, area fill)
2. **Chart.js** ou **ApexCharts** pour les graphiques interactifs (bar, line, area, donut)
3. **D3.js** uniquement si besoin de visualisations très custom

Configurer le thème du chart avec la palette orange Septeo :

```ts
const chartTheme = {
  colors: ['#ff6136', '#ffb199', '#e0e0e0', '#c0c0c0'],
  grid: { borderColor: 'var(--border-default-light)' },
  labels: { style: { colors: 'var(--text-body-secondary)', fontSize: '11px' } },
};
```
