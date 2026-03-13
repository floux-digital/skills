# Focus Management

Proper focus management ensures screen reader users always know where they are after the UI changes.


## When to Move Focus

- **After navigation** — focus the screen title or first meaningful element
- **After opening a modal/bottom sheet** — focus the modal title or close button
- **After closing a modal** — return focus to the element that triggered it
- **After deleting an item** — focus the next item, or the previous if it was the last
- **After content loads** — announce completion and optionally move focus
- **After error** — focus the error message or the problematic input


## Programmatic Focus (iOS)

Use `AccessibilityInfo.setAccessibilityFocus` with a ref:

```tsx
import { useRef, useEffect } from 'react';
import { AccessibilityInfo, findNodeHandle, Text } from 'react-native';

function ScreenTitle({ title }) {
  const titleRef = useRef(null);

  useEffect(() => {
    const node = findNodeHandle(titleRef.current);
    if (node) {
      // Small delay to let the screen finish rendering
      setTimeout(() => {
        AccessibilityInfo.setAccessibilityFocus(node);
      }, 100);
    }
  }, []);

  return (
    <Text ref={titleRef} accessibilityRole="header">
      {title}
    </Text>
  );
}
```


## Programmatic Focus (Android)

```tsx
import { Platform, UIManager, findNodeHandle } from 'react-native';

function focusElement(ref) {
  if (Platform.OS === 'android') {
    const node = findNodeHandle(ref.current);
    if (node) {
      UIManager.sendAccessibilityEvent(
        node,
        UIManager.AccessibilityEventTypes.typeViewFocused,
      );
    }
  }
}
```


## Focus Trapping in Modals

When a modal is open, screen reader users should not be able to navigate to content behind it.

### iOS
Use `accessibilityViewIsModal` on the modal container:

```tsx
<View accessibilityViewIsModal={true}>
  <Text accessibilityRole="header">Confirm Delete</Text>
  <Pressable accessibilityLabel="Cancel" onPress={onCancel}>
    <Text>Cancel</Text>
  </Pressable>
  <Pressable accessibilityLabel="Delete" onPress={onDelete}>
    <Text>Delete</Text>
  </Pressable>
</View>
```

### Android
Use `importantForAccessibility="no-hide-descendants"` on the background content:

```tsx
<View importantForAccessibility={isModalOpen ? 'no-hide-descendants' : 'auto'}>
  {/* Main screen content */}
</View>

<Modal visible={isModalOpen}>
  {/* Modal content */}
</Modal>
```


## Focus Order

### Default Behavior
React Native follows DOM/view hierarchy order by default (top-to-bottom, left-to-right).

### Custom Focus Order
Use `experimental_accessibilityOrder` to override:

```tsx
<View experimental_accessibilityOrder={['header', 'cta', 'description']}>
  <Text accessible nativeID="description">Details about this item</Text>
  <Text accessible nativeID="header" accessibilityRole="header">Title</Text>
  <Pressable accessible nativeID="cta" onPress={act}>
    <Text>Take Action</Text>
  </Pressable>
</View>
```


## Focus Restoration Hook Pattern

```tsx
function useFocusRestoration(triggerRef, isOpen) {
  useEffect(() => {
    if (!isOpen && triggerRef.current) {
      const node = findNodeHandle(triggerRef.current);
      if (node) {
        AccessibilityInfo.setAccessibilityFocus(node);
      }
    }
  }, [isOpen]);
}
```
