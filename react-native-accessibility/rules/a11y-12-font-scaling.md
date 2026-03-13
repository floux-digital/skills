---
ruleId: A11Y-12
title: Support dynamic font scaling
---

## [A11Y-12] Support dynamic font scaling

### Reasoning

Users with low vision increase font size in system settings. Fixed font sizes ignore this preference. React Native supports dynamic scaling by default — do not disable it.

### Incorrect

```tsx
<Text style={{ fontSize: 14 }} allowFontScaling={false}>
  Important information
</Text>
```

```tsx
<View style={{ height: 48 }}>
  <Text style={{ fontSize: 16 }}>This text will get clipped at large font sizes</Text>
</View>
```

### Correct

```tsx
<Text style={{ fontSize: 14 }}>
  Important information
</Text>
```

React Native scales fonts by default — just don't disable it.

```tsx
{/* Use flexible height instead of fixed */}
<View style={{ minHeight: 48, paddingVertical: 12 }}>
  <Text style={{ fontSize: 16 }}>This text wraps at large font sizes</Text>
</View>
```

### Guidelines

- Never set `allowFontScaling={false}` unless there is a genuine layout constraint (e.g., a fixed-size badge)
- If you must cap scaling, use `maxFontSizeMultiplier` instead of disabling entirely
- Use `minHeight` instead of fixed `height` for containers with text
- Use `paddingVertical` for vertical spacing instead of fixed heights
- Test with the largest font size setting on both iOS and Android
- Avoid truncating text at large sizes — prefer wrapping or scrolling
- `numberOfLines` is acceptable for list items, but ensure the full text is accessible via `accessibilityLabel`
