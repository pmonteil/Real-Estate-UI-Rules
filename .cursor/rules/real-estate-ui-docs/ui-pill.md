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

- **Toujours choisir la couleur en fonction du sens sémantique du statut :**

| Couleur | Signification | Exemples |
|---------|--------------|----------|
| `green` | Succès, validé, actif, positif | Validé, Actif, Publié, Effectuée, En ligne, Accepté |
| `red` | Erreur, rejeté, négatif, perdu | Rejeté, Échoué, Erreur, Annulé, Perdu, Supprimé |
| `blue` | En cours, info, neutre actif | En cours, Planifié, En traitement, En révision |
| `orange` | Attention, en attente, alerte | En attente, À vérifier, En retard, Expiré bientôt |
| `grey` | Inactif, brouillon, archivé | Brouillon, Archivé, Inactif, Fermé, Non défini |
| `purple` | Premium, IA, spécial | Premium, VIP, IA, Automatisé |

- Associer une icône si elle clarifie le statut (`check` pour validé, `x` pour rejeté, `clock` pour en cours, etc.).
- Utiliser des libellés courts et clairs (« En cours », « Validé », « Brouillon »).

## À ne pas faire

- Ne pas utiliser UiPill pour des comptages numériques — utiliser **UiBadge**.
- Éviter des libellés trop longs qui déforment la pill.
- **Ne jamais choisir une couleur incohérente** avec le sens du statut (ex. vert pour une erreur, rouge pour un succès). C'est un bug.

## Notes d'intégration

- **UiTableCell** : utilise UiPill en interne pour les colonnes de type `pill`.
- **UiTable** : passer `pillColor` et `pillIcon` via la configuration de colonne (clés de row ou valeurs par défaut).
- Les icônes sont chargées depuis `@tabler/icons-vue` (`clock`, `check`, `x`, `alert-triangle`, `star`, `archive`, etc.).
