# UiTableCell

> Composant de cellule de tableau utilisé en interne par UiTable. Affiche du texte, un lien, des actions ou une pill selon le type.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `type` | `'text' \| 'link' \| 'action' \| 'pill'` | `'text'` | Type de contenu de la cellule |
| `value` | `string` | `""` | Valeur affichée (texte ou libellé du lien) |
| `iconLeft` | `string \| Component` | — | Icône à gauche (type text) — nom Tabler ou composant |
| `iconRight` | `string \| Component` | — | Icône à droite (type text) |
| `linkIconLeft` | `string` | — | Icône du lien (type link) |
| `linkIconRight` | `string` | — | Icône du lien (type link) |
| `href` | `string` | — | URL pour le lien externe (type link) |
| `to` | `string \| object` | — | Route pour Vue Router (type link) |
| `actions` | `ActionItem[]` | Défaut: view/edit/menu | Boutons d’action (type action) |
| `pillLabel` | `string` | `"En cours"` | Libellé de la pill |
| `pillIcon` | `string` | — | Nom d’icône Tabler pour la pill |
| `pillColor` | `'blue' \| 'red' \| 'orange' \| 'green' \| 'purple' \| 'grey'` | `'blue'` | Couleur de la pill |

### ActionItem

```ts
interface ActionItem {
  name: string;
  icon: string;
  secondary?: boolean;   // Style plus discret
  variant?: 'ghost' | 'error' | 'transparent';
}
```

## Slots

Aucun slot. Le contenu est entièrement piloté par les props.

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `action` | `name: string` | Émis au clic sur un bouton d’action |

## Exemples

### Usage direct (sans UiTable)

```vue
<UiTableCell type="text" value="Jean Dupont" icon-left="user" />
<UiTableCell type="link" value="Voir la fiche" href="/fiche/1" />
<UiTableCell
  type="action"
  :actions="[
    { name: 'edit', icon: 'pencil' },
    { name: 'delete', icon: 'trash', variant: 'error' },
  ]"
  @action="handleAction"
/>
<UiTableCell type="pill" pill-label="Validé" pill-color="green" pill-icon="check" />
```

### Usage via UiTable (via config colonne)

UiTableCell est appelé automatiquement par UiTable. La configuration se fait sur la colonne :

```ts
{
  key: 'nom',
  label: 'Nom',
  type: 'text',
  iconLeft: 'user',
}
{
  key: 'statut',
  label: 'Statut',
  type: 'pill',
  pillColor: 'statutColor',
  pillIcon: 'clock',
}
{
  key: 'actions',
  type: 'action',
  actions: [
    { name: 'view', icon: 'eye' },
    { name: 'edit', icon: 'pencil' },
    { name: 'menu', icon: 'dots-vertical', secondary: true },
  ],
}
```

## Bonnes pratiques

- Utiliser UiTableCell principalement via la configuration des colonnes de UiTable.
- Pour des actions destructives, utiliser `variant: 'error'` sur le bouton concerné.
- Pour des actions secondaires (menu à 3 points), utiliser `secondary: true`.
- Les icônes sont des noms Tabler en kebab-case (`pencil`, `dots-vertical`, `trash`).

## À ne pas faire

- Ne pas utiliser UiTableCell en dehors d’un contexte de tableau sans raison particulière.
- Éviter de passer à la fois `href` et `to` pour une même cellule de type `link`.
- Ne pas oublier de gérer l’événement `action` si la colonne contient des boutons cliquables.

## Notes d'intégration

- **Dépendances** : UiLink, UiButton, UiPill.
- **UiTable** : utilise UiTableCell pour chaque cellule ; pas besoin d’appeler UiTableCell directement dans la plupart des cas.
- Les icônes sont chargées depuis `@tabler/icons-vue` ; les noms sont convertis en PascalCase en interne (ex. `pencil` → `IconPencil`).
