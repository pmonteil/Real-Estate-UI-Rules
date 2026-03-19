# UiSnackbar

> Composant de notification toast pour afficher des messages de feedback à l'utilisateur (succès, erreur, avertissement, information) avec auto-fermeture et barre de progression.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `title` | `string` | `"Notification"` | Titre du message affiché |
| `message` | `string` | `""` | Contenu du message |
| `status` | `"default" \| "success" \| "error" \| "warning"` | `"default"` | Variante visuelle (icône et couleurs) |
| `duration` | `number` | `5000` | Durée d'affichage en ms avant fermeture automatique |
| `showProgress` | `boolean` | `true` | Affiche la barre de progression du temps restant |
| `linkText` | `string` | `""` | Texte du lien d'action optionnel (ex: "Réessayer", "En savoir plus") |
| `modelValue` | `boolean` | `true` | Contrôle la visibilité via v-model |

## Slots

| Slot | Description |
|------|-------------|
| — | Aucun slot. Le contenu est déterminé par les props `title`, `message` et `linkText`. |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `(value: boolean)` | Émis à la fermeture (false) pour synchroniser v-model |
| `close` | — | Émis lorsque l'utilisateur ferme manuellement ou à la fin du timer |
| `link-click` | — | Émis lors du clic sur le lien d'action (`linkText`) |

## Exemples

### Usage basique

```vue
<template>
  <UiSnackbar
    v-model="showSnackbar"
    title="Succès"
    message="L'opération a été effectuée avec succès."
    status="success"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { UiSnackbar } from 'septeo-real-estate-ui';

const showSnackbar = ref(false);

function save() {
  // ... logique de sauvegarde
  showSnackbar.value = true;
}
</script>
```

### Avec variantes

```vue
<!-- Information (bleu) -->
<UiSnackbar
  v-model="showDefaultSnackbar"
  title="Information"
  message="Ceci est un message d'information."
  status="default"
  link-text="En savoir plus"
  @link-click="handleLinkClick"
/>

<!-- Succès (vert) -->
<UiSnackbar
  v-model="showSuccessSnackbar"
  title="Succès"
  message="L'opération a été effectuée avec succès."
  status="success"
/>

<!-- Erreur (rouge) -->
<UiSnackbar
  v-model="showErrorSnackbar"
  title="Erreur"
  message="Une erreur s'est produite. Veuillez réessayer."
  status="error"
  link-text="Réessayer"
  @link-click="retryAction"
/>

<!-- Avertissement (orange) -->
<UiSnackbar
  v-model="showWarningSnackbar"
  title="Attention"
  message="Cette action nécessite votre attention."
  status="warning"
/>
```

### Pattern programmatique (plusieurs snackbars)

```vue
<template>
  <div>
    <UiButton label="Sauvegarder" @click="save" />
    <UiSnackbar v-model="snackbars.success" title="Succès" message="Enregistré." status="success" />
    <UiSnackbar v-model="snackbars.error" title="Erreur" message="Échec." status="error" />
  </div>
</template>

<script setup lang="ts">
const snackbars = reactive({
  success: false,
  error: false,
});

function save() {
  try {
    // ... sauvegarde
    snackbars.success = true;
  } catch {
    snackbars.error = true;
  }
}
</script>
```

### Désactiver la barre de progression

```vue
<UiSnackbar
  v-model="showSnackbar"
  title="Message permanent"
  message="L'utilisateur doit fermer manuellement."
  status="default"
  :show-progress="false"
  :duration="0"
/>
```

## Bonnes pratiques

- **Un snackbar par type de message** : Pour gérer plusieurs statuts, utiliser plusieurs instances avec des refs distinctes plutôt qu'une seule instance dynamique.
- **Position** : Le composant gère sa propre largeur (380px). Le placer dans un conteneur positionné (ex: coin supérieur droit, via `position: fixed` + `top`/`right`) pour le positionnement global.
- **Durée adaptée** : `default` 5000ms convient à la plupart des cas ; réduire pour les erreurs critiques si l'utilisateur doit agir rapidement.
- **Lien d'action** : Utiliser `linkText` + `@link-click` pour des actions contextuelles (réessayer, voir les détails) plutôt que des messages purement informatifs.
- **Feedback immédiat** : Afficher le snackbar dès la fin de l'action (succès/erreur) pour un retour clair à l'utilisateur.

## À ne pas faire

- **Ne pas utiliser de `<div>` ou toast custom** : Toujours préférer `UiSnackbar` pour la cohérence visuelle et les variantes du DS.
- **Ne pas oublier le v-model** : Sans `v-model`, le composant reste affiché indéfiniment (sauf si `duration` est 0 et fermeture manuelle).
- **Ne pas empiler plusieurs snackbars au même endroit sans gestion** : Prévoir un conteneur ou une logique pour éviter le chevauchement.
- **Ne pas utiliser pour des confirmations critiques** : Pour les suppressions ou actions irréversibles, utiliser `UiPopup` (modal) plutôt qu'un snackbar.

## Notes d'intégration

- Le composant utilise `Transition name="snackbar"` pour les animations d'entrée/sortie.
- Les icônes (info-circle, circle-check, alert-circle, alert-triangle) sont chargées dynamiquement depuis `@tabler/icons-vue`.
- Largeur fixe : 380px. Les textes longs s'adaptent via `min-width: 0` et `flex`.
- Pour un conteneur global (ex: `App.vue`), utiliser un `Teleport` ou un conteneur fixe en haut à droite de l'écran.
