---
ruleId: A11Y-5
title: Enforce minimum touch target size
---

## [A11Y-5] Enforce minimum touch target size

### Reasoning

Small touch targets are difficult for users with motor impairments to tap accurately. Both Apple (44x44 pt) and Google (48x48 dp) define minimum sizes. 44x44 is the minimum acceptable for React Native.

### Incorrect

```tsx
<Pressable
  onPress={onInfo}
  style={{ width: 24, height: 24 }}
>
  <InfoIcon size={24} />
</Pressable>
```

### Correct

```tsx
<Pressable
  onPress={onInfo}
  accessibilityLabel="More info"
  accessibilityRole="button"
  hitSlop={10}
  style={{ width: 44, height: 44, alignItems: 'center', justifyContent: 'center' }}
>
  <InfoIcon size={24} />
</Pressable>
```

### Guidelines

- Minimum 44x44 points for all tappable elements
- Use `hitSlop` to increase the tappable area without changing visual size
- If an icon is small, pad the touchable container — do not make the icon itself larger
- Verify both `width`/`height` and `minWidth`/`minHeight` — inline styles can override layout constraints
- Close buttons, icon buttons, and toolbar items are the most common offenders
