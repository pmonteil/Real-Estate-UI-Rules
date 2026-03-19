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

## Boutons d'action de la toolbar — Guide

Les boutons de la toolbar doivent être **logiques et utiles** au contexte du tableau. Ne jamais laisser les 6 boutons placeholder par défaut. Toujours personnaliser la toolbar avec des actions pertinentes.

### Actions courantes et icônes associées

| Action | Icône Tabler | Variant | Quand l'utiliser |
|--------|-------------|---------|-----------------|
| Ajouter / Créer | `plus` | `primary` | Créer un nouvel élément dans le tableau |
| Exporter | `download` | `secondary` | Exporter les données (CSV, PDF) |
| Importer | `upload` | `secondary` | Importer des données |
| Rechercher | `search` | Champ `UiInput` | Recherche textuelle dans le tableau |
| Rafraîchir | `refresh` | `ghost` | Recharger les données |
| Paramètres colonnes | `columns` | `ghost` | Configurer les colonnes visibles |
| Supprimer sélection | `trash` | `error` | Supprimer les éléments sélectionnés |
| Dupliquer | `copy` | `secondary` | Dupliquer un ou plusieurs éléments |
| Filtrer | `filter` | via `UiFilter` | Filtrer les données |

### Exemple de toolbar adaptée au contexte

```vue
<UiTable :columns="columns" :rows="rows" title="Biens immobiliers">
  <template #toolbar>
    <UiFilter label="Statut" icon="circle-check" :active="!!filters.statut" @click="toggleFilter('statut')" />
    <UiFilter label="Type" icon="home" :active="!!filters.type" @click="toggleFilter('type')" />
    <UiInput v-model="search" placeholder="Rechercher un bien..." icon="search" />
    <UiButton label="Exporter" variant="secondary" icon="download" size="sm" @click="exportData" />
    <UiButton label="Ajouter un bien" variant="primary" icon="plus" size="sm" @click="create" />
  </template>
</UiTable>
```

## Actions de ligne — Guide

Les boutons d'action dans la colonne `action` doivent être pertinents. Voici les actions courantes :

| Action | Icône | Variant/Style | Usage |
|--------|-------|--------------|-------|
| Voir le détail | `eye` | défaut | Ouvrir le détail (drawer ou page) |
| Modifier | `pencil` | défaut | Éditer l'élément |
| Supprimer | `trash` | `variant: 'error'` | **Obligatoire** : toujours en variant error |
| Menu contextuel | `dots-vertical` | `secondary: true` | Actions secondaires (dupliquer, archiver…) |
| Télécharger | `download` | défaut | Télécharger un fichier lié |
| Envoyer | `send` | défaut | Envoyer (email, notification) |

```ts
actions: [
  { name: 'view', icon: 'eye' },
  { name: 'edit', icon: 'pencil' },
  { name: 'delete', icon: 'trash', variant: 'error' },
]
```

## Colonnes de statut — Pills et couleurs sémantiques

Quand une colonne représente un **état ou un statut**, toujours utiliser le type `pill` avec une couleur **sémantiquement logique**. Ne jamais choisir une couleur au hasard.

### Mapping des couleurs par catégorie de statut

| Catégorie | Couleur | Icône suggérée | Exemples de libellés |
|-----------|---------|---------------|---------------------|
| **Succès / Validé / Actif** | `green` | `check`, `circle-check` | Validé, Actif, Publié, Effectuée, Accepté, En ligne |
| **Erreur / Rejeté / Échoué** | `red` | `x`, `alert-circle` | Rejeté, Échoué, Erreur, Annulé, Supprimé, Perdu |
| **En cours / Info** | `blue` | `clock`, `loader` | En cours, En traitement, En révision, Planifié |
| **Attention / En attente** | `orange` | `alert-triangle`, `clock` | En attente, À vérifier, Expiré bientôt, En retard |
| **Brouillon / Neutre / Archivé** | `grey` | `archive`, `file` | Brouillon, Archivé, Inactif, Non défini, Fermé |
| **Premium / IA / Spécial** | `purple` | `star`, `sparkles` | Premium, IA, VIP, Automatisé |

### Exemple complet dans un tableau

```ts
function getStatusColor(status: string): string {
  const map: Record<string, string> = {
    'actif': 'green',
    'en_cours': 'blue',
    'en_attente': 'orange',
    'rejete': 'red',
    'brouillon': 'grey',
    'archive': 'grey',
    'effectuee': 'green',
    'perdu': 'red',
  };
  return map[status] ?? 'grey';
}

function getStatusIcon(status: string): string {
  const map: Record<string, string> = {
    'actif': 'circle-check',
    'en_cours': 'clock',
    'en_attente': 'alert-triangle',
    'rejete': 'x',
    'brouillon': 'file',
    'archive': 'archive',
    'effectuee': 'check',
    'perdu': 'x',
  };
  return map[status] ?? 'circle';
}

const columns = [
  { key: 'reference', label: 'Réf.', icon: 'hash', width: '100px' },
  { key: 'nom', label: 'Bien', type: 'link', to: 'detailRoute' },
  { key: 'statut', label: 'Statut', type: 'pill', pillColor: 'statutColor', pillIcon: 'statutIcon', width: '140px' },
  { key: 'actions', label: '', type: 'action', width: '100px', sortable: false,
    actions: [
      { name: 'view', icon: 'eye' },
      { name: 'edit', icon: 'pencil' },
      { name: 'delete', icon: 'trash', variant: 'error' },
    ]
  },
];

const rows = biens.map(b => ({
  reference: b.ref,
  nom: b.name,
  detailRoute: `/biens/${b.id}`,
  statut: getStatusLabel(b.status),
  statutColor: getStatusColor(b.status),
  statutIcon: getStatusIcon(b.status),
}));
```

## Bonnes pratiques

- Toujours utiliser `UiTable` au lieu de `<table>` natif pour les données tabulaires.
- Définir un `key` unique dans les données (`id` ou `reference`) pour les animations de transition.
- Utiliser `width` sur les colonnes de taille fixe (actions, statuts, dates) pour un rendu stable.
- **Toujours personnaliser la toolbar** avec des actions utiles et cohérentes au contexte. Ne jamais laisser les boutons par défaut.
- **Toujours mapper les statuts avec des couleurs logiques** via le tableau de référence ci-dessus. Une couleur incohérente (vert pour une erreur, rouge pour un succès) est un bug.
- Mettre `sortable: false` sur les colonnes d'action et les colonnes non-triables.
- Préférer le slot `toolbar` pour des toolbars custom (filtres, recherche).
- **Actions destructives** (supprimer) : toujours en `variant: 'error'` dans la colonne d'actions.

## À ne pas faire

- Ne pas construire un tableau avec `<table>` + CSS custom.
- Ne pas créer des cellules custom sans passer par le système `type` + `UiTableCell`.
- Ne pas oublier `row-clickable` si les lignes doivent être cliquables.
- Ne pas mettre un `title` ET un slot `toolbar` vide — le header ne s'affiche que si l'un des deux est défini.
- Ne pas utiliser des couleurs en dur pour les pills — toujours utiliser les noms de couleur du DS (`blue`, `green`, `red`, etc.).
- **Ne jamais laisser la toolbar par défaut** avec les 6 boutons placeholder.
- **Ne jamais choisir une couleur de pill au hasard** — suivre le mapping sémantique (vert = positif, rouge = négatif, etc.).
- Ne pas mettre une action `delete` sans `variant: 'error'`.

## Notes d'intégration

- Le tableau est responsive : en dessous de 768px, il scrolle horizontalement. La colonne d'actions reste sticky à droite.
- Le tri est interne (côté client) avec `sortedRows`. Pour un tri serveur, écouter l'événement `sort` et mettre à jour `rows`.
- Les transitions `TransitionGroup` animent l'entrée/sortie des lignes.
- La toolbar par défaut contient 6 boutons placeholder — toujours la remplacer via le slot `toolbar` ou passer un `toolbarActions` adapté.
- Utilise `UiTableCell` en interne pour le rendu — pas besoin de l'importer dans la page.
