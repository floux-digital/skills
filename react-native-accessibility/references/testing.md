# Testing Accessibility


## Manual Testing

### VoiceOver (iOS)
- **On device:** Settings > Accessibility > VoiceOver > toggle on
- **Shortcut:** Triple-click Side button (configure in Settings > Accessibility > Accessibility Shortcut)
- **Not available in the iOS Simulator.** Always test on a physical device.
- **Xcode Accessibility Inspector:** Use for basic property inspection on macOS, but it does not replicate the VoiceOver experience.

### TalkBack (Android)
- **On device:** Settings > Accessibility > TalkBack > toggle on
- **Shortcut:** Long-press both volume keys for 3 seconds (enable in Settings > Accessibility)
- **Emulator:** TalkBack is not installed by default. Install from Google Play Store (requires emulator with Play Store image).
- **Command line:**
  ```bash
  # Enable TalkBack
  adb shell settings put secure enabled_accessibility_services com.google.android.marvin.talkback/com.google.android.marvin.talkback.TalkBackService

  # Disable TalkBack
  adb shell settings put secure enabled_accessibility_services com.android.talkback/com.google.android.marvin.talkback.TalkBackService
  ```


## What to Verify Manually

1. **Every interactive element** is reachable via swipe navigation
2. **Labels are meaningful** — not "button", "text", or auto-generated gibberish
3. **Roles are announced** — "button", "heading", "checkbox", not just text
4. **State changes are announced** — "selected", "checked", "expanded"
5. **Focus moves logically** after navigation, modal open/close, item deletion
6. **Dynamic content changes** are announced (toasts, errors, counters)
7. **Touch targets** are large enough to tap reliably (44x44)
8. **No focus traps** — user can always navigate away (except intentional modal traps)
9. **Escape gesture** dismisses modals on iOS


## Automated Testing

### React Native Testing Library

```tsx
import { render, screen } from '@testing-library/react-native';

test('delete button is accessible', () => {
  render(<DeleteButton />);

  const button = screen.getByRole('button', { name: 'Delete message' });
  expect(button).toBeTruthy();
  expect(button.props.accessibilityState?.disabled).toBe(false);
});

test('checkbox communicates state', () => {
  render(<TermsCheckbox checked={true} />);

  const checkbox = screen.getByRole('checkbox', { name: 'Accept terms' });
  expect(checkbox.props.accessibilityState?.checked).toBe(true);
});
```

### Jest Accessibility Assertions

```tsx
// Verify label exists
expect(element.props.accessibilityLabel).toBeDefined();
expect(element.props.accessibilityLabel).not.toBe('');

// Verify role
expect(element.props.accessibilityRole).toBe('button');

// Verify state
expect(element.props.accessibilityState).toEqual(
  expect.objectContaining({ disabled: false })
);
```

### What to Test Automatically
- Presence of `accessibilityLabel` on interactive elements
- Correct `accessibilityRole` assignments
- `accessibilityState` reflects component props
- `accessibilityValue` updates with value changes
- Decorative elements have `aria-hidden` or equivalent


## CI Integration

### Accessibility Linting

Use `eslint-plugin-react-native-a11y` to catch common issues:

```bash
npm install --save-dev eslint-plugin-react-native-a11y
```

```json
{
  "plugins": ["react-native-a11y"],
  "extends": ["plugin:react-native-a11y/all"]
}
```

Key rules:
- `has-accessibility-props` — touchables need accessibility props
- `has-valid-accessibility-role` — role values are valid
- `no-nested-touchables` — no touchables inside touchables
- `has-valid-accessibility-state` — state values are valid
