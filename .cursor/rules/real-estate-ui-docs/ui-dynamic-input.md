# UiDynamicInput

> Champ de saisie sous forme de tags : l'utilisateur ajoute des valeurs (balises) en tapant puis Entrée, et peut les supprimer avec Backspace ou le bouton ×. Idéal pour les listes de mots-clés, variables dynamiques ou champs multi-valeurs. Suffix `{ }` pour insertion de variables (contexte IA).

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `modelValue` | `string[]` | `[]` | Tableau des tags affichés |
| `label` | `string` | — | Label au-dessus du champ |
| `labelIcon` | `string \| Component` | — | Icône d'aide pour le label |
| `labelRequired` | `boolean` | — | Marque le champ comme requis |
| `labelTooltip` | `string` | — | Tooltip de l'icône d'aide |
| `placeholder` | `string` | `"Ajouter une valeur…"` | Placeholder quand il n'y a aucun tag |
| `hintText` | `string` | — | Texte d'aide sous le champ |
| `error` | `boolean` | `false` | État d'erreur (bordure rouge) |
| `errorMessage` | `string` | — | Message d'erreur (non géré en interne dans ce composant) |
| `disabled` | `boolean` | `false` | Désactive la saisie et la suppression |

## Slots

| Slot | Description |
|------|-------------|
| — | Aucun slot. Le contenu est entièrement piloté par `modelValue` et les événements. |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `(value: string[])` | Nouveau tableau de tags après ajout ou suppression |
| `suffix-click` | — | Clic sur le bouton `{ }` (suffix) |
| `add` | `(tag: string)` | Émis lors de l'ajout d'un tag |
| `remove` | `(tag: string, index: number)` | Émis lors de la suppression d'un tag |

## Exemples

### Usage basique

```vue
<template>
  <UiDynamicInput
    v-model="tags"
    label="Mots-clés"
    placeholder="Ajouter un mot-clé…"
    @add="onTagAdded"
    @remove="onTagRemoved"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { UiDynamicInput } from 'septeo-real-estate-ui';

const tags = ref<string[]>([]);

function onTagAdded(tag: string) {
  console.log('Ajouté:', tag);
}
function onTagRemoved(tag: string, index: number) {
  console.log('Supprimé:', tag, 'à l\'index', index);
}
</script>
```

### Avec suffix pour variables (contexte IA)

```vue
<UiDynamicInput
  v-model="variables"
  label="Variables"
  placeholder="Ajouter une variable…"
  hint-text="Utilisez { } pour insérer des variables dynamiques"
  @suffix-click="openVariablePicker"
/>
```

### Comportement clavier

- **Entrée** : ajoute le texte saisi comme nouveau tag (trimmed)
- **Backspace** (champ vide) : supprime le dernier tag
- **Blur** : si du texte non vide reste, il est ajouté comme tag

### Avec état d'erreur

```vue
<UiDynamicInput
  v-model="tags"
  label="Tags"
  :error="!!errors.tags"
  :error-message="errors.tags"
/>
```

### Méthode exposée

```vue
<UiDynamicInput ref="inputRef" v-model="tags" />

<script setup lang="ts">
const inputRef = ref();

function focusField() {
  inputRef.value?.focusInput();
}
</script>
```

## Bonnes pratiques

- **Contexte d'usage** : Variables dynamiques (email builder), mots-clés, tags, listes de valeurs non prédéfinies.
- **Validation** : Contrôler la longueur, l’unicité ou le format des tags côté parent via `@add` ou en réagissant à `update:modelValue`.
- **Suffix `{ }`** : Utiliser `@suffix-click` pour ouvrir un picker de variables (ex. prénom, nom, email) dans les contextes IA ou templates.
- **Placeholder** : S’adapte automatiquement : visible uniquement quand `modelValue.length === 0`.
- **Données** : Toujours initialiser `modelValue` avec un tableau (`[]` ou `ref([])`), jamais `undefined` ou `null`.

## À ne pas faire

- **Ne pas utiliser UiInput classique** : Pour une liste de valeurs dynamiques, `UiDynamicInput` est le bon choix.
- **Ne pas utiliser pour une sélection parmi des options fixes** : Dans ce cas, préférer `UiSelect` multi ou des checkboxes.
- **Ne pas ignorer les doublons** : Le composant n’empêche pas les doublons ; les gérer dans `@add` ou après `update:modelValue` si nécessaire.
- **Ne pas utiliser pour un champ texte simple** : Pour une seule valeur texte, utiliser `UiInput`.

## Notes d'intégration

- Dépend de `UiLabel`.
- Icônes : `x` (fermeture tag), `braces` (suffix) chargées depuis `@tabler/icons-vue`.
- Style IA : bordure violette au focus (`--alias-ai-500`), tags avec dégradé (`--surface-ai-gradient-1/2`).
- Le suffix est positionné en absolu à droite du champ, avec une largeur fixe de 36px.
- En mode `disabled`, les tags et le bouton de suppression restent visibles mais non interactifs.
