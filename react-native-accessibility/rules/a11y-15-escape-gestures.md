---
ruleId: A11Y-15
title: Support iOS escape and magic tap gestures
---

## [A11Y-15] Support iOS escape and magic tap gestures

### Reasoning

VoiceOver users rely on the two-finger Z-scrub gesture to go back or dismiss. Without `onAccessibilityEscape`, they may get stuck in a screen with no way to navigate back.

### Incorrect

```tsx
function SettingsSheet({ onClose }) {
  return (
    <View style={styles.sheet}>
      <Text>Settings</Text>
      <Pressable onPress={onClose}><Text>Close</Text></Pressable>
    </View>
  );
}
```

The close button works, but the escape gesture does nothing.

### Correct

```tsx
function SettingsSheet({ onClose }) {
  return (
    <View
      style={styles.sheet}
      onAccessibilityEscape={() => {
        onClose();
        return true;
      }}
      accessibilityViewIsModal={true}
    >
      <Text accessibilityRole="header">Settings</Text>
      <Pressable
        onPress={onClose}
        accessibilityLabel="Close"
        accessibilityRole="button"
      >
        <Text>Close</Text>
      </Pressable>
    </View>
  );
}
```

### Guidelines

- Implement `onAccessibilityEscape` on modals, bottom sheets, and overlay screens
- Return `true` from the handler to indicate the escape was handled
- `onMagicTap` (two-finger double-tap) should trigger the most relevant action on the current screen (e.g., answer/end call, play/pause media)
- These are iOS-only — no equivalent exists on Android, so they are additive, not required for cross-platform parity
