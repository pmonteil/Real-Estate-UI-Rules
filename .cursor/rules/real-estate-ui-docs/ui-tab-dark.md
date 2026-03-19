# UiTabDark

> Variante sombre de `UiTab`, conçue pour les fonds foncés (topbar, sidebar, bannières). Même API que `UiTab` avec un style adapté aux arrière-plans sombres.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `tabs` | `Tab[]` | — | **Requis.** Tableau d'objets onglet. Format : `{ value, label, icon?, disabled? }` |
| `modelValue` | `string \| number` | — | **Requis.** Valeur de l'onglet actuellement actif (v-model) |

## Format Tab

```typescript
interface Tab {
  label: string;
  value: string | number;
  icon?: string;       // Nom d'icône Tabler (kebab-case)
  disabled?: boolean;
}
```

> **Note :** Contrairement à `UiTab`, `UiTabDark` n'a **pas** de prop `size` — taille fixe.

## Slots

| Slot | Description |
|------|-------------|
| — | Aucun slot. |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `string \| number` | Émis au clic sur un onglet non désactivé. |

## Exemples

### Usage basique

```vue
<script setup lang="ts">
import { ref } from 'vue';
import { UiTabDark } from 'septeo-real-estate-ui';

const activeTab = ref('overview');

const tabs = [
  { value: 'overview', label: 'Vue d\'ensemble', icon: 'layout-dashboard' },
  { value: 'analytics', label: 'Analytics', icon: 'chart-line' },
];
</script>

<template>
  <div class="dark-section">
    <UiTabDark v-model="activeTab" :tabs="tabs" />
  </div>
</template>

<style scoped>
.dark-section {
  background-color: #2E3862;
  padding: 1rem;
  border-radius: 8px;
}
</style>
```

### Avec onglets désactivés

```vue
const tabs = [
  { value: 'a', label: 'Onglet A' },
  { value: 'b', label: 'Onglet B', disabled: true },
];
```

## Bonnes pratiques

- Utiliser `UiTabDark` uniquement sur des fonds sombres (ex: `#2E3862`, topbar Office, bannière).
- Sur fond clair, utiliser `UiTab` pour une meilleure lisibilité.
- Les icônes et labels sont en blanc / blanc semi-transparent pour contraster sur le fond sombre.

## À ne pas faire

- Ne pas utiliser `UiTabDark` sur fond clair — le contraste serait insuffisant.
- Ne pas mélanger `UiTab` et `UiTabDark` dans le même bloc visuel.

## Notes d'intégration

- Le conteneur a un fond `rgba(36, 43, 74, 0.6)` et des bordures arrondies (100px).
- L'onglet actif utilise le même dégradé que `UiTab` (`--gradient-menu-active`) avec une ombre orange.
- Les icônes sont en 16px (taille fixe).
- `UiTabDark` n'est pas utilisé dans l'application actuelle — le topbar utilise `UiTopbarOffice` avec un breadcrumb, et les onglets dans les headers sont en `CachedUiTab` sur fond blanc. Ce composant reste disponible pour des écrans ou sections à fond sombre.
