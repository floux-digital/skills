---
ruleId: A11Y-8
title: Manage focus after navigation and content changes
---

## [A11Y-8] Manage focus after navigation and content changes

### Reasoning

After a screen transition or content removal, screen reader focus can land on an invisible element or jump unpredictably. Explicit focus management ensures users always know where they are.

### Incorrect

```tsx
function DetailScreen() {
  // No focus management — VoiceOver lands on whatever element the system picks
  return (
    <View>
      <Text style={styles.title}>Order Details</Text>
      <Text>Order #12345</Text>
    </View>
  );
}
```

### Correct

```tsx
function DetailScreen() {
  const titleRef = useRef(null);

  useEffect(() => {
    const node = findNodeHandle(titleRef.current);
    if (node) {
      setTimeout(() => {
        AccessibilityInfo.setAccessibilityFocus(node);
      }, 100);
    }
  }, []);

  return (
    <View>
      <Text ref={titleRef} accessibilityRole="header" style={styles.title}>
        Order Details
      </Text>
      <Text>Order #12345</Text>
    </View>
  );
}
```

### Focus Scenarios

| Event | Focus Target |
|---|---|
| Navigate to new screen | Screen title or first meaningful element |
| Open modal / bottom sheet | Modal title or close button |
| Close modal | Element that triggered the modal |
| Delete list item | Next item, or previous if last |
| Submit form successfully | Success message |
| Form validation error | First error message or first invalid input |
| Content finishes loading | First loaded content element or announcement |
