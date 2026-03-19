# UiButton

> Bouton d'action principal du Design System. Remplace le `<button>` natif pour toutes les actions (primaire, secondaire, destructive, icône seule).

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `label` | `string` | — | Texte affiché dans le bouton |
| `icon` | `string \| Component \| FunctionalComponent` | — | Nom d'icône Tabler (ex: `"plus"`, `"mail"`) ou composant d'icône directement |
| `loading` | `boolean` | `false` | Affiche un spinner et désactive les clics |
| `disabled` | `boolean` | `false` | Désactive le bouton |
| `variant` | `'primary' \| 'secondary' \| 'ghost' \| 'error' \| 'accent' \| 'ai' \| 'third' \| 'tertiary'` | `'primary'` | Style visuel du bouton |
| `size` | `'sm' \| 'md'` | `'md'` | Taille du bouton |

## Slots

| Slot | Description |
|------|-------------|
| `icon` | Remplace l'icône par défaut (props) par un contenu personnalisé |

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `click` | `Event` | Émis au clic sur le bouton |

## Exemples

### Usage basique

```vue
<UiButton label="Sauvegarder" variant="primary" @click="handleSave" />
<UiButton label="Annuler" variant="secondary" @click="handleCancel" />
```

### Variants

```vue
<!-- Action principale (1 par vue) -->
<UiButton label="Enregistrer" variant="primary" @click="save" />

<!-- Action secondaire -->
<UiButton label="Annuler" variant="secondary" @click="cancel" />

<!-- Action destructive -->
<UiButton label="Supprimer" variant="error" icon="trash" @click="remove" />

<!-- Style discret (liens/boutons tertiaires) -->
<UiButton label="Modifier" variant="ghost" icon="edit" />

<!-- Accent orange (alerte, warning) -->
<UiButton label="Attention" variant="accent" icon="alert-triangle" />

<!-- Style IA / gradient violet -->
<UiButton label="Générer" variant="ai" />
```

### Tailles et icônes

```vue
<UiButton label="Nouveau bien" variant="primary" icon="plus" size="md" />
<UiButton label="Tester" variant="secondary" icon="send" size="sm" />
<UiButton icon="trash" variant="ghost" size="sm" @click="remove" />
```

### États

```vue
<UiButton label="Chargement..." variant="primary" loading />
<UiButton label="Désactivé" variant="primary" disabled />
```

## Bonnes pratiques

- **Un seul bouton primary par zone** (header, modal, formulaire) pour la clarté.
- Utiliser `primary` pour l’action principale (sauvegarder, valider).
- Utiliser `secondary` pour les actions annexes (annuler, retour).
- Utiliser `error` pour les actions destructives (supprimer, quitter sans sauvegarder).
- Utiliser `ghost` pour les actions peu mises en avant (modifier, éditer).
- Garder les tailles cohérentes dans une même zone : `sm` pour les actions secondaires, `md` pour les CTAs.
- Toujours fournir un `label` sauf pour les boutons icon-only (qui deviennent carrés automatiquement).

## À ne pas faire

- **Ne pas utiliser** `<button>` natif à la place de `UiButton`.
- Ne pas mettre plusieurs boutons `primary` côte à côte.
- Ne pas utiliser `error` pour des actions non destructives.
- Ne pas définir manuellement des couleurs ou des styles, les variants s’en chargent.
- Ne pas combiner `loading` et `disabled`.

## Notes d'intégration

- Icônes : nom kebab-case (ex: `"arrow-left"`, `"plus"`) → chargement dynamique depuis `@tabler/icons-vue`.
- Bouton icon-only : si `icon` est fourni sans `label`, le bouton devient carré (28×28 sm, 36×36 md).
- Basé sur Quasar `q-btn` avec `flat`/`unelevated` selon le variant.
- Méthodes exposées : aucune (le composant n’expose pas `focus()` ou similaire).
