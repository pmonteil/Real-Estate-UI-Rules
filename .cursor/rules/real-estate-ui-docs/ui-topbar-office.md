# UiTopbarOffice

> Barre de navigation globale en haut de l'écran. Affiche un fil d'Ariane (breadcrumb) à gauche et des actions (notifications par défaut) à droite. Fond sombre aligné avec le menu Office.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `breadcrumb` | `BreadcrumbItem[]` | — | **Requis.** Items du fil d'Ariane |
| `notificationCount` | `number` | `0` | Nombre affiché sur le badge de notifications. Si > 0, un badge s'affiche sur l'icône cloche |

## Format BreadcrumbItem

```typescript
interface BreadcrumbItem {
  label: string;
  path?: string;        // Optionnel, pour la navigation
  value?: string | number; // Identifiant pour gérer le clic
}
```

## Slots

| Slot | Description |
|------|-------------|
| `actions` | Zone à droite pour remplacer ou compléter les actions. Si fourni, remplace le bouton de notification par défaut |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `navigate` | `(item: BreadcrumbItem, index: number)` | Émis au clic sur un item du breadcrumb (sauf le dernier, qui est le niveau actuel) |
| `notification-click` | — | Émis au clic sur le bouton notifications |

## Exemples

### Usage basique (MainLayout)

```vue
<script setup lang="ts">
import { computed } from 'vue';
import { UiTopbarOffice } from 'septeo-real-estate-ui';
import type { BreadcrumbItem } from 'septeo-real-estate-ui';

const breadcrumb = computed<BreadcrumbItem[]>(() => {
  const path = route.path;
  if (path === '/') return [{ label: 'Accueil', value: 'home' }];
  if (path.includes('/biens/immobiliers')) {
    return [
      { label: 'Biens', value: 'biens' },
      { label: 'Biens immobiliers', value: 'biens-immobiliers' },
    ];
  }
  return [{ label: 'Modelo InTouch', value: 'modelo-intouch' }];
});

function handleBreadcrumbNavigate(item: BreadcrumbItem, index: number) {
  if (item.value === 'biens') router.push('/biens');
  else if (item.value === 'biens-immobiliers') router.push('/biens/immobiliers');
}
</script>

<template>
  <UiTopbarOffice
    :breadcrumb="breadcrumb"
    :notification-count="notificationCount"
    @navigate="handleBreadcrumbNavigate"
    @notification-click="handleNotificationClick"
  />
</template>
```

### Avec slot actions personnalisé

```vue
<UiTopbarOffice :breadcrumb="breadcrumb">
  <template #actions>
    <UiButton label="Action" variant="ghost" size="sm" />
    <button class="notification-btn" @click="openNotifications">
      <IconBell />
      <span v-if="count > 0" class="badge">{{ count }}</span>
    </button>
  </template>
</UiTopbarOffice>
```

### Breadcrumb dynamique selon la route

```vue
const breadcrumb = computed(() => {
  const items = [{ label: 'Modelo InTouch', value: 'modelo-intouch' }];
  if (route.path.startsWith('/campagnes')) items.push({ label: 'Campagnes', value: 'campagnes' });
  if (route.path.includes('/automatisations')) items.push({ label: 'Automatisations', value: 'automatisations' });
  return items;
});
```

## Bonnes pratiques

- Toujours fournir un breadcrumb cohérent avec la hiérarchie de navigation.
- Le dernier item est affiché comme niveau actuel (non cliquable).
- Gérer `@navigate` pour naviguer via `router.push()` selon `item.value` ou `item.path`.
- Positionner la topbar en `sticky` ou `fixed` avec `z-index` élevé (ex: 1200).

## À ne pas faire

- Ne pas laisser un breadcrumb vide — fournir au moins `[{ label: 'Accueil' }]`.
- Ne pas oublier de gérer la navigation pour les items cliquables.

## Notes d'intégration

- Hauteur fixe : 44px. Fond `#2E3862` (aligné avec le menu Office).
- **Position fixed obligatoire** : la topbar est en `position: fixed; top: 0; left: 0; right: 0; z-index: 1200;`. Elle **ne scroll pas** — elle reste toujours visible tout en haut de l'écran.

```scss
.topbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 44px;
  z-index: 1200;
}
```

- Le breadcrumb intégré n’utilise **pas** le composant `UiBreadcrumb` séparé — c’est une implémentation interne propre à la topbar.
- Lueurs animées (glow) décoratives comme sur `UiMenuOffice`.
- Si `notificationCount > 99`, le badge affiche `"99+"`.
- Le slot `actions` remplace entièrement le bouton de notification par défaut quand il est fourni.
