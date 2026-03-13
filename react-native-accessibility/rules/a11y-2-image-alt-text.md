---
ruleId: A11Y-2
title: Images need alt text or decorative marking
---

## [A11Y-2] Images need alt text or decorative marking

### Reasoning

Screen readers announce images without labels as "image" with no context. Meaningful images need descriptions. Decorative images should be hidden so they don't clutter screen reader navigation.

### Incorrect

```tsx
<Image source={{ uri: profilePhoto }} />
```

```tsx
<Image source={backgroundPattern} />
```

### Correct

```tsx
{/* Meaningful image — provide description */}
<Image
  source={{ uri: profilePhoto }}
  accessibilityLabel="Profile photo of Jane Doe"
  accessibilityRole="image"
/>
```

```tsx
{/* Decorative image — hide from screen readers */}
<Image source={backgroundPattern} aria-hidden={true} />
```

### Guidelines

- Meaningful images: set `accessibilityLabel` that describes the content or purpose
- Decorative images (backgrounds, dividers, icons next to text): hide with `aria-hidden={true}`
- User avatars: include the person's name in the label
- Status icons (checkmark, warning): describe the status, not the icon shape
