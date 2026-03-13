---
ruleId: A11Y-1
title: All interactive elements need accessible labels
---

## [A11Y-1] All interactive elements need accessible labels

### Reasoning

Without `accessibilityLabel`, screen readers construct a label from child Text nodes, which produces confusing or meaningless announcements — especially for icon-only buttons, image buttons, and components with complex children.

### Incorrect

```tsx
<Pressable onPress={onSend}>
  <SendIcon />
</Pressable>
```

```tsx
<TouchableOpacity onPress={onClose}>
  <Image source={closeIcon} />
</TouchableOpacity>
```

### Correct

```tsx
<Pressable
  onPress={onSend}
  accessibilityLabel="Send message"
  accessibilityRole="button"
>
  <SendIcon />
</Pressable>
```

```tsx
<TouchableOpacity
  onPress={onClose}
  accessibilityLabel="Close"
  accessibilityRole="button"
>
  <Image source={closeIcon} />
</TouchableOpacity>
```

### Guidelines

- Label describes what the element **does**, not what it **looks like**: "Delete" not "Trash can icon"
- Don't include the role in the label: "Send" not "Send button"
- Don't include state in the label: use `accessibilityState` instead
- Use sentence case
- Keep labels concise — 2-4 words is ideal
