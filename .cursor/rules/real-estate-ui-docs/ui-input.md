# UiInput

> Champ de saisie texte conforme au DS. Remplace l’`<input>` natif pour tous les formulaires.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `modelValue` | `string` | `''` | Valeur liée (v-model) |
| `label` | `string` | — | Libellé affiché au-dessus du champ |
| `labelIcon` | `string \| Component \| FunctionalComponent` | — | Icône du label (ex: `"help-circle"`) |
| `labelRequired` | `boolean` | `false` | Affiche un astérisque pour champ requis |
| `labelTooltip` | `string` | — | Tooltip affiché au survol de l’icône du label |
| `placeholder` | `string` | `''` | Texte de placeholder |
| `disabled` | `boolean` | `false` | Désactive le champ |
| `error` | `boolean` | `false` | Met le champ en état d’erreur (bordure + icône) |
| `errorMessage` | `string` | — | Message d’erreur affiché dans un tooltip sur l’icône |
| `hintText` | `string` | — | Texte d’aide sous le champ |
| `iconLeft` | `string \| Component \| FunctionalComponent` | — | Icône à gauche dans le champ (ex: `"search"`) |

## Slots

Le composant n'expose pas de slots publics.

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `string` | Émis à chaque frappe |
| `focus` | `FocusEvent` | Émis au focus |
| `blur` | `FocusEvent` | Émis à la perte de focus |

## Exemples

### Usage basique

```vue
<UiInput v-model="email" label="Email" placeholder="exemple@agence.fr" />
```

### Avec label requis et tooltip

```vue
<UiInput
  v-model="nom"
  label="Nom du contact"
  label-required
  label-tooltip="Indiquez le nom complet du contact"
/>
```

### Champ recherche avec icône

```vue
<UiInput
  v-model="searchQuery"
  placeholder="Rechercher un bien"
  icon-left="search"
/>
```

### État erreur

```vue
<UiInput
  v-model="email"
  label="Email"
  error
  error-message="Format d'email invalide"
/>
```

### Champ désactivé avec hint

```vue
<UiInput
  v-model="readOnlyValue"
  label="Référence"
  disabled
  hint-text="Cette valeur est générée automatiquement"
/>
```

## Bonnes pratiques

- **Toujours associer** une `UiLabel` ou utiliser le prop `label` pour l’accessibilité.
- Utiliser `labelRequired` pour les champs obligatoires.
- Préférer `hintText` pour les explications, `errorMessage` pour les erreurs de validation.
- Utiliser `iconLeft="search"` pour les champs de recherche.
- Laisser le composant gérer les états visuels (focus, filled, error).

## À ne pas faire

- **Ne pas utiliser** `<input>` natif à la place de `UiInput`.
- Ne pas dupliquer le label en dehors du composant si `label` est utilisé.
- Ne pas forcer de styles (couleurs, bordures) : le composant les gère.
- Ne pas utiliser des valeurs en dur : privilégier les tokens du DS.
- Le composant utilise actuellement `type="text"` en interne ; pour date/heure, privilégier un composant adapté (ex. `UiDynamicInput` ou champ date du DS).

## Notes d'intégration

- Intègre `UiLabel` en interne quand `label` est fourni.
- Expose `focus()` et `blur()` via `defineExpose`.
- Icônes : format kebab-case (ex: `"search"`, `"mail"`) → chargement depuis `@tabler/icons-vue`.
- En erreur, une icône `exclamation-circle` apparaît à droite avec tooltip.
