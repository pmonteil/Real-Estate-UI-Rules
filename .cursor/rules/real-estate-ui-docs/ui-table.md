# UiTable

> Tableau de données avec header triable, toolbar d'actions, et transitions animées sur les lignes. Utilise `UiTableCell` en interne pour le rendu des cellules.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `title` | `string` | `""` | Titre H3 affiché au-dessus du tableau |
| `columns` | `TableColumn[]` | — | **Requis.** Définition des colonnes |
| `rows` | `TableRow[]` | — | **Requis.** Données des lignes (objets clé/valeur) |
| `showHeader` | `boolean` | `true` | Affiche/masque la ligne d'en-tête |
| `rowClickable` | `boolean` | `false` | Rend les lignes cliquables (curseur pointer, hover) |
| `toolbarActions` | `ToolbarAction[]` | *6 actions par défaut* | Boutons de la toolbar (affiché si pas de slot `toolbar`) |

## Types

### TableColumn

```typescript
interface TableColumn {
  key: string;           // Clé de la donnée dans la row
  label: string;         // Label affiché dans le header
  type?: 'text' | 'link' | 'action' | 'pill';  // Type de cellule (défaut: 'text')
  width?: string;        // Largeur fixe (ex: '120px', '20%')
  sortable?: boolean;    // Triable (défaut: true, sauf 'action')
  icon?: string;         // Icône dans le header (nom Tabler)
  iconLeft?: string;     // Icône à gauche de la cellule texte
  iconRight?: string;    // Icône à droite de la cellule texte
  // Pour type 'link'
  linkIconLeft?: string;
  linkIconRight?: string;
  href?: string;         // Clé de la row contenant l'URL
  to?: string;           // Clé de la row contenant la route
  // Pour type 'action'
  actions?: { name: string; icon: string; secondary?: boolean }[];
  // Pour type 'pill'
  pillColor?: string;    // Clé de la row contenant la couleur
  pillColorDefault?: 'blue' | 'red' | 'orange' | 'green' | 'purple' | 'grey';
  pillIcon?: string;     // Clé de la row contenant l'icône
  pillIconDefault?: string;
}
```

### ToolbarAction

```typescript
interface ToolbarAction {
  name: string;      // Identifiant de l'action
  icon: string;      // Icône Tabler
  primary?: boolean; // true = variant 'third', false = 'secondary'
}
```

## Slots

| Slot | Description |
|------|-------------|
| `toolbar` | Remplace les boutons de toolbar par défaut. Permet d'injecter des filtres, recherche, etc. |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `toolbar-action` | `string` (name) | Clic sur un bouton toolbar par défaut |
| `row-click` | `(row, index)` | Clic sur une ligne (si `rowClickable`) |
| `cell-action` | `{ action, row, rowIndex, column }` | Clic sur un bouton d'action dans une cellule |
| `sort` | `(key, direction)` | Changement de tri (key = colonne, direction = 'asc' \| 'desc') |

## Exemples

### Tableau basique

```vue
<UiTable
  :columns="[
    { key: 'nom', label: 'Nom' },
    { key: 'ville', label: 'Ville' },
    { key: 'prix', label: 'Prix', width: '120px' },
  ]"
  :rows="biens"
/>
```

### Avec colonnes typées

```vue
<UiTable
  title="Biens immobiliers"
  :columns="[
    { key: 'reference', label: 'Réf.', icon: 'hash', width: '100px' },
    { key: 'nom', label: 'Bien', type: 'link', to: 'detailRoute' },
    { key: 'statut', label: 'Statut', type: 'pill', pillColor: 'statutColor', width: '140px' },
    { key: 'actions', label: '', type: 'action', width: '80px', sortable: false,
      actions: [
        { name: 'edit', icon: 'pencil' },
        { name: 'delete', icon: 'trash', secondary: true },
      ]
    },
  ]"
  :rows="biens"
  row-clickable
  @row-click="openDetail"
  @cell-action="handleAction"
/>
```

### Avec toolbar custom

```vue
<UiTable :columns="columns" :rows="rows">
  <template #toolbar>
    <UiInput v-model="search" placeholder="Rechercher..." />
    <UiButton icon="download" variant="secondary" size="sm" />
  </template>
</UiTable>
```

## Bonnes pratiques

- Toujours utiliser `UiTable` au lieu de `<table>` natif pour les données tabulaires.
- Définir un `key` unique dans les données (`id` ou `reference`) pour les animations de transition.
- Utiliser `width` sur les colonnes de taille fixe (actions, statuts, dates) pour un rendu stable.
- Le type `pill` est idéal pour les colonnes de statut — mapper la couleur via `pillColor`.
- Mettre `sortable: false` sur les colonnes d'action et les colonnes non-triables.
- Préférer le slot `toolbar` pour des toolbars custom (filtres, recherche).

## À ne pas faire

- Ne pas construire un tableau avec `<table>` + CSS custom.
- Ne pas créer des cellules custom sans passer par le système `type` + `UiTableCell`.
- Ne pas oublier `row-clickable` si les lignes doivent être cliquables.
- Ne pas mettre un `title` ET un slot `toolbar` vide — le header ne s'affiche que si l'un des deux est défini.
- Ne pas utiliser des couleurs en dur pour les pills — toujours utiliser les noms de couleur du DS (`blue`, `green`, `red`, etc.).

## Notes d'intégration

- Le tableau est responsive : en dessous de 768px, il scrolle horizontalement. La colonne d'actions reste sticky à droite.
- Le tri est interne (côté client) avec `sortedRows`. Pour un tri serveur, écouter l'événement `sort` et mettre à jour `rows`.
- Les transitions `TransitionGroup` animent l'entrée/sortie des lignes.
- La toolbar par défaut contient 6 boutons placeholder — toujours la remplacer via le slot `toolbar` ou passer un `toolbarActions` adapté.
- Utilise `UiTableCell` en interne pour le rendu — pas besoin de l'importer dans la page.
