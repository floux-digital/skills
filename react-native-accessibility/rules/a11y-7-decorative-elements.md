---
ruleId: A11Y-7
title: Hide decorative elements from screen readers
---

## [A11Y-7] Hide decorative elements from screen readers

### Reasoning

Decorative images, dividers, background patterns, and icons that duplicate adjacent text clutter screen reader navigation. Every extra element a user has to swipe through slows them down.

### Incorrect

```tsx
<View style={styles.row}>
  <MailIcon />
  <Text>Email</Text>
</View>
```

Screen reader reads: "Image" then "Email" — the icon announcement is meaningless.

### Correct

```tsx
<View style={styles.row}>
  <MailIcon aria-hidden={true} />
  <Text>Email</Text>
</View>
```

Screen reader reads: "Email" — clean.

### What to Hide

- Icons next to text that already describes the same thing
- Background images and patterns
- Decorative dividers and separators
- Animated decorations (confetti, loading shimmer placeholders)
- Brand logos that are not interactive

### How to Hide

```tsx
{/* Cross-platform (preferred) */}
<View aria-hidden={true}>{/* decorative content */}</View>

{/* iOS-specific */}
<View accessibilityElementsHidden={true}>{/* decorative content */}</View>

{/* Android-specific */}
<View importantForAccessibility="no-hide-descendants">{/* decorative content */}</View>
```

### What NOT to Hide

- Icons that are the only indicator of meaning (status icons without text)
- Images that convey information (charts, photos, maps)
- Interactive elements of any kind
