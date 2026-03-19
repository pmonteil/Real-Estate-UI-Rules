# UiBadge

> Badge numérique ou indicateur pour afficher des comptages (notifications, quantités). Ne contient pas de libellé texte — pour des étiquettes colorées, utiliser **UiPill**.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `value` | `number \| string` | `0` | Valeur affichée (nombre ou chaîne) |
| `max` | `number` | `99` | Valeur maximale affichée ; au-delà on affiche `{max}+` |
| `type` | `'notifications' \| 'quantity'` | `'notifications'` | Style du badge (fond orange vs gris) |
| `selected` | `boolean` | `false` | État sélectionné (inverse les couleurs pour rester lisible sur fond accent) |
| `size` | `'default' \| 'xs'` | `'default'` | Taille du badge |

## Slots

Aucun slot.

## Événements

Aucun événement.

## Exemples

### Usage basique (notifications)

```vue
<UiBadge :value="5" />
<UiBadge :value="150" :max="99" />
```

### Type quantity (compteur neutre)

```vue
<UiBadge :value="12" type="quantity" />
<UiBadge :value="3" type="quantity" :selected="isFilterActive" />
```

### Taille xs (point discret)

```vue
<UiBadge :value="1" size="xs" type="notifications" />
```

### Dans UiFilter (affichage du comptage)

```vue
<UiFilter
  label="Statut"
  icon="circle-check"
  :count="selectedCount"
  :active="!!filters.statut"
  :show-badge="true"
  @click="toggleDropdown('statut')"
/>
```

## Bonnes pratiques

- Utiliser `type="notifications"` pour les alertes (couleur accent/orange).
- Utiliser `type="quantity"` pour des compteurs neutres (ex. nombre de résultats).
- Utiliser `selected` quand le badge est sur un fond accent (ex. UiFilter actif) pour garder un bon contraste.
- Définir `max` pour éviter des nombres trop longs (ex. `99+` au lieu de `999`).

## À ne pas faire

- Ne pas utiliser UiBadge pour des libellés textuels (« Email », « En cours », etc.) — utiliser **UiPill**.
- Éviter d’utiliser UiBadge pour des statuts colorés (vert/rouge/bleu) — utiliser **UiPill** avec `color`.
- Ne pas oublier de définir `max` pour les compteurs qui peuvent dépasser 99.

## Notes d'intégration

- **UiFilter** : UiBadge est utilisé en interne quand `showBadge` est true. Le badge prend `selected` quand le filtre est actif ou survolé.
- Pour des étiquettes avec texte et couleur (ex. « Validé » en vert), utiliser **UiPill**.
- En taille `xs`, seul un point coloré est affiché, sans texte.
