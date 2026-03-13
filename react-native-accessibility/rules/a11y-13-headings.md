---
ruleId: A11Y-13
title: Mark section headings for screen reader navigation
---

## [A11Y-13] Mark section headings for screen reader navigation

### Reasoning

Screen reader users navigate long screens by jumping between headings (VoiceOver rotor → Headings). Without heading roles, they must swipe through every element to find sections.

### Incorrect

```tsx
<Text style={styles.sectionTitle}>Payment Methods</Text>
<FlatList data={methods} renderItem={renderMethod} />

<Text style={styles.sectionTitle}>Recent Transactions</Text>
<FlatList data={transactions} renderItem={renderTransaction} />
```

### Correct

```tsx
<Text accessibilityRole="header" style={styles.sectionTitle}>
  Payment Methods
</Text>
<FlatList data={methods} renderItem={renderMethod} />

<Text accessibilityRole="header" style={styles.sectionTitle}>
  Recent Transactions
</Text>
<FlatList data={transactions} renderItem={renderTransaction} />
```

### Guidelines

- Mark screen titles and section titles with `accessibilityRole="header"`
- Do not overuse headings — only main section dividers, not every label
- In `SectionList`, the section header render function should include the heading role
- For screen titles managed by React Navigation, the header is typically accessible by default — verify it is marked as a heading
