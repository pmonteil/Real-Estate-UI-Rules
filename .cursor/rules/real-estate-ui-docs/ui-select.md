# UiSelect

> Liste déroulante conforme au DS. Remplace le `<select>` natif pour choisir une option parmi plusieurs.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `modelValue` | `string \| number \| null` | `null` | Valeur sélectionnée (v-model) |
| `options` | `SelectOption[] \| (string \| number)[]` | `[]` | Liste des options (objet `{ label, value, disabled? }` ou valeur simple) |
| `label` | `string` | — | Libellé au-dessus du champ |
| `labelIcon` | `string \| Component \| FunctionalComponent` | — | Icône du label |
| `labelRequired` | `boolean` | `false` | Affiche un astérisque |
| `labelTooltip` | `string` | — | Tooltip sur l’icône du label |
| `placeholder` | `string` | `'placeholder'` | Texte affiché quand aucune option n’est sélectionnée |
| `disabled` | `boolean` | `false` | Désactive le champ |
| `error` | `boolean` | `false` | État d’erreur |
| `errorMessage` | `string` | — | Message d’erreur dans un tooltip |
| `hintText` | `string` | — | Texte d’aide sous le champ |
| `iconLeft` | `string \| Component \| FunctionalComponent` | — | Icône à gauche dans le champ |

## Slots

Le composant n'expose pas de slots publics.

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `string \| number \| null` | Émis à la sélection d’une option |
| `change` | `string \| number \| null` | Émis à la sélection (même payload que `update:modelValue`) |
| `focus` | `FocusEvent` | Émis au focus |
| `blur` | `FocusEvent` | Émis à la perte de focus |

## Interface SelectOption

```typescript
interface SelectOption {
  label: string;
  value: string | number;
  disabled?: boolean;
}
```

## Exemples

### Usage basique (objets)

```vue
<UiSelect
  v-model="selected"
  label="Statut"
  placeholder="Choisir un statut"
  :options="[
    { label: 'En attente', value: 'pending' },
    { label: 'Validé', value: 'approved' },
    { label: 'Refusé', value: 'rejected' },
  ]"
/>
```

### Options simples

```vue
<UiSelect
  v-model="visiblePour"
  label="Visible pour"
  :options="['Tous', 'Admins uniquement', 'Agents uniquement']"
/>
```

### Options avec valeurs désactivées

```vue
<UiSelect
  v-model="mode"
  label="Mode"
  :options="[
    { label: 'Normal', value: 'normal' },
    { label: 'Avancé (bientôt)', value: 'advanced', disabled: true },
  ]"
/>
```

### Avec erreur et hint

```vue
<UiSelect
  v-model="expediteur"
  label="Adresse expéditeur"
  placeholder="Choisir un expéditeur"
  :options="senders"
  error
  error-message="Sélectionnez une adresse"
  hint-text="L'adresse visible dans les emails envoyés"
/>
```

## Bonnes pratiques

- Utiliser des objets `{ label, value }` pour un label affiché différent de la valeur.
- Prévoir un `placeholder` explicite au lieu de la valeur par défaut `"placeholder"`.
- Toujours fournir `label` pour l’accessibilité.
- Utiliser `disabled` sur une option pour les choix indisponibles sans la masquer.
- Grouper les options par catégorie si besoin (via plusieurs `UiSelect` ou un composant de groupe).

## À ne pas faire

- Ne pas utiliser `<select>` natif à la place de `UiSelect`.
- Ne pas oublier de mapper correctement `options` : `{ label, value }` ou tableau de valeurs simples.
- Ne pas utiliser des options vides sans `placeholder` approprié.
- Ne pas forcer des styles custom sur le dropdown.
- Ne pas fournir des options avec des `value` en doublon.

## Notes d'intégration

- Intègre `UiLabel` quand `label` est fourni.
- Expose `open()`, `close()` et `focus()` via `defineExpose`.
- Format d’options : objet `{ label, value, disabled? }` ou valeur simple (string/number) pour label = value.
- Navigation clavier : Entrée/Espace pour ouvrir, flèches pour naviguer, Échap pour fermer.
- Fermeture automatique au clic en dehors du champ.
