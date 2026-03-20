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

## Quand utiliser UiPopup (obligatoire)

**Toute action destructive ou dangereuse DOIT être précédée d'une popup de confirmation.** C'est une règle absolue.

### Actions qui nécessitent une popup

| Action | Variante | Titre suggéré | Icône |
|--------|----------|---------------|-------|
| Supprimer un élément | `negative` | "Supprimer [élément]" | `error` |
| Annuler des modifications non sauvegardées | `negative` | "Annuler les modifications" | `warning` |
| Réinitialiser une configuration | `negative` | "Réinitialiser la configuration" | `warning` |
| Quitter une page avec des changements | `negative` | "Quitter sans sauvegarder ?" | `warning` |
| Révoquer un accès / Désactiver un compte | `negative` | "Révoquer l'accès" | `error` |
| Imposer des réglages à tous les utilisateurs | `positive` | "Imposer les préréglages" | `warning` |
| Publier / Mettre en ligne (action impactante) | `positive` | "Publier [élément]" | `information` |
| Archiver définitivement | `negative` | "Archiver [élément]" | `warning` |

### Pattern standard

```vue
<!-- Le bouton déclenche la popup -->
<UiButton
  label="Supprimer"
  variant="error"
  icon="trash"
  @click="showDeletePopup = true"
/>

<!-- La popup de confirmation -->
<UiPopup
  v-model="showDeletePopup"
  variant="negative"
  title="Supprimer ce bien"
  description="Cette action est irréversible. Toutes les données associées seront supprimées."
  icon-variant="error"
  confirm-label="Supprimer"
  confirm-icon="trash"
  cancel-label="Annuler"
  @confirm="handleDelete"
/>
```

### Règle pour l'annulation de modifications

Quand l'utilisateur clique sur "Annuler" dans une barre de sauvegarde et qu'il y a des modifications non sauvegardées, **toujours afficher une popup** :

```vue
<UiPopup
  v-model="showCancelPopup"
  variant="negative"
  title="Annuler les modifications"
  description="Vos modifications non sauvegardées seront perdues."
  icon-variant="warning"
  confirm-label="Oui, annuler"
  cancel-label="Continuer l'édition"
  @confirm="resetChanges"
/>
```

## Bonnes pratiques

- **Toute action destructive = popup obligatoire.** Pas d'exception.
- **Choisir la variante** : `negative` pour les suppressions ou actions à risque, `positive` pour les confirmations neutres mais impactantes.
- **Labels clairs et contextuels** : Adapter `confirmLabel` et `cancelLabel` au contexte (« Supprimer », « Réinitialiser » plutôt que des labels génériques comme « Confirmer »).
- **Description explicite** : Toujours expliquer la conséquence de l'action dans `description` (« Cette action est irréversible », « Vos modifications seront perdues »).
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
