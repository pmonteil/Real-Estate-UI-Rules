# UiLink

> Composant de lien stylisé. Génère un `<a>`, `<router-link>` ou `<button>` selon les props fournies. Intègre des icônes Tabler avec animation bounce au survol.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `label` | `string` | — | **Requis.** Texte du lien |
| `href` | `string` | `undefined` | URL externe → génère un `<a>` |
| `to` | `string \| object` | `undefined` | Route Vue Router → génère un `<router-link>` |
| `iconLeft` | `string \| Component` | `undefined` | Icône à gauche du label (nom Tabler ou composant) |
| `iconRight` | `string \| Component` | `undefined` | Icône à droite du label (nom Tabler ou composant) |
| `disabled` | `boolean` | `false` | Désactive le lien (curseur interdit, pas de navigation) |
| `active` | `boolean` | `false` | État actif — texte en couleur `--text-body` au lieu de secondaire |

## Slots

Aucun slot. Le contenu est défini via la prop `label`.

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `click` | `MouseEvent` | Émis au clic (bloqué si `disabled`) |

## Comportement du tag

| Props fournies | Tag HTML généré |
|----------------|-----------------|
| `to` défini | `<router-link>` |
| `href` défini | `<a>` |
| Aucun des deux | `<button>` |

## Exemples

### Lien de navigation interne

```vue
<UiLink label="Voir les détails" to="/biens/123" icon-right="chevron-right" />
```

### Lien externe

```vue
<UiLink label="Documentation" href="https://docs.example.com" icon-left="external-link" />
```

### Action (bouton déguisé en lien)

```vue
<UiLink label="Annuler" @click="cancel" />
```

### Avec état actif

```vue
<UiLink label="Mes widgets" :active="currentPage === 'widgets'" to="/widgets" />
```

## Bonnes pratiques

- Utiliser `UiLink` pour tout lien textuel au lieu de `<a>` natif ou texte cliquable custom.
- Préférer `to` pour la navigation interne (Vue Router) et `href` pour les liens externes.
- L'icône `chevron-right` à droite est un bon pattern pour indiquer la navigation.
- L'icône `external-link` à droite convient pour les liens sortants.
- Utiliser le mode bouton (sans `href` ni `to`) pour les actions textuelles comme "Annuler", "Retour".

## À ne pas faire

- Ne pas utiliser `<a>` ou `<router-link>` natif si `UiLink` convient.
- Ne pas utiliser `UiLink` pour une action principale — utiliser `UiButton` à la place.
- Ne pas mettre un label vide — le label est requis.
- Ne pas ajouter de style `text-decoration` custom — le composant gère les états.
- Ne pas confondre avec `UiButton variant="ghost"` : `UiLink` est pour des liens/actions secondaires discrets.

## Notes d'intégration

- Utilisé en interne par `UiBreadcrumb` pour chaque item du fil d'Ariane.
- L'animation bounce des icônes au hover est intégrée (pas besoin de CSS custom).
- Couleur de base : `--text-body-secondary`. Hover : `--text-action-hover`. Active : `--text-body`.
- Les icônes sont chargées dynamiquement depuis `@tabler/icons-vue` (taille 18px, stroke 2).
