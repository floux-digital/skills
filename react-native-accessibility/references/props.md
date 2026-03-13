# Accessibility Props Reference

## Core Props (Both Platforms)

### `accessible`
Makes an element discoverable by screen readers. All touchable elements are accessible by default.
- iOS: maps to `isAccessibilityElement`
- Android: maps to `focusable`

### `accessibilityLabel`
Text read aloud by screen readers. Set on every interactive element.

```tsx
<Pressable accessibilityLabel="Delete message" onPress={onDelete}>
  <TrashIcon />
</Pressable>
```

### `accessibilityHint`
Describes what happens after the action, when not obvious from the label.
- iOS: read after label (user can disable in settings)
- Android: always read after label

```tsx
<Pressable
  accessibilityLabel="Submit"
  accessibilityHint="Sends your response and closes the form"
  onPress={onSubmit}
/>
```

### `accessibilityRole`
Communicates the purpose of a component. Valid values:
`adjustable` | `alert` | `button` | `checkbox` | `combobox` | `header` | `image` | `imagebutton` | `keyboardkey` | `link` | `menu` | `menubar` | `menuitem` | `none` | `progressbar` | `radio` | `radiogroup` | `scrollbar` | `search` | `spinbutton` | `summary` | `switch` | `tab` | `tablist` | `text` | `timer` | `togglebutton` | `toolbar` | `grid`

### `role`
Newer prop that takes precedence over `accessibilityRole`. Valid values:
`alert` | `button` | `checkbox` | `combobox` | `grid` | `heading` | `img` | `link` | `list` | `listitem` | `menu` | `menubar` | `menuitem` | `none` | `presentation` | `progressbar` | `radio` | `radiogroup` | `scrollbar` | `searchbox` | `slider` | `spinbutton` | `summary` | `switch` | `tab` | `tablist` | `timer` | `toolbar`

### `accessibilityState`
Current state of a component. Object with optional fields:
- `disabled` (boolean) — element is disabled
- `selected` (boolean) — element is selected
- `checked` (boolean | `'mixed'`) — checkbox/toggle state
- `busy` (boolean) — element is loading
- `expanded` (boolean) — expandable element is open/closed

```tsx
<Pressable
  accessibilityRole="checkbox"
  accessibilityState={{ checked: isChecked, disabled: isDisabled }}
  onPress={toggle}
/>
```

### `accessibilityValue`
Current value for range-based components (sliders, progress bars).
- `min` (integer) — minimum value
- `max` (integer) — maximum value
- `now` (integer) — current value
- `text` (string) — textual description (overrides min/now/max)

```tsx
<View
  accessibilityRole="progressbar"
  accessibilityValue={{ min: 0, max: 100, now: 65 }}
/>
```

### `accessibilityActions` + `onAccessibilityAction`
Define custom actions for assistive technology.

Standard actions: `activate`, `increment`, `decrement`, `magicTap` (iOS), `escape` (iOS), `longpress` (Android), `expand` (Android), `collapse` (Android).

```tsx
<View
  accessible
  accessibilityRole="adjustable"
  accessibilityActions={[
    { name: 'increment', label: 'Increase' },
    { name: 'decrement', label: 'Decrease' },
  ]}
  onAccessibilityAction={(event) => {
    switch (event.nativeEvent.actionName) {
      case 'increment': increase(); break;
      case 'decrement': decrease(); break;
    }
  }}
/>
```


## ARIA Props (Web-Compatible Equivalents)

These map directly to the props above:
- `aria-label` → `accessibilityLabel`
- `aria-hidden` → `accessibilityElementsHidden` (iOS) / `importantForAccessibility` (Android)
- `aria-busy` → `accessibilityState.busy`
- `aria-checked` → `accessibilityState.checked`
- `aria-disabled` → `accessibilityState.disabled`
- `aria-expanded` → `accessibilityState.expanded`
- `aria-selected` → `accessibilityState.selected`
- `aria-valuemax/min/now/text` → `accessibilityValue` fields
- `aria-modal` (iOS) → `accessibilityViewIsModal`
- `aria-live` (Android) → `accessibilityLiveRegion`
- `aria-labelledby` (Android) → `accessibilityLabelledBy`


## iOS-Only Props

| Prop | Purpose |
|---|---|
| `accessibilityLanguage` | Language for screen reader (BCP 47, e.g. `"fr-FR"`) |
| `accessibilityIgnoresInvertColors` | Prevents color inversion on images/photos |
| `accessibilityViewIsModal` | VoiceOver ignores sibling elements |
| `accessibilityElementsHidden` | Hides element and children from VoiceOver |
| `accessibilityShowsLargeContentViewer` | Large content viewer on long press (iOS 13+) |
| `accessibilityLargeContentTitle` | Title for large content viewer |
| `onAccessibilityEscape` | Two-finger Z-gesture handler |
| `onAccessibilityTap` | Double-tap handler for focused element |
| `onMagicTap` | Two-finger double-tap handler |


## Android-Only Props

| Prop | Purpose |
|---|---|
| `accessibilityLabelledBy` | References another element's `nativeID` for form labels |
| `accessibilityLiveRegion` | Announces dynamic changes: `'none'`, `'polite'`, `'assertive'` |
| `importantForAccessibility` | `'auto'`, `'yes'`, `'no'`, `'no-hide-descendants'` |


## Experimental

### `experimental_accessibilityOrder`
Defines focus order for descendant components. Array of `nativeID` values.

```tsx
<View experimental_accessibilityOrder={['title', 'action', 'description']}>
  <Text accessible nativeID="description">Details here</Text>
  <Text accessible nativeID="title">Main Title</Text>
  <Pressable accessible nativeID="action" onPress={act}>
    <Text>Do It</Text>
  </Pressable>
</View>
```

Focus order: title → action → description. Non-listed accessible descendants are excluded from focus.
