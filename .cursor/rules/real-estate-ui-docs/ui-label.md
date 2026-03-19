# UiLabel

> Libellé de formulaire conforme au DS. À utiliser au-dessus des champs pour l’accessibilité et la cohérence visuelle.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `label` | `string` | *(requis)* | Texte du libellé |
| `icon` | `string \| Component \| FunctionalComponent \| false` | — | Icône à droite du texte ; `false` pour désactiver |
| `required` | `boolean` | `false` | Affiche un astérisque rouge après le texte |
| `tooltip` | `string` | — | Tooltip affiché au survol de l’icône. Si fourni sans `icon`, utilise `help-circle` par défaut |

## Slots

| Slot | Description |
|------|-------------|
| `icon` | Remplace l’icône par défaut par un contenu personnalisé |

## Événements

Le composant n’émet pas d’événements.

## Exemples

### Usage basique

```vue
<UiLabel label="Nom du champ" />
<UiInput v-model="value" />
```

### Champ requis

```vue
<UiLabel label="Email" required />
<UiInput v-model="email" />
```

### Avec icône et tooltip

```vue
<UiLabel label="Aide" icon="help-circle" tooltip="Cliquez pour plus d'infos" />
```

### Icône par défaut avec tooltip

```vue
<!-- Affiche automatiquement help-circle si tooltip fourni sans icon -->
<UiLabel label="Mot de passe" tooltip="Minimum 8 caractères" />
```

## Bonnes pratiques

- Toujours mettre un libellé au-dessus des champs de formulaire.
- Utiliser `required` pour les champs obligatoires (cohérent avec la validation).
- Utiliser `tooltip` pour les explications sans encombrer l’interface.
- Préférer le prop `label` du composant parent (`UiInput`, `UiSelect`, etc.) plutôt qu’un `UiLabel` séparé quand le DS le propose.
- Garder les libellés courts et clairs.

## À ne pas faire

- Ne pas utiliser `<label>` natif à la place de `UiLabel` pour les formulaires.
- Ne pas oublier le `required` sur les champs obligatoires.
- Ne pas mettre un libellé vide ou redondant.
- Ne pas utiliser de texte trop long (risque de débordement).
- Ne pas placer plusieurs `UiLabel` pour un même champ.

## Notes d'intégration

- Intégré par défaut dans `UiInput`, `UiSelect`, `UiTextarea` via le prop `label`.
- Utiliser `UiLabel` seul uniquement pour des champs custom (ex: slider, date picker).
- Icônes : format kebab-case → chargement depuis `@tabler/icons-vue`.
- Si `icon={false}` est fourni avec un `tooltip`, l’icône n’est pas affichée.
