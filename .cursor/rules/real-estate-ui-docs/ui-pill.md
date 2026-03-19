# UiPill

> Pastille colorée pour afficher un statut ou une catégorie (ex. « En cours », « Validé », « Rejeté »). Peut inclure une icône optionnelle.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `label` | `string` | `"Label"` | Texte affiché dans la pill |
| `icon` | `string \| Component` | — | Icône Tabler (nom kebab-case, ex. `clock`, `check`, `x`) |
| `color` | `'blue' \| 'red' \| 'orange' \| 'green' \| 'purple' \| 'grey'` | `'blue'` | Couleur de la pill |
| `type` | `'default' \| 'rounded'` | `'default'` | `rounded` donne des bords arrondis (100px) |

## Slots

Aucun slot.

## Événements

Aucun événement.

## Exemples

### Usage basique

```vue
<UiPill label="En cours" color="blue" />
<UiPill label="Validé" color="green" icon="check" />
<UiPill label="Rejeté" color="red" icon="x" />
<UiPill label="En attente" color="orange" icon="alert-triangle" />
<UiPill label="Premium" color="purple" icon="star" />
<UiPill label="Archivé" color="grey" icon="archive" />
```

### Dans un tableau (via UiTable)

```ts
const tableColumns = [
  {
    key: 'statut',
    label: 'Statut',
    type: 'pill',
    pillColor: 'statutColor',
    pillIcon: undefined,
  },
];

const formattedRows = leads.map(lead => ({
  statut: getStatusLabel(lead.status),
  statutColor: getStatusColor(lead.status),  // 'green' | 'blue' | 'red' | 'grey'
}));
```

### Type rounded

```vue
<UiPill label="Nouveau" color="green" type="rounded" />
```

### Dans un drawer de détail

```vue
<UiPill
  :label="getStatusLabel(selectedLead.status)"
  :color="getStatusColor(selectedLead.status)"
/>
```

## Bonnes pratiques

- Choisir une couleur sémantique : `green` pour succès/validé, `red` pour erreur/rejeté, `blue` pour info/en cours, `orange` pour attention, `grey` pour neutre/archivé, `purple` pour premium/IA.
- Associer une icône uniquement si elle clarifie le statut.
- Utiliser des libellés courts et clairs (« En cours », « Validé », « Brouillon »).

## À ne pas faire

- Ne pas utiliser UiPill pour des comptages numériques — utiliser **UiBadge**.
- Éviter des libellés trop longs qui déforment la pill.
- Ne pas mélanger les conventions de couleurs (ex. vert pour erreur).

## Notes d'intégration

- **UiTableCell** : utilise UiPill en interne pour les colonnes de type `pill`.
- **UiTable** : passer `pillColor` et `pillIcon` via la configuration de colonne (clés de row ou valeurs par défaut).
- Les icônes sont chargées depuis `@tabler/icons-vue` (`clock`, `check`, `x`, `alert-triangle`, `star`, `archive`, etc.).
