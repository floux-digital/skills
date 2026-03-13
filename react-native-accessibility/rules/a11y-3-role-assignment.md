---
ruleId: A11Y-3
title: Assign correct accessibility roles
---

## [A11Y-3] Assign correct accessibility roles

### Reasoning

Roles tell screen reader users what kind of element they're interacting with and what behavior to expect. A `View` with `onPress` that lacks a role is announced as just text — users won't know it's tappable.

### Incorrect

```tsx
<Pressable onPress={onToggle}>
  <Text>Dark Mode</Text>
</Pressable>
```

```tsx
<View>
  <Text style={styles.sectionTitle}>Settings</Text>
</View>
```

### Correct

```tsx
<Pressable
  onPress={onToggle}
  accessibilityRole="switch"
  accessibilityState={{ checked: isDarkMode }}
  accessibilityLabel="Dark mode"
>
  <Text>Dark Mode</Text>
</Pressable>
```

```tsx
<View>
  <Text accessibilityRole="header" style={styles.sectionTitle}>Settings</Text>
</View>
```

### Common Role Mappings

| Component | Role |
|---|---|
| Tappable action | `button` |
| Navigation link | `link` |
| On/off toggle | `switch` |
| Checkbox | `checkbox` |
| Radio option | `radio` |
| Slider / stepper | `adjustable` |
| Section title | `header` |
| Image | `image` |
| Search input | `search` |
| Tab | `tab` |
| Progress indicator | `progressbar` |
| Alert / toast | `alert` |
