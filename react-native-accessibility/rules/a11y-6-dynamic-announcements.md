---
ruleId: A11Y-6
title: Announce dynamic content changes to screen readers
---

## [A11Y-6] Announce dynamic content changes to screen readers

### Reasoning

Screen reader users cannot see visual changes (toasts, counters updating, items being added/removed). If the content changes without an announcement, the user has no idea something happened.

### Incorrect

```tsx
function CartBadge({ count }) {
  return <Text>{count}</Text>;
}
```

```tsx
function Toast({ message }) {
  return <Text style={styles.toast}>{message}</Text>;
}
```

### Correct

```tsx
import { AccessibilityInfo, Platform } from 'react-native';

function CartBadge({ count }) {
  useEffect(() => {
    if (Platform.OS === 'ios') {
      AccessibilityInfo.announceForAccessibility(`${count} items in cart`);
    }
  }, [count]);

  return (
    <Text accessibilityLiveRegion="polite" accessibilityLabel={`${count} items in cart`}>
      {count}
    </Text>
  );
}
```

```tsx
function Toast({ message }) {
  useEffect(() => {
    if (Platform.OS === 'ios') {
      AccessibilityInfo.announceForAccessibility(message);
    }
  }, [message]);

  return (
    <Text
      accessibilityRole="alert"
      accessibilityLiveRegion="assertive"
    >
      {message}
    </Text>
  );
}
```

### When to Announce

| Scenario | Method |
|---|---|
| Toast / snackbar | `role="alert"` + `liveRegion="assertive"` + `announceForAccessibility` |
| Counter update | `liveRegion="polite"` + `announceForAccessibility` |
| Form validation error | `role="alert"` + move focus to error |
| Content loaded | `announceForAccessibility` |
| Item added/removed from list | `announceForAccessibility` |

### Platform Strategy

- **Android:** `accessibilityLiveRegion` handles it natively
- **iOS:** Call `AccessibilityInfo.announceForAccessibility()` in an effect
- **Cross-platform:** Do both — set `accessibilityLiveRegion` AND call `announceForAccessibility` on iOS
