# UiTab

> Composant d'onglets horizontaux pour la navigation au niveau d'une page. Affiche une liste d'onglets avec support des icônes, état actif et désactivé.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `tabs` | `Tab[]` | — | **Requis.** Tableau d'objets onglet. Format : `{ value, label, icon?, disabled? }` |
| `modelValue` | `string \| number` | — | **Requis.** Valeur de l'onglet actuellement actif (v-model) |
| `size` | `"default" \| "xs"` | `"default"` | Taille des onglets. `xs` réduit le padding et la taille de police |

## Format Tab

```typescript
interface Tab {
  label: string;        // Libellé affiché
  value: string | number; // Identifiant unique (utilisé pour v-model)
  icon?: string;        // Nom d'icône Tabler (ex: "layout-dashboard", "mail") — kebab-case, sans préfixe tabler:
  disabled?: boolean;   // Désactive l'onglet
}
```

## Slots

| Slot | Description |
|------|-------------|
| — | Aucun slot. Contenu géré via les props `tabs`. |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `string \| number` | Émis au clic sur un onglet (sauf si disabled). La nouvelle valeur correspond à `tab.value`. |

## CachedUiTab — Wrapper utilisé dans l'app

> **Important :** Dans l'application, `CachedUiTab` est le composant principal utilisé. C'est un wrapper local (`src/components/CachedUiTab.vue`) qui pré-charge les icônes Tabler pour éviter le lazy-load à chaque changement d'onglet. Il expose la même interface que `UiTab` (props `tabs`, `modelValue`, événement `update:modelValue`).

**Différences avec UiTab :**
- Utilise un cache statique d'icônes (`layout-dashboard`, `mail`, `template`, `users`, `settings`, etc.) — seules les icônes présentes dans le cache sont affichées.
- Support optionnel de `badge` sur les onglets : `{ value, label, icon?, badge?: number | string }`.

```vue
<CachedUiTab v-model="activeTab" :tabs="tabs" />
```

## Exemples

### Usage basique

```vue
<script setup lang="ts">
import { ref } from 'vue';
import { UiTab } from 'septeo-real-estate-ui';

const activeTab = ref('suivi');

const tabs = [
  { value: 'suivi', label: 'Suivi des campagnes', icon: 'mail' },
  { value: 'statistiques', label: 'Statistiques', icon: 'chart-line' },
];
</script>

<template>
  <UiTab v-model="activeTab" :tabs="tabs" />
  <div>Contenu de l'onglet : {{ activeTab }}</div>
</template>
```

### Avec variante xs

```vue
<UiTab v-model="activeTab" :tabs="tabs" size="xs" />
```

### Avec onglet désactivé

```vue
const tabs = [
  { value: 'a', label: 'Onglet A' },
  { value: 'b', label: 'Onglet B', disabled: true },
  { value: 'c', label: 'Onglet C' },
];
```

### Dans le header (pattern recommandé)

```vue
<UiContainersHeader
  title="Bibliothèque de modèles"
  @back="goBack"
>
  <template #tabs>
    <CachedUiTab v-model="activeTab" :tabs="navigationTabs" />
  </template>
  <template #actions>
    <UiButton label="Créer" variant="primary" />
  </template>
</UiContainersHeader>
```

### Avec badge (CachedUiTab)

```vue
const tabs = [
  { value: 'automatisations', label: 'Automatisations', icon: 'robot', badge: 102 },
  { value: 'suivi', label: 'Suivi des automatisations', icon: 'clock' },
];
```

## Bonnes pratiques

- Utiliser `CachedUiTab` pour les headers de page afin de profiter du cache d'icônes et d'éviter le flicker au changement d'onglet.
- Placer les onglets dans le slot `#tabs` de `UiContainersHeader` pour une navigation de page cohérente.
- Utiliser des `value` stables (ex: `'suivi'`, `'statistiques'`) pour synchroniser avec la navigation ou l'état global (ex: stores).
- Les icônes doivent être au format kebab-case (`layout-dashboard`, `chart-line`). Le composant les convertit en PascalCase pour `@tabler/icons-vue`.

## À ne pas faire

- Ne pas utiliser des icônes non supportées par le cache dans `CachedUiTab` sans les ajouter au cache local.
- Ne pas utiliser `UiTab` pour la navigation latérale — préférer `UiMenuOffice`.
- Ne pas oublier de lier `modelValue` à un état réactif (ref/computed) pour que la sélection soit persistée.

## Notes d'intégration

- Le composant utilise `@tabler/icons-vue` pour les icônes. Les noms sont convertis automatiquement (ex: `layout-dashboard` → `IconLayoutDashboard`).
- En variante `xs`, la police passe en `body-small` (12px).
- L'onglet actif a un fond en dégradé (`--gradient-menu-active`) et une ombre portée. Animation légère au clic (`tab-pop`).
- `UiTab` est idéal pour les fonds clairs. Pour les fonds sombres (topbar, sidebar dark), utiliser `UiTabDark`.
