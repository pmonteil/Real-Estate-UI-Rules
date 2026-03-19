# UiInputSuffix

> Champ de saisie numérique avec un suffixe visuel (unité) à droite : m², €, %, px, etc. Supporte une icône à gauche et un état d’erreur avec tooltip.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `modelValue` | `string \| number` | `""` | Valeur du champ (liée par v-model) |
| `label` | `string` | — | Label au-dessus du champ |
| `labelIcon` | `string \| Component` | — | Icône d'aide pour le label |
| `labelRequired` | `boolean` | `false` | Indique un champ obligatoire |
| `labelTooltip` | `string` | — | Tooltip de l'icône d'aide |
| `placeholder` | `string` | `""` | Placeholder du champ |
| `suffix` | `string` | `"m²"` | Texte du suffixe (unité) affiché à droite |
| `iconLeft` | `string \| Component` | — | Icône affichée à gauche du champ (ex: "ruler" pour surface) |
| `disabled` | `boolean` | `false` | Désactive le champ |
| `error` | `boolean` | `false` | État d'erreur (bordure rouge, icône erreur) |
| `errorMessage` | `string` | — | Message d'erreur affiché dans un tooltip au survol de l'icône |
| `hintText` | `string` | — | Texte d'aide sous le champ |

## Slots

| Slot | Description |
|------|-------------|
| — | Aucun slot. Le contenu est entièrement piloté par les props. |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `(value: string \| number)` | Valeur saisie (le champ est `type="number"`) |
| `focus` | `(event: FocusEvent)` | Focus sur le champ |
| `blur` | `(event: FocusEvent)` | Perte de focus |

## Exemples

### Usage basique

```vue
<template>
  <UiInputSuffix
    v-model="surfaceValue"
    label="Surface"
    placeholder="0"
    suffix="m²"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { UiInputSuffix } from 'septeo-real-estate-ui';

const surfaceValue = ref('');
</script>
```

### Avec différentes unités

```vue
<!-- Prix en euros -->
<UiInputSuffix
  v-model="priceValue"
  label="Prix"
  placeholder="0"
  suffix="€"
/>

<!-- Surface avec icône -->
<UiInputSuffix
  v-model="surfaceValue"
  label="Surface"
  placeholder="0"
  suffix="m²"
  icon-left="ruler"
/>

<!-- Pourcentage -->
<UiInputSuffix
  v-model="percentValue"
  label="Taux"
  placeholder="0"
  suffix="%"
/>
```

### Avec état d'erreur

```vue
<UiInputSuffix
  v-model="value"
  label="Surface"
  suffix="m²"
  :error="!!errors.surface"
  error-message="La surface doit être supérieure à 0"
/>
```

### Méthodes exposées

```vue
<UiInputSuffix ref="inputRef" v-model="value" label="Prix" suffix="€" />

<script setup lang="ts">
const inputRef = ref();

function focusField() {
  inputRef.value?.focus();
}
function blurField() {
  inputRef.value?.blur();
}
</script>
```

## Bonnes pratiques

- **Unité adaptée** : Utiliser `suffix` pour l’unité métier (m², €, %, px, etc.) plutôt qu’un label générique.
- **Icône contextuelle** : `iconLeft` pour renforcer le sens (règle pour surface, euro pour prix).
- **Champ numérique** : Le champ est `type="number"` ; les spinners sont masqués pour un rendu sobre.
- **Erreurs** : Fournir `errorMessage` pour que l’utilisateur voie le détail de l’erreur au survol de l’icône.
- **Cohérence** : Utiliser ce composant pour tous les champs numériques avec unité fixe, pas un `UiInput` custom.

## À ne pas faire

- **Ne pas utiliser UiInput classique** : Pour un champ avec unité visuelle, `UiInputSuffix` est le composant approprié.
- **Ne pas mélanger avec UiDynamicInput** : Ce dernier sert aux tags ; `UiInputSuffix` est pour une valeur numérique unique avec unité.
- **Ne pas oublier le suffix** : Si l’unité n’est pas évidente, toujours préciser le suffix pour éviter les ambiguïtés.
- **Ne pas hardcoder les unités** : Passer le suffix en prop pour réutiliser le composant (m², €, %, etc.).
- **Dépendance Quasar** : Le composant utilise `q-tooltip` pour `errorMessage`. S’assurer que Quasar est disponible ou adapter le tooltip si besoin.

## Notes d'intégration

- Dépend de `UiLabel` et de `q-tooltip` (Quasar) pour le message d’erreur.
- Icônes : `iconLeft` et `exclamation-circle` (erreur) chargées depuis `@tabler/icons-vue`.
- Le suffix est positionné en absolu à droite, largeur 36px, avec fond `--surface-default-bis` et bordure alignée au champ.
- États : hover (fond blanc, animation « bounce » sur l’icône), focus (bordure bleue), error (bordure rouge, icône erreur), disabled (style grisé).
- En `disabled` ou `error`, le suffix devient transparent avec seule une bordure gauche pour rester discret.
