# KPI, Chiffres Clés & Data Visualization

> Conventions pour la génération de dashboards modernes, interactifs, visuels et premium, alignés avec le Design System **Real Estate UI (Septeo)**.

⚠️ IMPORTANT :
Toutes les couleurs doivent provenir des variables `mapped → Context`.
Aucune couleur hardcodée.

---

# 1. Philosophie globale

Créer des dashboards :

- modernes (SaaS / fintech / AI tools)
- très lisibles
- interactifs
- visuellement riches mais maîtrisés
- arrondis et fluides (soft UI)

---

## Principes fondamentaux

- **Lisibilité > décoratif**
- **Graphiques au service de la donnée**
- **Arrondi systématique**
- **Hiérarchie forte**
- **Densité maîtrisée**

---

# 2. Couleurs (STRICT)

## Source

👉 UNIQUEMENT variables DS (`mapped → Context`)

---

## Autorisé

- `--alias-accent-default` (orange Septeo)
- bleu Septeo (si défini dans DS)
- couleurs neutres (`--text-*`, `--border-*`, `--surface-*`)

---

## Interdit

❌ HEX
❌ couleurs arbitraires
❌ multicolore non maîtrisé

---

## Règles

- 1 couleur dominante par graphique
- variations via opacity / gradients uniquement

---

# 3. KPI ROW (CRITIQUE)

## Objectif

👉 En desktop : **tous les KPI sur une seule ligne (priorité)**
👉 Responsive : wrap autorisé si nécessaire

---

## Règles

1. Toujours essayer de tenir sur 1 ligne
2. Réduire largeur avant wrap
3. Wrap uniquement en fallback

---

## Implémentation

```scss
.kpi-row {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1rem;
}
```

---

## Desktop prioritaire

```scss
@media (min-width: 1200px) {
  .kpi-row {
    grid-template-columns: repeat(4, 1fr);
  }
}
```

---

## Ajustement

- min-width possible : 220 → 200 → 180px
- ensuite seulement wrap

---

## Interdictions

❌ wrap immédiat
❌ 2 KPI par ligne en desktop
❌ cartes trop larges

---

# 4. KPI CARD (CRITIQUE)

## Priorité

👉 Le chiffre est **intouchable visuellement**

- jamais caché
- jamais superposé
- toujours lisible

---

## Style

```scss
.kpi-card {
  background: var(--surface-default);
  border: 1px solid var(--border-default);
  border-radius: 16px;
  padding: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  min-width: 0;
  transition: border-color 0.2s ease;
}

.kpi-card:hover {
  border-color: var(--border-strong);
}
```

---

## Règles

- toujours une sparkline
- toujours un trend
- jamais de KPI vide

---

# 5. SPARKLINE (CRITIQUE)

## Interdictions

❌ derrière le texte
❌ sur le chiffre
❌ trop visible
❌ élément dominant

---

## Placement autorisé

### Option A (recommandé)

```
[ valeur ]     [ sparkline ]
```

---

### Option B (premium)

```
valeur
trend

sparkline en bas (zone dédiée)
```

---

## Style

```scss
.sparkline {
  opacity: 0.15;
}
```

---

## Règle absolue

👉 La sparkline ne doit jamais gêner la lecture

---

# 6. KPI STYLE

- border-radius : 12–16px
- pas de shadow
- hover léger
- icône possible (fond arrondi)

---

# 7. GRAPHIQUES (CHARTS)

## Librairie obligatoire

👉 ApexCharts uniquement

---

## Interdictions

❌ Chart.js
❌ graph statique
❌ config par défaut

---

## Obligatoire

- animation
- hover
- tooltip custom
- transitions fluides

---

# 8. INTERACTIVITÉ

Chaque graphique doit inclure :

- animation d’entrée
- hover dynamique
- feedback visuel

---

## Animations

- line → draw progressif
- bar → montée
- donut → remplissage

---

# 9. TYPES DE GRAPHIQUES

---

## Area Chart

- curve smooth
- gradient léger
- highlight dernier point

---

## Bar Chart

- border-radius 6px
- spacing large
- hover accentué

---

## Donut

- épais
- centré
- animé

---

## Graphiques avancés (encouragés)

- comparaison multi séries
- highlight peak
- overlays subtils
- mini charts intégrés

---

# 10. EFFETS VISUELS

Autorisés (subtils uniquement)

---

## Gradient

- basé sur variables DS uniquement

---

## Glow léger

```scss
filter: blur(8px);
opacity: 0.1;
```

---

## Overlay

- profondeur légère
- jamais dominant

---

# 11. DATA STORYTELLING

Un graphique doit :

- raconter une évolution
- montrer une tendance
- mettre en avant un point clé

---

## Obligatoire

- highlight (dernier point OU peak)

---

# 12. TOOLTIP

## Style obligatoire

- fond blanc
- border fine
- padding propre
- typo DS

---

## Interdiction

❌ tooltip par défaut ApexCharts

---

# 13. MICRO-INTERACTIONS

Obligatoire :

- hover KPI → border léger
- hover chart → point actif
- hover bar → intensité ↑

---

# 14. STRUCTURE CODE

Toujours séparer :

- data
- options chart
- styles

---

# 15. CHART CARD

```scss
.chart-card {
  background: var(--surface-default);
  border: 1px solid var(--border-default);
  border-radius: 16px;
  padding: 1.5rem;
}
```

---

# 16. AXES & GRILLE

- grille légère
- labels discrets
- jamais dominant

---

# 17. INTERDICTIONS GLOBALES

❌ KPI mal alignés
❌ KPI sur plusieurs lignes (desktop sans contrainte)
❌ sparkline intrusive
❌ graph sans animation
❌ tooltip par défaut
❌ couleurs hors DS
❌ rendu générique

---

# 18. OBJECTIF FINAL

Chaque dashboard doit ressembler à :

- un produit SaaS premium
- un outil AI analytics
- une interface moderne type Stripe / Linear

---

👉 Jamais un template admin générique

---
