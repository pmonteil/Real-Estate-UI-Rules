# UiMenuOffice

> Menu latéral principal de l'application (sidebar). Navigation hiérarchique avec items, sous-menus (flyout), badges de notification, mode plié/déplié et bloc utilisateur en footer.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `items` | `MenuItem[]` | — | **Requis.** Liste des items du menu |
| `modelValue` | `string \| number` | — | Valeur de l'item actuellement actif (v-model) |
| `collapsed` | `boolean` | `false` | Affiche le menu en mode plié (icônes seules) |
| `showToggle` | `boolean` | `true` | Affiche le bouton pour plier/déplier |
| `user` | `UserInfo \| undefined` | — | Infos utilisateur pour le bloc footer (nom, rôle, avatar) |

## Format MenuItem

```typescript
interface MenuItem {
  label: string;
  icon?: string;           // Nom Tabler (ex: "tabler:search", "tabler:mail")
  value?: string | number;  // Identifiant unique (optionnel, sinon index)
  badge?: number | string; // Badge de notification
  disabled?: boolean;
  children?: MenuItem[];   // Sous-menu (flyout au survol)
}
```

## Format UserInfo

```typescript
interface UserInfo {
  name: string;
  role?: string;
  avatar?: string;  // URL de l'avatar, sinon initiales dérivées du name
}
```

## Slots

| Slot | Description |
|------|-------------|
| — | Aucun slot. Tout est configuré via les props. |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `string \| number` | Émis lors du clic sur un item ou un enfant de sous-menu |
| `toggle` | — | Émis au clic sur le bouton plier/déplier |
| `user-action` | `'profile' \| 'logout'` | Émis lors d'une action utilisateur (Modifier le profil, Se déconnecter) |

## Exemples

### Usage basique (MainLayout)

```vue
<script setup lang="ts">
import { ref, computed } from 'vue';
import { UiMenuOffice } from 'septeo-real-estate-ui';
import type { MenuItem, UserInfo } from 'septeo-real-estate-ui';

const menuCollapsed = ref(false);
const activeMenuItem = computed(() => route.path === '/campagnes' ? 'campagnes' : 'modelo-intouch');

const menuItems: MenuItem[] = [
  { label: 'Modelo InTouch', icon: 'tabler:send', value: 'modelo-intouch-parent', badge: 5, children: [
    { label: "Vue d'ensemble", icon: 'tabler:layout-dashboard', value: 'modelo-intouch' },
    { label: 'Campagnes', icon: 'tabler:mail', value: 'campagnes' },
    { label: 'Automatisations', icon: 'tabler:robot', value: 'automatisations' },
  ]},
  { label: 'Mes widgets', icon: 'tabler:layout-grid', value: 'mes-widgets-parent', children: [
    { label: 'Estimation', icon: 'tabler:calculator', value: 'mes-widgets-estimation' },
  ]},
];

const userInfo: UserInfo = {
  name: 'Philippe Martin',
  role: 'Administrateur',
  avatar: undefined,
};
</script>

<template>
  <UiMenuOffice
    v-model="activeMenuItem"
    :items="menuItems"
    :collapsed="menuCollapsed"
    :user="userInfo"
    @update:model-value="handleMenuItemClick"
    @toggle="menuCollapsed = !menuCollapsed"
    @user-action="handleUserAction"
  />
</template>
```

### Item simple (sans sous-menu)

```vue
{
  label: 'E-mail',
  icon: 'tabler:mail',
  value: 'email',
}
```

### Item avec sous-menu

```vue
{
  label: 'Biens',
  icon: 'tabler:home',
  value: 'biens',
  children: [
    { label: "Vue d'ensemble", icon: 'tabler:layout-dashboard', value: 'biens-vue-ensemble' },
    { label: 'Biens immobiliers', icon: 'tabler:building', value: 'biens-immobiliers' },
  ],
}
```

### Item avec badge

```vue
{
  label: 'Modelo InTouch',
  icon: 'tabler:send',
  value: 'modelo-intouch-parent',
  badge: 5,
}
```

## Bonnes pratiques

- **Le menu doit toujours pouvoir se replier/déplier.** Le bouton toggle (icône chevron sur le bord droit du menu) doit être fonctionnel et accessible. Toujours brancher l'événement `@toggle` pour basculer l'état `collapsed`, et s'assurer que le layout (contenu, header, barre de sauvegarde) s'adapte dynamiquement à la largeur du menu (206px déplié → 60px plié).

```vue
<UiMenuOffice
  v-model="activeMenuItem"
  :items="menuItems"
  :collapsed="menuCollapsed"
  @toggle="menuCollapsed = !menuCollapsed"
/>
```

```scss
.main-content {
  margin-left: 206px; // 60px si menuCollapsed
  transition: margin-left 0.2s ease;
}
```

- Utiliser des `value` cohérents avec les routes pour synchroniser l'item actif avec `useRoute().path`.
- Les items avec `children` n'émettent pas directement — la sélection se fait sur les enfants (flyout).
- En mode plié, les labels des items simples s'affichent en tooltip au survol.
- Préfixer les icônes avec `tabler:` (ex: `tabler:mail`) pour la résolution Tabler.

## À ne pas faire

- **Ne jamais oublier de brancher `@toggle`** sur le menu. Le bouton de repli doit toujours être fonctionnel — un menu qui ne se replie pas est un bug.
- Ne pas oublier de gérer `@update:model-value` pour la navigation (ex: `router.push()`).
- Ne pas utiliser des icônes non disponibles dans `@tabler/icons-vue`.
- Ne pas placer le menu dans un conteneur avec overflow hidden qui couperait les flyouts.

## Notes d'intégration

- Le menu a une largeur fixe : 206px (déplié) / 60px (plié).
- **Position fixed obligatoire** : le menu est en `position: fixed; top: 44px; left: 0; bottom: 0; z-index: 1100;`. Il **ne scroll pas** avec la page — il reste fixe en permanence sur toute la hauteur de la fenêtre (sous la topbar).

```scss
.sidebar {
  position: fixed;
  top: 44px;    // Sous la topbar
  left: 0;
  bottom: 0;
  width: 206px; // 60px si collapsed
  z-index: 1100;
  overflow-y: auto; // Scroll interne si le menu est plus long que la fenêtre
}
```

- Fond `#2E3862` avec lueurs animées (glow) décoratives.
- Le bouton toggle est positionné en absolu sur le bord droit (`right: -12px`).
- Les sous-menus s'affichent en flyout à droite de l'item au survol. En mode plié, un header avec le label du parent apparaît dans le flyout.
- Le z-index du flyout est 100. S'assurer qu'aucun overlay ne le masque.
