# UiTag

> Tag sélectionnable ou désélectionnable pour les filtres, catégories ou choix multiples. Affiche une icône « plus » quand non sélectionné, « croix » quand sélectionné (pour retirer).

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `label` | `string` | — | Texte du tag (obligatoire) |
| `selected` | `boolean` | `false` | État de sélection |
| `disabled` | `boolean` | `false` | Désactive le tag (non cliquable) |

## Slots

Aucun slot.

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `click` | `MouseEvent` | Émis au clic |
| `toggle` | `selected: boolean` | Émis au clic avec la nouvelle valeur de sélection |

## Exemples

### Usage basique

```vue
<UiTag label="Appartement" />
<UiTag label="Maison" :selected="true" />
<UiTag label="Studio" disabled />
```

### Avec v-model (si le composant supporte update:selected)

```vue
<UiTag v-model:selected="tagSelected" label="Appartement" />

<!-- Équivalent manuel : -->
<UiTag
  :selected="tagSelected"
  label="Appartement"
  @toggle="tagSelected = $event"
/>
```

### Liste de tags pour filtres

```vue
<div class="tag-list">
  <UiTag v-model:selected="filters.appartement" label="Appartement" />
  <UiTag v-model:selected="filters.maison" label="Maison" />
  <UiTag v-model:selected="filters.villa" label="Villa" />
  <UiTag label="Terrain" />
  <UiTag label="Local commercial" />
</div>
```

## Bonnes pratiques

- Utiliser UiTag pour des filtres à plusieurs choix (type de bien, catégories, etc.).
- Gérer `toggle` pour mettre à jour l’état (sélection/désélection).
- Pour des tags en lecture seule ou non cliquables, utiliser plutôt **UiPill**.

## À ne pas faire

- Ne pas utiliser UiTag pour des statuts ou labels en lecture seule — utiliser **UiPill**.
- Éviter des libellés trop longs qui déforment le tag.
- Ne pas oublier de réagir à `toggle` ou `click` si le tag doit changer d’état.

## Notes d'intégration

- L’icône change automatiquement : `plus` quand non sélectionné, `x` quand sélectionné.
- Animation « shake » au survol quand le tag est sélectionné pour indiquer qu’on peut le retirer.
- Bordure pointillée quand non sélectionné, bordure pleine quand sélectionné.
- Au survol en état sélectionné, la bordure et l’icône passent en rouge (retrait).
