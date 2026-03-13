# Screen Readers

## VoiceOver (iOS)

### How Users Navigate
- **Swipe right/left** — move to next/previous element
- **Double-tap** — activate the focused element
- **Three-finger swipe** — scroll
- **Two-finger Z-scrub** — escape/go back (triggers `onAccessibilityEscape`)
- **Two-finger double-tap** — magic tap (triggers `onMagicTap`)
- **Swipe up/down on adjustable** — increment/decrement

### What VoiceOver Reads
For each element, VoiceOver reads in order:
1. `accessibilityLabel`
2. `accessibilityRole` (as a trait, e.g., "button")
3. `accessibilityState` (e.g., "selected", "dimmed")
4. `accessibilityHint` (if user has hints enabled)
5. `accessibilityValue` (for adjustable/range elements)

### Writing Good Labels for VoiceOver
- Be concise: "Delete" not "Tap to delete this item"
- Don't include the role in the label: "Send" not "Send button" (VoiceOver adds "button")
- Don't include state in the label: use `accessibilityState` instead
- Use sentence case: "Mark as read" not "Mark As Read"


## TalkBack (Android)

### How Users Navigate
- **Swipe right/left** — move to next/previous element
- **Double-tap** — activate the focused element
- **Two-finger swipe** — scroll
- **Volume up/down** — increment/decrement adjustable elements
- **Double-tap and hold** — long press (triggers `longpress` action)

### What TalkBack Reads
For each element, TalkBack reads in order:
1. `accessibilityLabel`
2. Role description
3. `accessibilityState`
4. `accessibilityHint` (always read, user cannot disable)
5. Usage hint (e.g., "Double tap to activate")

### Key Differences from VoiceOver
- `accessibilityHint` is always read — keep it short
- No magic tap or escape gesture support
- Use `accessibilityLiveRegion` to announce dynamic content (not available on iOS)
- Use `accessibilityLabelledBy` to associate form labels (not available on iOS)
- Long press action is supported (not available on iOS)


## Screen Reader Announcements

### Programmatic Announcements
```tsx
import { AccessibilityInfo } from 'react-native';

// Announce to screen reader
AccessibilityInfo.announceForAccessibility('Item deleted');
```

Use for: toast messages, form submission confirmations, content loaded notifications.

### Live Regions (Android)
```tsx
<Text accessibilityLiveRegion="polite">
  {count} items in cart
</Text>
```

- `polite` — announces when user is idle
- `assertive` — interrupts current speech

### Detecting Screen Reader
```tsx
import { AccessibilityInfo } from 'react-native';

const isActive = await AccessibilityInfo.isScreenReaderEnabled();

// Or listen for changes
const subscription = AccessibilityInfo.addEventListener(
  'screenReaderChanged',
  (isEnabled) => { /* handle */ }
);
```

Use sparingly — prefer building UIs that work for everyone by default.


## Sending Focus (Android)
```tsx
import { Platform, UIManager, findNodeHandle } from 'react-native';

if (Platform.OS === 'android') {
  UIManager.sendAccessibilityEvent(
    findNodeHandle(viewRef.current),
    UIManager.AccessibilityEventTypes.typeViewFocused,
  );
}
```
