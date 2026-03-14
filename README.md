# React Native Accessibility — Agent Skill

Helps AI coding assistants write accessible React Native code. 10 rules covering labels, roles, states, focus, touch targets, announcements, modals, motion, color, and custom controls. 5 reference docs for props, screen readers, platform differences, focus management, and testing.

Built on patterns from the [React Native docs](https://reactnative.dev/docs/accessibility) and production apps.

## Install for Claude Code

```bash
# Clone into your project
git clone https://github.com/rushatgabhane/react-native-accessibility-skill.git /tmp/rn-a11y-skill
cp -r /tmp/rn-a11y-skill/react-native-accessibility .claude/skills/react-native-accessibility
rm -rf /tmp/rn-a11y-skill
```

For personal use (all projects):

```bash
cp -r /tmp/rn-a11y-skill/react-native-accessibility ~/.claude/skills/react-native-accessibility
```

## Use

```
/react-native-accessibility
```

The skill also activates automatically when writing React Native UI code.

## Other AI Tools

This skill follows the [Agent Skills](https://agentskills.io) open standard and works with Cursor, Codex, Gemini, and other compatible tools.

## Contributing

Keep markdown concise — there is a token cost to loading skills. Focus on what LLMs get wrong. Include incorrect/correct code examples.

## License

MIT
