---
ruleId: A11Y-11
title: Do not rely on color alone to convey information
---

## [A11Y-11] Do not rely on color alone to convey information

### Reasoning

Users with color vision deficiencies cannot distinguish information conveyed only through color. WCAG 2.1 requires at least 4.5:1 contrast ratio for normal text and 3:1 for large text.

### Incorrect

```tsx
<View style={{ backgroundColor: isError ? 'red' : 'green' }}>
  <Text style={{ color: 'white' }}>{status}</Text>
</View>
```

A color-blind user cannot tell if the status is error or success.

### Correct

```tsx
<View style={{ backgroundColor: isError ? '#D32F2F' : '#388E3C' }}>
  <Text style={{ color: 'white' }}>
    {isError ? 'Error: ' : 'Success: '}{status}
  </Text>
  {isError && <WarningIcon aria-hidden={true} />}
</View>
```

### Guidelines

- Add text labels, icons, or patterns alongside color indicators
- Error states: include "Error" text or a warning icon, not just red color
- Success states: include "Success" text or a checkmark icon
- Form validation: pair red borders with error text below the input
- Charts and graphs: use patterns or labels in addition to color
- Selected states: use a border, checkmark, or bold text in addition to highlight color
- Minimum contrast ratios:
  - Normal text (< 18pt): **4.5:1**
  - Large text (>= 18pt bold or >= 24pt): **3:1**
  - UI components and graphical objects: **3:1**
