---
ruleId: A11Y-10
title: Form inputs need associated labels
---

## [A11Y-10] Form inputs need associated labels

### Reasoning

A `TextInput` without a label is announced as "edit text" or "text field" by screen readers — the user has no idea what to type. Every input needs a descriptive label.

### Incorrect

```tsx
<Text>Email</Text>
<TextInput value={email} onChangeText={setEmail} />
```

The `Text` and `TextInput` are separate accessibility elements — the screen reader does not associate them.

### Correct

```tsx
{/* Option 1: accessibilityLabel on the input */}
<Text>Email</Text>
<TextInput
  value={email}
  onChangeText={setEmail}
  accessibilityLabel="Email"
/>
```

```tsx
{/* Option 2: accessibilityLabelledBy on Android */}
<Text nativeID="emailLabel">Email</Text>
<TextInput
  value={email}
  onChangeText={setEmail}
  accessibilityLabel="Email"
  accessibilityLabelledBy={Platform.OS === 'android' ? 'emailLabel' : undefined}
/>
```

```tsx
{/* Option 3: Wrap label and input as one accessible element */}
<View accessible accessibilityLabel="Email input">
  <Text>Email</Text>
  <TextInput value={email} onChangeText={setEmail} />
</View>
```

### Guidelines

- Always set `accessibilityLabel` on `TextInput`
- For `placeholder` text: don't rely on it as the label — it disappears when the user types
- For password fields: label as "Password", not "Enter your password here"
- For search: use `accessibilityRole="search"` on the input or its container
- For error states: update the label or announce the error separately (see A11Y-6)
