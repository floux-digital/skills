---
ruleId: A11Y-14
title: Trap focus within modals and overlays
---

## [A11Y-14] Trap focus within modals and overlays

### Reasoning

When a modal is open, screen reader users can navigate to elements behind it — interacting with invisible content. Focus must be contained within the modal and restored when it closes.

### Incorrect

```tsx
<View>
  <MainContent />
  {isModalOpen && (
    <View style={styles.modal}>
      <Text>Are you sure?</Text>
      <Pressable onPress={confirm}><Text>Yes</Text></Pressable>
      <Pressable onPress={cancel}><Text>No</Text></Pressable>
    </View>
  )}
</View>
```

VoiceOver can still reach `MainContent` elements behind the modal.

### Correct

```tsx
<View importantForAccessibility={isModalOpen ? 'no-hide-descendants' : 'auto'}>
  <MainContent />
</View>

{isModalOpen && (
  <View
    style={styles.modal}
    accessibilityViewIsModal={true}
  >
    <Text accessibilityRole="header">Are you sure?</Text>
    <Pressable
      accessibilityLabel="Yes, confirm"
      accessibilityRole="button"
      onPress={confirm}
    >
      <Text>Yes</Text>
    </Pressable>
    <Pressable
      accessibilityLabel="No, cancel"
      accessibilityRole="button"
      onPress={cancel}
    >
      <Text>No</Text>
    </Pressable>
  </View>
)}
```

### Guidelines

- **iOS:** Set `accessibilityViewIsModal={true}` on the modal container
- **Android:** Set `importantForAccessibility="no-hide-descendants"` on the background content
- React Native's `<Modal>` component handles focus trapping on both platforms — prefer it over custom overlays
- When the modal opens, move focus to its title or first element
- When the modal closes, return focus to the trigger element (see A11Y-8)
- Support the escape gesture on iOS with `onAccessibilityEscape` (see A11Y-15)
