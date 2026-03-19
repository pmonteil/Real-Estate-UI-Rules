# UiRadio

> Bouton radio conforme au DS. Pour choisir une seule option parmi plusieurs (groupe exclusif).

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `modelValue` | `string \| number \| boolean` | — | Valeur du groupe (v-model sur le parent) |
| `value` | `string \| number \| boolean` | — | Valeur de cette option quand elle est sélectionnée |
| `name` | `string` | — | Attribut `name` du radio natif (groupe logique) |
| `label` | `string` | — | Libellé affiché à droite du cercle |
| `disabled` | `boolean` | `false` | Désactive cette option |
| `error` | `boolean` | `false` | État d’erreur (bordure rouge) |

## Slots

Le composant n'expose pas de slots publics.

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `string \| number \| boolean` | Émis quand cette option est sélectionnée |
| `change` | `string \| number \| boolean` | Émis au changement (même payload) |
| `focus` | `FocusEvent` | Émis au focus |
| `blur` | `FocusEvent` | Émis à la perte de focus |

## Exemples

### Groupe de radios (choix unique)

```vue
<script setup>
const typeCampagne = ref('email');
</script>

<template>
  <div class="radio-group">
    <UiLabel label="Type de campagne" required />
    <div class="radio-options">
      <UiRadio v-model="typeCampagne" value="email" label="Email" name="type" />
      <UiRadio v-model="typeCampagne" value="sms" label="SMS" name="type" />
      <UiRadio v-model="typeCampagne" value="push" label="Push" name="type" />
    </div>
  </div>
</template>
```

### Avec valeur numérique

```vue
<UiRadio v-model="priority" :value="1" label="Basse" name="priority" />
<UiRadio v-model="priority" :value="2" label="Normale" name="priority" />
<UiRadio v-model="priority" :value="3" label="Haute" name="priority" />
```

### États

```vue
<UiRadio v-model="choice" value="a" label="Option A" />
<UiRadio v-model="choice" value="b" label="Option B (désactivée)" disabled />
<UiRadio v-model="choice" value="c" label="Option C (erreur)" error />
```

## Bonnes pratiques

- Utiliser `UiRadio` pour un choix unique et exclusif parmi plusieurs options.
- Toujours regrouper les radios avec le même `name` et le même `v-model` (sur le parent).
- Fournir un `label` clair pour chaque option.
- Donner une valeur par défaut au `modelValue` du parent si une option doit être présélectionnée.
- Utiliser `error` sur tout le groupe en cas de validation échouée (ou sur le container).

## À ne pas faire

- Ne pas utiliser `<input type="radio">` natif à la place de `UiRadio`.
- Ne pas utiliser des radios pour des choix multiples : préférer `UiCheckbox`.
- Ne pas oublier le prop `value` sur chaque `UiRadio`.
- Ne pas mélanger des types (`string` et `number`) dans un même groupe si possible.
- Ne pas utiliser plusieurs `v-model` pour un même groupe : un seul pour tout le groupe.

## Notes d'intégration

- Expose `focus()` et `blur()` via `defineExpose`.
- Basé sur un radio natif masqué (accessibilité conservée).
- `modelValue` doit être géré au niveau du parent et passé à chaque `UiRadio`.
- Une seule option peut être sélectionnée : `isChecked = modelValue === value`.
- Taille du cercle : 18×18px.
- Le composant utilise une icône checkmark à l’intérieur du cercle quand coché (style DS).
