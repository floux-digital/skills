---
ruleId: A11Y-4
title: Communicate component state to assistive technology
---

## [A11Y-4] Communicate component state to assistive technology

### Reasoning

Screen reader users cannot see visual state changes (grayed out, highlighted, checked). `accessibilityState` is the only way they know an element is disabled, selected, expanded, or checked.

### Incorrect

```tsx
<Pressable
  onPress={isDisabled ? undefined : onPress}
  style={isDisabled ? styles.disabled : styles.active}
>
  <Text>Submit</Text>
</Pressable>
```

```tsx
<Pressable onPress={toggle} style={isOpen ? styles.expanded : styles.collapsed}>
  <Text>More Options</Text>
</Pressable>
```

### Correct

```tsx
<Pressable
  onPress={onPress}
  accessibilityLabel="Submit"
  accessibilityRole="button"
  accessibilityState={{ disabled: isDisabled }}
  disabled={isDisabled}
>
  <Text>Submit</Text>
</Pressable>
```

```tsx
<Pressable
  onPress={toggle}
  accessibilityLabel="More options"
  accessibilityRole="button"
  accessibilityState={{ expanded: isOpen }}
>
  <Text>More Options</Text>
</Pressable>
```

### Available States

| State | Type | Use For |
|---|---|---|
| `disabled` | boolean | Grayed-out / non-interactive elements |
| `selected` | boolean | Currently selected tab, list item, option |
| `checked` | boolean \| `'mixed'` | Checkboxes, toggles, radio buttons |
| `busy` | boolean | Loading / processing elements |
| `expanded` | boolean | Collapsible sections, dropdowns, accordions |
