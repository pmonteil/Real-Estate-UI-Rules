# UiCheckbox

> Case à cocher conforme au DS. Pour les choix multiples (sélection de 0 à N options).

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `modelValue` | `boolean` | `false` | État coché/décoché (v-model) |
| `label` | `string` | — | Libellé affiché à droite de la case |
| `disabled` | `boolean` | `false` | Désactive la checkbox |
| `error` | `boolean` | `false` | État d’erreur (bordure rouge) |

## Slots

Le composant n'expose pas de slots publics.

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `boolean` | Émis au changement d’état |
| `change` | `boolean` | Émis au changement (même payload) |
| `focus` | `FocusEvent` | Émis au focus |
| `blur` | `FocusEvent` | Émis à la perte de focus |

## Exemples

### Usage basique

```vue
<UiCheckbox v-model="acceptTerms" label="Accepter les conditions" />
```

### Dans un tableau (sélection multiple)

```vue
<UiCheckbox v-model="filter.actif" :label="filter.nom" />
```

### États

```vue
<UiCheckbox v-model="checked" label="Option activée" />
<UiCheckbox v-model="invalid" label="Champ requis" error />
<UiCheckbox :model-value="false" label="Non modifiable" disabled />
```

## Bonnes pratiques

- Utiliser `UiCheckbox` pour les sélections multiples (plusieurs options cochables).
- Pour un seul choix binaire (on/off), préférer `UiSwitch` (feedback plus explicite).
- Toujours fournir un `label` descriptif.
- Utiliser `error` pour les champs obligatoires non cochés en validation.
- Grouper les checkboxes sémantiquement (ex: options liées à une section).

## À ne pas faire

- Ne pas utiliser `<input type="checkbox">` natif à la place de `UiCheckbox`.
- Ne pas utiliser une checkbox pour un choix unique exclusif : préférer `UiRadio`.
- Ne pas utiliser une checkbox pour un simple toggle binaire : préférer `UiSwitch`.
- Ne pas oublier le `label` pour l’accessibilité.
- Ne pas forcer des styles custom sur la case.

## Notes d'intégration

- Expose `focus()`, `blur()` et `toggle()` via `defineExpose`.
- Basé sur une checkbox native masquée (accessibilité conservée).
- Taille de la case : 18×18px.
- Utilisé dans `BiensTable` via `h(UiCheckbox, {...})` pour les lignes de tableau.
- Pour des listes de checkboxes, gérer le state au niveau parent (tableau de booléens ou Set d’IDs).
