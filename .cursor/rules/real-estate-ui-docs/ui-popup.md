# UiPopup

> Modal centré pour les confirmations, avertissements ou demandes d'action. Overlay avec fond semi-transparent, boutons Retour/Confirmer et option de fermeture par clic sur l'overlay.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `modelValue` | `boolean` | `false` | Contrôle la visibilité via v-model |
| `variant` | `"positive" \| "negative"` | `"positive"` | Variante globale : positive (bleu) ou negative (rouge) |
| `title` | `string` | `""` | Titre du popup |
| `description` | `string` | `""` | Texte descriptif sous le titre |
| `iconVariant` | `"information" \| "error" \| "warning" \| "neutral"` | dérivé de `variant` | Variante de l'icône affichée en haut |
| `showIcon` | `boolean` | `true` | Affiche l'icône dans le contenu |
| `showCloseButton` | `boolean` | `true` | Affiche le bouton X en haut à droite |
| `cancelLabel` | `string` | `"Retour"` | Label du bouton Annuler. Vide = masqué |
| `confirmLabel` | `string` | `"Je confirme"` | Label du bouton Confirmer. Vide = masqué |
| `confirmIcon` | `string` | `"check"` | Nom de l'icône du bouton Confirmer |
| `closeOnOverlay` | `boolean` | `true` | Ferme au clic sur l'overlay (hors contenu) |
| `persistent` | `boolean` | `false` | Si true, ne ferme pas au confirm (permet plusieurs actions) |

## Slots

| Slot | Description |
|------|-------------|
| défaut | Contenu additionnel inséré après `description` dans la zone texte |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `(value: boolean)` | Synchronise la visibilité avec v-model |
| `confirm` | — | Émis au clic sur Confirmer |
| `cancel` | — | Émis au clic sur Annuler ou Fermer |
| `close` | — | Émis lorsque le popup se ferme |

## Exemples

### Usage basique (confirmation positive)

```vue
<template>
  <UiPopup
    v-model="showModal"
    title="Confirmer l'action"
    description="Voulez-vous vraiment continuer ?"
    confirm-label="Confirmer"
    cancel-label="Annuler"
    @confirm="handleConfirm"
    @cancel="handleCancel"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { UiPopup } from 'septeo-real-estate-ui';

const showModal = ref(false);

function handleConfirm() {
  // logique
}
function handleCancel() {
  showModal.value = false;
}
</script>
```

### Variante négative (suppression, erreur)

```vue
<UiPopup
  v-model="showDeleteModal"
  variant="negative"
  title="Supprimer l'élément"
  description="Cette action est irréversible."
  confirm-label="Supprimer"
  cancel-label="Annuler"
  @confirm="deleteItem"
/>
```

### Avec icône d'avertissement personnalisée

```vue
<UiPopup
  v-model="showForceModal"
  title="Imposer les préréglages"
  description="Cette action imposera ces préréglages à tous les futurs modèles créés par les utilisateurs."
  icon-variant="warning"
  confirm-label="Confirmer"
  cancel-label="Annuler"
  @confirm="confirmForcePresets"
  @cancel="cancelForcePresets"
/>
```

### Avec contenu additionnel (slot)

```vue
<UiPopup
  v-model="showModal"
  title="Détails"
  description="Informations complémentaires :"
>
  <ul>
    <li>Point 1</li>
    <li>Point 2</li>
  </ul>
</UiPopup>
```

### Méthodes exposées (open/close)

```vue
<template>
  <UiPopup ref="popupRef" title="Modal" @confirm="doSomething" />
</template>

<script setup lang="ts">
import { ref } from 'vue';

const popupRef = ref();

function openModal() {
  popupRef.value?.open();
}
</script>
```

## Bonnes pratiques

- **Utiliser pour les confirmations critiques** : Suppression, action irréversible, changement impactant plusieurs utilisateurs.
- **Choisir la variante** : `positive` pour les confirmations neutres, `negative` pour les suppressions ou actions à risque.
- **Labels clairs** : Adapter `confirmLabel` et `cancelLabel` au contexte (« Supprimer », « Annuler » plutôt que des labels génériques).
- **Ne pas bloquer sans issue** : Toujours proposer un moyen de fermer (bouton Annuler ou clic overlay) sauf si `persistent` est requis.
- **Téléport** : Le composant utilise déjà `Teleport to="body"` pour éviter les problèmes de z-index.

## À ne pas faire

- **Ne pas remplacer par un snackbar** : Les confirmations importantes doivent passer par `UiPopup`, pas `UiSnackbar`.
- **Ne pas hardcoder le positionnement** : Le popup est centré ; ne pas tenter de le repositionner manuellement.
- **Ne pas utiliser `UiPopupIcon` seul** : C'est un composant interne au popup ; utiliser `UiPopup` pour l'affichage modal.
- **Ne pas oublier de gérer `@confirm`** : Si `persistent` est false, le popup se ferme au confirm, mais la logique métier doit être dans le handler.

## Notes d'intégration

- Dépend de `UiPopupIcon` et `UiButton` (composants internes du DS).
- `z-index: 1000` pour l'overlay. Vérifier la cohérence avec les autres couches (drawer, topbar).
- Transitions : `ui-popup-fade` pour l'overlay, `ui-popup-scale` pour le contenu.
- `max-width: 430px` pour le panneau de contenu.
