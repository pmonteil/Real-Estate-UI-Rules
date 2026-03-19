# UiContainersHeader

> Header de page standardisé. Fournit un bouton retour, un titre, et des slots pour l'état, les onglets et les actions. Utilisé en haut de chaque page applicative.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `title` | `string` | `""` | Titre de la page (rendu en H2 : 20px Bold) |
| `size` | `"default" \| "xs"` | `"default"` | Taille du header. `xs` réduit le padding et passe le titre en H3 (16px) |
| `showBackButton` | `boolean` | `true` | Affiche/masque le bouton retour (chevron gauche) |

## Slots

| Slot | Description |
|------|-------------|
| `state` | Zone après le titre pour afficher des infos d'état (pills, badges, tags). Prend le flex restant dans la title-area. |
| `tabs` | Zone centrale pour des onglets (`CachedUiTab`) ou un stepper. `flex-shrink: 0`. |
| `actions` | Zone à droite pour les boutons d'action. **Toujours rendu** (même vide, prend `flex: 1` aligné à droite). |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `back` | — | Émis au clic sur le bouton retour |

## Structure interne

```
┌─ ui-containers-header ──────────────────────────────────────┐
│  [← back]  Titre  [state...]  │  [tabs]  │  [...actions →] │
│  ◄── title-area (flex:1) ──►  │flex-shrink│  ◄── flex:1 ──► │
└─────────────────────────────────────────────────────────────┘
```

- `border-bottom: 1px solid var(--border-default)`
- Padding default : `12px 16px` / xs : `8px 16px`
- Le slot `actions` est **toujours présent** dans le DOM (pas de `v-if`), ce qui garantit que les tabs restent centrés même sans actions.

## Exemples

### Page standard avec onglets et actions

```vue
<UiContainersHeader
  title="Bibliothèque de modèles"
  @back="goBack"
>
  <template #tabs>
    <CachedUiTab v-model="activeTab" :tabs="tabs" />
  </template>
  <template #actions>
    <UiButton label="Créer" variant="primary" icon="plus" @click="create" />
  </template>
</UiContainersHeader>
```

### Page avec état (pill de statut)

```vue
<UiContainersHeader
  :title="itemName"
  @back="goBack"
>
  <template #state>
    <UiPill :label="statusLabel" :color="statusColor" />
  </template>
  <template #actions>
    <UiButton label="Enregistrer" variant="secondary" size="sm" @click="save" />
  </template>
</UiContainersHeader>
```

### Page simple sans actions ni onglets

```vue
<UiContainersHeader
  title="Nouveau modèle"
  size="xs"
  @back="$router.push('/bibliotheque-modeles')"
/>
```

### Builder (variante xs avec state complexe)

```vue
<UiContainersHeader
  :title="templateName"
  size="xs"
  @back="handleBack"
>
  <template #state>
    <span class="tag">Brouillon</span>
  </template>
  <template #actions>
    <UiButton label="Aperçu" variant="secondary" size="sm" />
    <UiButton label="Publier" variant="primary" size="sm" />
  </template>
</UiContainersHeader>
```

## Notes d'intégration

- **Toujours utiliser** ce composant pour les headers de page plutôt qu'un header custom.
- Le header doit être rendu **sticky** via CSS sur le parent : `position: sticky; top: 44px; z-index: 50;`. Il reste **fixe visuellement** quand l'utilisateur scroll la page (juste en-dessous de la topbar).
- Le composant ne gère pas le sticky lui-même — c'est à la page de l'appliquer via une classe.
- **Max-width interne** : Le fond du header s'étire sur toute la largeur disponible (100% de la zone contenu), mais son **contenu interne** (titre, tabs, actions) doit être contraint à `max-width: 1400px` et centré horizontalement, cohérent avec le container de page. Cela se fait en wrappant le contenu interne ou en appliquant le même pattern que le page-container :

```scss
.page-header-wrapper {
  position: sticky;
  top: 44px;
  z-index: 50;
  background: var(--surface-default);
  border-bottom: 1px solid var(--border-default);
}

.page-header-wrapper :deep(.ui-containers-header__content) {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 2rem;
}
```

- En variante `xs`, le titre passe en H3 (16px Semi Bold). Utilisé pour les sous-pages / builders.
- Le slot `actions` est toujours dans le DOM pour maintenir l'équilibre du layout flex même quand il est vide.
