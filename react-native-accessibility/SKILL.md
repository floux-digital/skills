---
name: react-native-accessibility
description: Guides writing accessible React Native components — screen reader labels, roles, states, focus management, touch targets, and platform-specific patterns for VoiceOver and TalkBack. Use when writing or modifying React Native UI code.
license: MIT
metadata:
  version: "1.0"
---

When writing or modifying React Native components, follow these accessibility standards. Apply them as you write — do not wait for a separate review step.


## Core Principles

1. Every interactive element must work with VoiceOver (iOS) and TalkBack (Android).
2. Set `accessibilityLabel` on all touchable elements — never rely on auto-constructed labels from child Text nodes.
3. Use `accessibilityRole` (or `role`) to communicate what a component is. Use `accessibilityState` to communicate its current state.
4. Minimum touch target: 44x44 points.
5. iOS and Android have different accessibility props — check `references/platform.md` when unsure.
6. Hide decorative elements from screen readers. Announce dynamic content changes.
7. Manage focus after navigation transitions, modal open/close, and content removal.


## Quick Reference

When writing a component, consult the relevant rules:

### Every Component
- [A11Y-1](rules/a11y-1-accessible-labels.md) — Add labels to all interactive elements
- [A11Y-3](rules/a11y-3-role-assignment.md) — Assign correct roles
- [A11Y-5](rules/a11y-5-touch-target-size.md) — Enforce 44x44 minimum touch targets

### Images & Icons
- [A11Y-2](rules/a11y-2-image-alt-text.md) — Alt text or decorative marking
- [A11Y-7](rules/a11y-7-decorative-elements.md) — Hide decorative elements

### Stateful Components
- [A11Y-4](rules/a11y-4-state-communication.md) — Communicate state (disabled, checked, expanded, etc.)
- [A11Y-16](rules/a11y-16-accessible-actions.md) — Implement increment/decrement and custom actions

### Forms
- [A11Y-10](rules/a11y-10-form-labels.md) — Associate labels with inputs

### Lists & Sections
- [A11Y-13](rules/a11y-13-headings.md) — Mark headings for screen reader navigation

### Dynamic Content
- [A11Y-6](rules/a11y-6-dynamic-announcements.md) — Announce changes to screen readers

### Navigation & Modals
- [A11Y-8](rules/a11y-8-focus-management.md) — Move focus after navigation
- [A11Y-14](rules/a11y-14-modal-focus-trap.md) — Trap focus in modals
- [A11Y-15](rules/a11y-15-escape-gestures.md) — Support iOS escape and magic tap

### Visual Accessibility
- [A11Y-9](rules/a11y-9-reduced-motion.md) — Respect reduced motion
- [A11Y-11](rules/a11y-11-color-contrast.md) — Do not rely on color alone
- [A11Y-12](rules/a11y-12-font-scaling.md) — Support dynamic font scaling


## References

- `references/props.md` — All React Native accessibility props with usage.
- `references/screen-readers.md` — VoiceOver and TalkBack behavior and gestures.
- `references/focus-management.md` — Focus order, trapping, and programmatic focus.
- `references/platform.md` — iOS vs Android prop differences.
- `references/testing.md` — Testing accessibility on devices and in CI.
