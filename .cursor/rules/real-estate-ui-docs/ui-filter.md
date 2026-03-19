# UiFilter

> Bouton déclencheur de filtre. Affiche un libellé, une icône et éventuellement un badge de comptage. **Important : le menu déroulant n’est pas inclus** — il doit être construit à l’extérieur. UiFilter sert uniquement de déclencheur.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `label` | `string` | — | Libellé du filtre (ex. « Statut », « Type de bien ») |
| `icon` | `string \| Component` | — | Icône Tabler (nom kebab-case, ex. `circle-check`, `home`) |
| `count` | `number` | — | Valeur affichée dans le badge quand `showBadge` est true |
| `active` | `boolean` | `false` | État actif (bordure et couleurs accent) |
| `showBadge` | `boolean` | `true` | Affiche le badge avec `count` |
| `showDropdown` | `boolean` | `true` | Affiche le chevron orienté vers le bas |
| `size` | `'default' \| 'xs'` | `'default'` | Taille du bouton |

## Slots

Aucun slot.

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `click` | `Event` | Émis au clic sur le bouton — utilisé pour ouvrir/fermer le menu déroulant |

## Exemples

### Usage basique (déclencheur uniquement)

```vue
<UiFilter
  label="Statut"
  icon="circle-check"
  :active="!!filters.statut"
  :show-badge="false"
  @click="toggleDropdown('statut')"
/>
```

### Taille xs (mode compact)

```vue
<UiFilter
  label="Statut"
  icon="circle-check"
  :active="!!filters.statut"
  :show-badge="false"
  size="xs"
  @click="toggleDropdown('statut')"
/>
```

### Pattern complet avec menu déroulant

Le menu déroulant **n’est pas fourni** par UiFilter. Il faut le construire avec un wrapper, une Transition et une liste d’options :

```vue
<div class="filter-dropdown" @click.stop>
  <UiFilter
    label="Statut"
    icon="circle-check"
    :active="!!filters.statut"
    :show-badge="false"
    @click="toggleDropdown('statut')"
  />
  <Transition name="dropdown">
    <div v-if="openDropdown === 'statut'" class="filter-menu">
      <button
        v-for="opt in statutOptions"
        :key="opt.value"
        class="filter-menu__item"
        :class="{ 'filter-menu__item--selected': filters.statut === opt.value }"
        @click="handleFilterClick('statut', opt.value)"
      >
        <span>{{ opt.label }}</span>
        <IconCheck v-if="filters.statut === opt.value" :size="14" :stroke-width="2.5" class="filter-menu__check" />
      </button>
    </div>
  </Transition>
</div>
```

### Script associé

```ts
const openDropdown = ref<string | null>(null);
const filters = ref<Record<string, string>>({});
const statutOptions = [
  { value: 'actif', label: 'Actif' },
  { value: 'archive', label: 'Archivé' },
];

function toggleDropdown(key: string) {
  openDropdown.value = openDropdown.value === key ? null : key;
}

function handleFilterClick(key: string, value: string) {
  filters.value[key] = filters.value[key] === value ? '' : value;
  openDropdown.value = null; // Fermer après sélection
}
```

### Styles du menu déroulant

```scss
.filter-dropdown {
  position: relative;
}

.filter-menu {
  position: absolute;
  top: calc(100% + 4px);
  left: 0;
  min-width: 160px;
  background: var(--surface-default);
  border: 1px solid var(--border-default-light);
  border-radius: var(--alias-border-radius-md);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  padding: var(--brand-scale-4);
  z-index: 50;
}

.filter-menu__item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  padding: var(--brand-scale-8) var(--brand-scale-12);
  border: none;
  background: transparent;
  border-radius: var(--alias-border-radius-sm);
  font-family: var(--font-family-body);
  font-size: var(--body-font-size);
  color: var(--text-body);
  cursor: pointer;
  text-align: left;
  gap: 8px;
}

.filter-menu__item:hover {
  background: var(--surface-neutral-hover);
}

.filter-menu__item--selected {
  font-weight: var(--font-weight-semi-bold);
  color: var(--text-action);
}

/* Transition dropdown */
.dropdown-enter-active,
.dropdown-leave-active {
  transition: opacity 0.15s ease, transform 0.15s ease;
}
.dropdown-enter-from,
.dropdown-leave-to {
  opacity: 0;
  transform: translateY(-4px);
}
```

## Bonnes pratiques

- Toujours envelopper le UiFilter et le menu dans un `div` avec `@click.stop` pour éviter la fermeture lors du clic sur le filtre.
- Utiliser `openDropdown` pour gérer quel menu est ouvert (un seul à la fois en général).
- Mettre `:active="!!filters.xxx"` pour indiquer qu’un filtre est appliqué.
- Fermer le menu après la sélection (réinitialiser `openDropdown`) pour une meilleure UX.
- Utiliser `size="xs"` sur les barres de filtres en mode compact (responsive).

## À ne pas faire

- Ne pas s’attendre à un menu déroulant intégré : UiFilter est uniquement un bouton déclencheur.
- Éviter d’oublier `@click.stop` sur le wrapper, sinon les clics peuvent se propager et fermer le menu.
- Ne pas ouvrir plusieurs menus en même temps sans gérer la fermeture des autres (prévoir un état unique pour `openDropdown`).

## Notes d'intégration

- **UiBadge** : utilisé en interne quand `showBadge` est true et `count` est défini.
- **Icônes** : noms Tabler en kebab-case (`circle-check`, `home`, `user`, `tag`, `hammer`).
- En mode `active`, la bordure et le texte passent en couleurs accent (orange).
- Le badge adopte un style sélectionné quand `active` ou au survol pour rester lisible sur fond accent.
