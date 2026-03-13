# React Native Accessibility — Agent Skill for Claude Code

An agent skill that helps AI coding assistants write accessible React Native code from the start. Covers screen reader labels, roles, states, focus management, touch targets, dynamic announcements, and platform-specific patterns for iOS (VoiceOver) and Android (TalkBack).

Built on patterns from the [React Native Accessibility docs](https://reactnative.dev/docs/accessibility) and real-world accessibility implementations in production React Native apps.

## Installing

### Claude Code

```bash
npx skills add https://github.com/rushatgabhane/react-native-accessibility-skill --skill react-native-accessibility
```

Or clone and install manually:

```bash
# Project-level (recommended)
git clone https://github.com/rushatgabhane/react-native-accessibility-skill.git
cp -r react-native-accessibility-skill/react-native-accessibility .claude/skills/react-native-accessibility

# Or personal (all projects)
cp -r react-native-accessibility-skill/react-native-accessibility ~/.claude/skills/react-native-accessibility
```

### Other AI Tools

The skill follows the [Agent Skills](https://agentskills.io) open standard and works with Cursor, Codex, Gemini, and other compatible tools.

## Using

The skill activates automatically when writing React Native UI code. You can also invoke it directly:

**Claude Code:**
```
/react-native-accessibility
```

**With arguments:**
```
/react-native-accessibility Focus on form accessibility
/react-native-accessibility Check this modal for focus trapping
```

**Natural language:**
```
Use the react-native-accessibility skill to check my component
```

## What's Included

### 16 Rules

| ID | Rule |
|---|---|
| A11Y-1 | All interactive elements need accessible labels |
| A11Y-2 | Images need alt text or decorative marking |
| A11Y-3 | Assign correct accessibility roles |
| A11Y-4 | Communicate component state to assistive tech |
| A11Y-5 | Enforce minimum 44x44 touch targets |
| A11Y-6 | Announce dynamic content changes |
| A11Y-7 | Hide decorative elements from screen readers |
| A11Y-8 | Manage focus after navigation and content changes |
| A11Y-9 | Respect reduced motion preferences |
| A11Y-10 | Form inputs need associated labels |
| A11Y-11 | Do not rely on color alone |
| A11Y-12 | Support dynamic font scaling |
| A11Y-13 | Mark section headings for navigation |
| A11Y-14 | Trap focus within modals and overlays |
| A11Y-15 | Support iOS escape and magic tap gestures |
| A11Y-16 | Implement accessibility actions for custom controls |

Each rule includes reasoning, incorrect/correct code examples, and guidelines.

### 5 References

| File | Content |
|---|---|
| `references/props.md` | Complete reference of all React Native accessibility props |
| `references/screen-readers.md` | VoiceOver and TalkBack behavior, gestures, announcements |
| `references/focus-management.md` | Focus order, trapping, and programmatic focus control |
| `references/platform.md` | iOS vs Android differences and cross-platform patterns |
| `references/testing.md` | Testing on devices, emulators, and in CI |

## Contributing

Contributions welcome! When adding or editing rules:

- Keep markdown concise — there is a token cost to loading skills
- Focus on what LLMs get wrong, not basics they already know
- Include incorrect and correct code examples
- Test your examples for correctness

## License

MIT
