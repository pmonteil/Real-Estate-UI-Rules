# UiToggle

> Sélecteur en forme de boutons groupés pour choisir une option parmi plusieurs. À distinguer de `UiSwitch` : le toggle permet de sélectionner une valeur parmi N options, le switch est un booléen on/off.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `options` | `ToggleOption[]` | — | Liste des options. Chaque option : `{ label, value, icon?: string, disabled?: boolean }` |
| `modelValue` | `string \| number` | — | Valeur sélectionnée (équivalent d’une des `option.value`) |
| `label` | `string` | — | Label affiché au-dessus du toggle |
| `labelIcon` | `string \| boolean` | `"help-circle"` | Icône d’aide à côté du label. `false` pour masquer |
| `labelTooltip` | `string` | — | Texte du tooltip associé à l’icône d’aide |
| `required` | `boolean` | `false` | Indique un champ obligatoire (astérisque) |

## Slots

| Slot | Description |
|------|-------------|
| — | Aucun slot. Le contenu provient des `options`. |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `(value: string \| number)` | Émis à la sélection d’une option pour mettre à jour v-model |

## Type ToggleOption

```ts
interface ToggleOption {
  label: string;
  value: string | number;
  icon?: string;        // nom Tabler (ex: "mail", "message")
  disabled?: boolean;
}
```

## Exemples

### Usage basique

```vue
<template>
  <UiToggle
    v-model="form.type"
    label="Type de message"
    :options="messageTypeOptions"
  />
</template>

<script setup lang="ts">
import { reactive } from 'vue';
import { UiToggle } from 'septeo-real-estate-ui';

const form = reactive({ type: 'email' });

const messageTypeOptions = [
  { value: 'email', label: 'Email', icon: 'mail' },
  { value: 'sms', label: 'SMS', icon: 'message', disabled: true },
];
</script>
```

### Avec icônes et option désactivée

```vue
<UiToggle
  v-model="viewMode"
  label="Affichage"
  :options="[
    { value: 'list', label: 'Liste', icon: 'list' },
    { value: 'grid', label: 'Grille', icon: 'layout-grid' },
    { value: 'map', label: 'Carte', icon: 'map', disabled: true },
  ]"
/>
```

### Sans icône de label

```vue
<UiToggle
  v-model="choice"
  label="Choix"
  :label-icon="false"
  :options="options"
/>
```

### Valeurs numériques

```vue
<UiToggle
  v-model="quantity"
  :options="[
    { value: 1, label: '1' },
    { value: 5, label: '5' },
    { value: 10, label: '10' },
  ]"
/>
```

## Bonnes pratiques

- **UiToggle vs UiSwitch** :
  - **UiToggle** : sélection parmi 2+ options (ex. Email / SMS, Liste / Grille).
  - **UiSwitch** : booléen on/off (ex. activer/désactiver une fonctionnalité).
- **Options claires** : Labels courts et explicites ; éviter plus de 4–5 options pour garder une UI lisible.
- **Valeur initiale** : S’assurer que `modelValue` correspond à une des `option.value` au chargement.
- **Icônes optionnelles** : Utiles pour des choix visuels (list/grid, mail/sms). Omettre `icon` si le texte suffit.

## À ne pas faire

- **Ne pas utiliser UiSwitch pour choisir entre options** : Un switch = on/off, pas une sélection multiple.
- **Ne pas utiliser UiRadio à la place** : Pour un groupe de boutons groupés style pill, `UiToggle` est plus adapté au DS.
- **Ne pas oublier `disabled`** : Pour des options indisponibles, utiliser `disabled: true` dans l’option plutôt qu’une logique externe.
- **Ne pas mélanger types de `value`** : Garder un type cohérent (tout `string` ou tout `number`) dans les options pour éviter les bugs de comparaison.

## Notes d'intégration

- Dépend de `UiLabel` pour le label.
- Icônes chargées dynamiquement depuis `@tabler/icons-vue` (convention `icon-name` → `IconName`).
- Style actif : dégradé orange (`--gradient-menu-active`) avec ombre. Cohérent avec le DS Real Estate.
- `white-space: nowrap` sur les boutons pour éviter le retour à la ligne des labels.
