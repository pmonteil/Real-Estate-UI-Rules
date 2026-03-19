# UiSwitch

> Interrupteur on/off conforme au DS. Pour activer ou désactiver une option en un clic.

## Props

| Prop | Type | Défaut | Description |
|------|------|--------|-------------|
| `modelValue` | `boolean` | `false` | État du switch (v-model) |
| `label` | `string` | — | Libellé affiché à droite du switch |
| `disabled` | `boolean` | `false` | Désactive le switch |
| `error` | `boolean` | `false` | État d’erreur (bordure rouge) |

## Slots

Le composant n'expose pas de slots publics.

## Événements

| Événement | Payload | Description |
|-----------|---------|-------------|
| `update:modelValue` | `boolean` | Émis au changement d’état |
| `change` | `boolean` | Émis au changement (même payload) |
| `focus` | `FocusEvent` | Émis au focus |
| `blur` | `FocusEvent` | Émis à la perte de focus |

## Exemples

### Usage basique

```vue
<UiSwitch v-model="notifications" label="Activer les notifications" />
```

### Avec label

```vue
<UiSwitch v-model="headerShowLogo" label="Afficher le logo dans l'en-tête" />
```

### Désactivé / verrouillé

```vue
<UiSwitch v-model="lockedOption" label="Option verrouillée" disabled />
```

### État erreur

```vue
<UiSwitch v-model="invalidOption" label="Option invalide" error />
```

### Avec callback de mise à jour

```vue
<UiSwitch v-model="forcePresets" @update:model-value="onForcePresetsToggle" />
```

## Bonnes pratiques

- Utiliser un `label` clair pour expliquer l’effet du switch.
- Utiliser `disabled` quand l’option est verrouillée ou non applicable.
- Utiliser `error` pour signaler une configuration invalide.
- Réserver le switch aux choix binaires (on/off, oui/non).
- Éviter les libellés négatifs (ex: « Désactiver X ») : préférer « Activer X » avec état inversé si besoin.

## À ne pas faire

- Ne pas utiliser une checkbox pour un simple on/off : privilégier `UiSwitch`.
- Ne pas oublier le `label` pour l’accessibilité.
- Ne pas utiliser plusieurs switches pour une même valeur.
- Ne pas masquer l’état actuel : le switch doit refléter clairement on/off.
- Ne pas utiliser `error` pour un simple état désactivé.

## Notes d'intégration

- Expose `focus()`, `blur()` et `toggle()` via `defineExpose`.
- Basé sur une checkbox native masquée (accessibilité conservée).
- Dimensions : track 40×22px, thumb 14px.
- Utilisé massivement dans `EmailBuilderPage` pour les options header/footer et de blocs.
- Compatible avec `v-model` et `:model-value` + `@update:model-value` pour les callbacks.
