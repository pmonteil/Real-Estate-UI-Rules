# UiBreadcrumb

> Composant de fil d'Ariane pour la navigation hiérarchique. Chaque item est un `UiLink` avec séparateur configurable. Utilisable sur fond clair (contrairement au breadcrumb intégré de `UiTopbarOffice`).

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `items` | `BreadcrumbItem[]` | — | **Requis.** Liste des items du fil d'Ariane |
| `separator` | `string` | `"chevron-right"` | Nom d'icône Tabler pour le séparateur (kebab-case, ex: `chevron-right`, `slash`) |

## Format BreadcrumbItem

```typescript
interface BreadcrumbItem {
  label: string;
  href?: string;       // URL pour lien <a>
  to?: string | object; // Route pour vue-router (router-link)
  iconLeft?: string;   // Icône à gauche du label
  iconRight?: string;  // Icône à droite
  disabled?: boolean;
}
```

> **Note :** Si ni `href` ni `to` n'est fourni, l'item est rendu comme texte (niveau actuel). Le premier item reçoit `active: true` par défaut.

## Slots

| Slot | Description |
|------|-------------|
| — | Aucun slot. Contenu géré via la prop `items`. |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `click` | `(item: BreadcrumbItem, index: number)` | Émis au clic sur un item (avant la navigation native si href/to) |

## Exemples

### Usage basique

```vue
<script setup lang="ts">
import { UiBreadcrumb } from 'septeo-real-estate-ui';

const breadcrumbItems = [
  { label: 'Accueil', iconLeft: 'home', href: '/' },
  { label: 'Biens immobiliers', href: '/biens' },
  { label: 'Appartements', href: '/biens/appartements' },
  { label: 'Paris 16ème' }, // Dernier = niveau actuel (sans lien)
];
</script>

<template>
  <UiBreadcrumb :items="breadcrumbItems" />
</template>
```

### Avec vue-router (to)

```vue
const items = [
  { label: 'Accueil', to: '/' },
  { label: 'Biens', to: '/biens' },
  { label: 'Détail' },
];
```

### Avec séparateur personnalisé

```vue
<UiBreadcrumb :items="items" separator="slash" />
```

### Avec gestion du clic

```vue
<UiBreadcrumb :items="items" @click="onBreadcrumbClick" />

function onBreadcrumbClick(item: BreadcrumbItem, index: number) {
  console.log('Clic sur', item.label, 'à l\'index', index);
}
```

## Bonnes pratiques

- Le dernier item représente généralement la page courante — ne pas lui donner `href`/`to` pour qu'il soit visuellement distinct (non cliquable).
- Utiliser `UiBreadcrumb` sur les pages à fond clair ; pour la topbar sombre, le breadcrumb est intégré dans `UiTopbarOffice`.
- Préférer `to` avec vue-router pour une navigation SPA sans rechargement.

## À ne pas faire

- Ne pas laisser un tableau `items` vide.
- Ne pas utiliser `UiBreadcrumb` dans la topbar Office — celle-ci a son propre breadcrumb.

## Notes d'intégration

- Chaque item est rendu via `UiLink`. Les props `iconLeft`, `iconRight`, `href`, `to`, `disabled` sont transmises.
- Le premier item reçoit `active: true` automatiquement (style de lien actif).
- Le séparateur utilise une icône Tabler — noms kebab-case convertis en PascalCase (ex: `chevron-right` → `IconChevronRight`).
- Le composant est utilisé dans la page Index (showcase). Pour la navigation réelle de l'app, le breadcrumb est géré par `UiTopbarOffice`.
