# UiAvatar

> Avatar utilisateur affichant une photo ou des initiales. Supporte plusieurs tailles et types (initiales avec bordure dégradée, photo, couleurs d’arrière-plan).

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `initials` | `string` | `"PM"` | Initiales affichées (max 2 caractères, mis en majuscules) |
| `src` | `string` | — | URL de l’image (active le type `photo` si défini) |
| `alt` | `string` | — | Texte alternatif de l’image |
| `size` | `'xs' \| 'default' \| 'xl'` | `'default'` | Taille de l’avatar |
| `type` | `'initials' \| 'photo' \| 'colored'` | `'initials'` | Type d’affichage |
| `color` | `'neutral' \| 'red' \| 'blue' \| 'green' \| 'orange' \| 'purple'` | `'neutral'` | Couleur de fond (pour `type="colored"`) |

## Slots

Aucun slot.

## Événements

Aucun événement.

## Exemples

### Usage basique (initiales)

```vue
<UiAvatar initials="PM" />
<UiAvatar initials="JD" />
<UiAvatar initials="AB" size="xl" />
```

### Tailles

```vue
<UiAvatar initials="PM" size="xs" />
<UiAvatar initials="JD" size="default" />
<UiAvatar initials="AB" size="xl" />
```

### Type photo

```vue
<UiAvatar
  src="/avatars/user.jpg"
  alt="Photo de Jean Dupont"
  initials="JD"
/>
```

### Type colored (couleurs par utilisateur)

```vue
<UiAvatar initials="BL" type="colored" color="blue" />
<UiAvatar initials="RD" type="colored" color="red" />
<UiAvatar initials="GR" type="colored" color="green" />
<UiAvatar initials="OR" type="colored" color="orange" />
<UiAvatar initials="PU" type="colored" color="purple" />
<UiAvatar initials="XY" type="colored" color="neutral" />
```

### Dans une liste ou un header

```vue
<div class="user-card">
  <UiAvatar :initials="user.initials" :src="user.avatarUrl" size="default" />
  <div>
    <span class="user-name">{{ user.name }}</span>
    <span class="user-role">{{ user.role }}</span>
  </div>
</div>
```

## Bonnes pratiques

- Fournir des initiales de 1 ou 2 caractères (ex. prénom + nom : « JD »).
- Utiliser `type="photo"` quand une image est disponible, sinon `initials` ou `colored`.
- Choisir `type="colored"` avec une couleur stable par utilisateur pour la reconnaissance visuelle.
- Utiliser `size="xs"` dans les listes denses, `default` pour les profils standards, `xl` pour les en-têtes ou vues détaillées.

## À ne pas faire

- Ne pas utiliser `src` avec `type="initials"` — `src` force le type photo.
- Éviter des initiales de plus de 2 caractères (elles sont tronquées).
- Ne pas oublier `alt` ou `initials` pour l’accessibilité quand une photo est affichée.

## Notes d'intégration

- **Type initials** : bordure en dégradé (Modelo : vert → bleu → orange), fond sombre, initiales en blanc.
- **Type photo** : l’image remplit l’avatar avec `object-fit: cover`.
- **Type colored** : fond coloré selon `color`, utile pour différencier des utilisateurs.
- Effet de zoom au survol (`scale(1.05)`) et au clic (`scale(0.95)`).
