# UiTextarea

> Zone de texte multi-lignes conforme au DS. Remplace le `<textarea>` natif pour les saisies longues.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `modelValue` | `string` | `''` | Valeur liée (v-model) |
| `label` | `string` | — | Libellé affiché au-dessus du champ |
| `labelIcon` | `string \| Component \| FunctionalComponent` | — | Icône du label |
| `labelRequired` | `boolean` | `false` | Affiche un astérisque pour champ requis |
| `labelTooltip` | `string` | — | Tooltip sur l’icône du label |
| `placeholder` | `string` | `''` | Texte de placeholder |
| `disabled` | `boolean` | `false` | Désactive le champ |
| `error` | `boolean` | `false` | État d’erreur (bordure + icône) |
| `errorMessage` | `string` | — | Message d’erreur dans un tooltip |
| `infoMessage` | `string` | — | Message d’information (icône info) |
| `hintText` | `string` | — | Texte d’aide sous le champ |
| `rows` | `number` | `5` | Nombre de lignes visibles (hauteur minimale) |

## Slots

Le composant n'expose pas de slots publics.

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `string` | Émis à chaque modification du texte |
| `focus` | `FocusEvent` | Émis au focus |
| `blur` | `FocusEvent` | Émis à la perte de focus |

## Exemples

### Usage basique

```vue
<UiTextarea v-model="message" label="Message" placeholder="Saisissez votre message..." />
```

### Avec nombre de lignes

```vue
<UiTextarea v-model="description" label="Description" :rows="8" />
```

### État erreur

```vue
<UiTextarea
  v-model="content"
  label="Contenu du mail"
  error
  error-message="Le contenu est obligatoire"
/>
```

### Avec hint et info

```vue
<UiTextarea
  v-model="notes"
  label="Notes"
  hint-text="Ces notes ne sont visibles que par vous"
  info-message="Conseil : soyez précis pour un meilleur suivi"
/>
```

## Bonnes pratiques

- Utiliser `UiTextarea` pour toute saisie multi-ligne (description, message, notes).
- Ajuster `rows` selon le contexte (court = 3–4, long = 8+).
- Toujours utiliser le prop `label` pour l’accessibilité.
- Utiliser `hintText` pour les informations complémentaires.
- Utiliser `error` et `errorMessage` pour la validation.

## À ne pas faire

- Ne pas utiliser `<textarea>` natif à la place de `UiTextarea`.
- Ne pas définir `rows` trop petit (moins de 3) pour une bonne lisibilité.
- Ne pas l’utiliser pour une saisie sur une seule ligne (préférer `UiInput`).
- Le composant utilise `resize: none` ; ne pas tenter de le rendre redimensionnable via CSS.

## Notes d'intégration

- Intègre `UiLabel` quand `label` est fourni.
- Expose `focus()` et `blur()` via `defineExpose`.
- Icône erreur : `exclamation-circle` à droite avec tooltip si `errorMessage` est fourni.
- Icône info : `info-circle` à droite si `infoMessage` est fourni (sans `error`).
- `hintText` passe en rouge quand `error` est activé.
