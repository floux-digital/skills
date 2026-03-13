---
ruleId: A11Y-9
title: Respect reduced motion preferences
---

## [A11Y-9] Respect reduced motion preferences

### Reasoning

Users with vestibular disorders, motion sensitivity, or seizure conditions enable "Reduce Motion" in system settings. Ignoring this can cause nausea, dizziness, or seizures.

### Incorrect

```tsx
function FadeInCard({ children }) {
  const opacity = useSharedValue(0);

  useEffect(() => {
    opacity.value = withSpring(1);
  }, []);

  return <Animated.View style={{ opacity }}>{children}</Animated.View>;
}
```

### Correct

```tsx
import { AccessibilityInfo } from 'react-native';

function FadeInCard({ children }) {
  const opacity = useSharedValue(0);
  const [reduceMotion, setReduceMotion] = useState(false);

  useEffect(() => {
    AccessibilityInfo.isReduceMotionEnabled().then(setReduceMotion);
    const sub = AccessibilityInfo.addEventListener(
      'reduceMotionChanged',
      setReduceMotion,
    );
    return () => sub.remove();
  }, []);

  useEffect(() => {
    opacity.value = reduceMotion ? 1 : withSpring(1);
  }, [reduceMotion]);

  return <Animated.View style={{ opacity }}>{children}</Animated.View>;
}
```

### With Reanimated's `useReducedMotion`

```tsx
import { useReducedMotion } from 'react-native-reanimated';

function FadeInCard({ children }) {
  const reduceMotion = useReducedMotion();
  const opacity = useSharedValue(0);

  useEffect(() => {
    opacity.value = reduceMotion ? 1 : withSpring(1);
  }, [reduceMotion]);

  return <Animated.View style={{ opacity }}>{children}</Animated.View>;
}
```

### Guidelines

- Replace motion-based animations with opacity or instant transitions when reduce motion is enabled
- Auto-playing animations should stop or use a simpler alternative
- Parallax scrolling effects should be disabled
- Page transition animations should use cross-fade instead of slide
- Loading spinners are generally acceptable (small, contained motion)
