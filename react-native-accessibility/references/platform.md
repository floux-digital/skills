# Platform Differences

iOS and Android handle accessibility differently. This reference covers what is available on each platform and how to handle the gaps.


## Prop Availability

| Prop | iOS | Android |
|---|---|---|
| `accessible` | `isAccessibilityElement` | `focusable` |
| `accessibilityLabel` | Yes | Yes |
| `accessibilityHint` | User can disable | Always reads |
| `accessibilityRole` / `role` | Yes | Yes |
| `accessibilityState` | Yes | Yes |
| `accessibilityValue` | Yes | Yes |
| `accessibilityActions` | Yes | Yes |
| `accessibilityLanguage` | Yes | No |
| `accessibilityIgnoresInvertColors` | Yes | No |
| `accessibilityViewIsModal` | Yes | No |
| `accessibilityElementsHidden` | Yes | No |
| `accessibilityShowsLargeContentViewer` | Yes (13+) | No |
| `accessibilityLabelledBy` | No | Yes |
| `accessibilityLiveRegion` | No | Yes |
| `importantForAccessibility` | No | Yes |
| `onAccessibilityEscape` | Yes | No |
| `onAccessibilityTap` | Yes | No |
| `onMagicTap` | Yes | No |


## Common Patterns for Cross-Platform

### Hiding Background Content (Modals)

```tsx
// iOS: use accessibilityViewIsModal on the modal
<View accessibilityViewIsModal={true}>
  {/* modal content */}
</View>

// Android: hide the background
<View importantForAccessibility={isModalOpen ? 'no-hide-descendants' : 'auto'}>
  {/* background content */}
</View>
```

### Hiding Decorative Elements

```tsx
// iOS
<Image source={decorativeIcon} accessibilityElementsHidden={true} />

// Android
<Image source={decorativeIcon} importantForAccessibility="no" />

// Cross-platform using aria
<Image source={decorativeIcon} aria-hidden={true} />
```

### Announcing Dynamic Content

```tsx
import { AccessibilityInfo, Platform } from 'react-native';

// iOS: use announceForAccessibility
AccessibilityInfo.announceForAccessibility('3 new messages');

// Android: use live regions on the element
<Text accessibilityLiveRegion="polite">{messageCount} new messages</Text>
```

For cross-platform, do both: set `accessibilityLiveRegion` on Android AND call `announceForAccessibility` on iOS.

```tsx
function announceChange(message) {
  if (Platform.OS === 'ios') {
    AccessibilityInfo.announceForAccessibility(message);
  }
  // On Android, live regions handle this automatically
}
```

### Form Label Association

```tsx
// Android: use accessibilityLabelledBy
<Text nativeID="emailLabel">Email</Text>
<TextInput accessibilityLabelledBy="emailLabel" />

// iOS: use accessibilityLabel directly
<TextInput accessibilityLabel="Email" />

// Cross-platform
<Text nativeID="emailLabel">Email</Text>
<TextInput
  accessibilityLabel="Email"
  accessibilityLabelledBy={Platform.OS === 'android' ? 'emailLabel' : undefined}
/>
```


## Platform-Specific File Pattern

For components with substantially different accessibility implementations, use platform-specific files instead of inline `Platform.OS` checks:

```
MyComponent/
  index.ios.tsx    — uses onAccessibilityEscape, accessibilityViewIsModal
  index.android.tsx — uses importantForAccessibility, accessibilityLiveRegion
  index.tsx        — shared logic
```
