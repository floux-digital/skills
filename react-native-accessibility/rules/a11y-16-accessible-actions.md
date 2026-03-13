---
ruleId: A11Y-16
title: Implement accessibility actions for custom controls
---

## [A11Y-16] Implement accessibility actions for custom controls

### Reasoning

Custom controls (sliders, steppers, swipeable rows, long-press menus) often have gestures that screen reader users cannot perform. `accessibilityActions` exposes these interactions to assistive technology.

### Incorrect

```tsx
function QuantityStepper({ value, onChange }) {
  return (
    <View style={styles.stepper}>
      <Pressable onPress={() => onChange(value - 1)}><Text>-</Text></Pressable>
      <Text>{value}</Text>
      <Pressable onPress={() => onChange(value + 1)}><Text>+</Text></Pressable>
    </View>
  );
}
```

Screen reader users encounter three separate elements instead of one adjustable control.

### Correct

```tsx
function QuantityStepper({ value, min = 0, max = 99, onChange }) {
  return (
    <View
      accessible
      accessibilityRole="adjustable"
      accessibilityLabel={`Quantity: ${value}`}
      accessibilityValue={{ min, max, now: value }}
      accessibilityActions={[
        { name: 'increment', label: 'Increase quantity' },
        { name: 'decrement', label: 'Decrease quantity' },
      ]}
      onAccessibilityAction={(event) => {
        switch (event.nativeEvent.actionName) {
          case 'increment':
            if (value < max) onChange(value + 1);
            break;
          case 'decrement':
            if (value > min) onChange(value - 1);
            break;
        }
      }}
    >
      <Pressable
        onPress={() => onChange(Math.max(min, value - 1))}
        aria-hidden={true}
      >
        <Text>-</Text>
      </Pressable>
      <Text aria-hidden={true}>{value}</Text>
      <Pressable
        onPress={() => onChange(Math.min(max, value + 1))}
        aria-hidden={true}
      >
        <Text>+</Text>
      </Pressable>
    </View>
  );
}
```

### Common Custom Actions

| Control | Actions to Implement |
|---|---|
| Stepper / quantity | `increment`, `decrement` |
| Swipeable list row | Custom actions for swipe reveals (delete, archive, pin) |
| Star rating | `increment`, `decrement` |
| Media player | Custom action for play/pause, or use `onMagicTap` |
| Drag-to-reorder | Custom actions for "Move up" / "Move down" |
| Long-press menu | Custom actions for each menu item |

### Guidelines

- Wrap multi-element custom controls in a single `accessible` container
- Hide the internal sub-elements with `aria-hidden={true}`
- Use `increment`/`decrement` for value-based controls
- Use named custom actions for context menu items
- Set `accessibilityValue` with `min`, `max`, `now` for range controls
- Provide `label` on custom actions — it's what VoiceOver reads in the Actions rotor
