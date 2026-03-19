# UiPopupIcon

> Icône de statut affichée dans un cercle coloré. Utilisée principalement à l’intérieur de `UiPopup` pour indiquer le type de message (information, erreur, avertissement, neutre).

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `variant` | `"information" \| "error" \| "warning" \| "neutral"` | `"information"` | Variante visuelle (couleur de fond et icône) |

## Slots

| Slot | Description |
|------|-------------|
| — | Aucun slot. L’icône est déterminée par `variant`. |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| — | — | Aucun événement émis. Composant purement visuel. |

## Exemples

### Usage basique (intégré dans UiPopup)

```vue
<!-- UiPopup utilise UiPopupIcon en interne via showIcon -->
<UiPopup
  v-model="showModal"
  title="Information"
  description="Message."
  :show-icon="true"
  icon-variant="information"
/>
```

### Usage standalone (rare)

```vue
<template>
  <div class="my-card">
    <UiPopupIcon variant="warning" />
    <p>Attention : action requise</p>
  </div>
</template>

<script setup lang="ts">
import { UiPopupIcon } from 'septeo-real-estate-ui';
</script>
```

### Variantes

```vue
<!-- Information (bleu) -->
<UiPopupIcon variant="information" />

<!-- Erreur (rouge) -->
<UiPopupIcon variant="error" />

<!-- Avertissement (orange) -->
<UiPopupIcon variant="warning" />

<!-- Neutre (gris) -->
<UiPopupIcon variant="neutral" />
```

## Bonnes pratiques

- **Utilisation principale** : Laisser `UiPopup` gérer l’icône via `showIcon` et `iconVariant` ; ne pas placer manuellement `UiPopupIcon` sauf cas très spécifique.
- **Cohérence sémantique** : 
  - `information` → messages neutres ou explicatifs
  - `error` → erreurs, suppressions, échecs
  - `warning` → avertissements, actions à confirmer
  - `neutral` → contenu informatif sans gravité
- **Taille fixe** : L’icône a une taille fixe (48×48px avec icône 20px). Ne pas la redimensionner pour garder la cohérence du DS.

## À ne pas faire

- **Ne pas l’utiliser pour des tooltips au survol** : Pour de l’aide contextuelle, préférer un composant de type tooltip ou `UiPopupIcon` dans un `UiPopup` ouvert au clic.
- **Ne pas le confondre avec les icônes de statut de `UiSnackbar`** : Chaque composant a son propre mapping d’icônes ; ne pas réutiliser `UiPopupIcon` comme statut de snackbar.
- **Ne pas surcharger en styles** : Le composant est déjà stylé par le DS ; éviter les overrides sauf besoin très précis.

## Notes d'intégration

- Icônes Tabler : `info-circle`, `exclamation-circle`, `alert-triangle`, `alien` (neutral).
- Mapping des variantes : `information` → fond `--surface-light-action`, `error` → `--surface-error`, `warning` → `--surface-warning`, `neutral` → `--surface-default-bis`.
- Pas d’interaction : pas de clic, hover ou événement. Pour une action au clic, l’utiliser dans un conteneur cliquable (ex. `UiPopup`).
