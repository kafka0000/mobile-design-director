# Interaction & Motion Physics

A static design is a dead design. This guide defines *how it feels* across all mobile platforms.

---

## 1. Spring System — The Only Easing You Need

Avoid `ease-in-out`. Use spring dynamics for natural motion.

### Parameter Presets

| Preset | Damping | Stiffness | Mass | Use Case |
|--------|---------|-----------|------|----------|
| **Snappy** | 15 | 400 | 0.8 | Button press, toggle, chip select |
| **Gentle** | 20 | 150 | 1.0 | Card expand, modal present |
| **Bouncy** | 10 | 300 | 0.6 | Playful reveals, celebrations |
| **Heavy** | 25 | 200 | 1.5 | Drawer slide, sheet dismiss |
| **Micro** | 18 | 500 | 0.5 | Scale feedback, press state |

### Platform Implementations

**SwiftUI:**
```swift
// Snappy
.animation(.spring(response: 0.3, dampingFraction: 0.7), value: isPressed)

// Gentle
.animation(.spring(response: 0.5, dampingFraction: 0.8), value: isExpanded)

// Bouncy
.animation(.spring(response: 0.4, dampingFraction: 0.5), value: showCelebration)
```

**Flutter:**
```dart
// Snappy
final controller = AnimationController(duration: Duration(milliseconds: 300));
final animation = CurvedAnimation(parent: controller, curve: Curves.easeOutBack);

// Spring simulation
final spring = SpringSimulation(
  SpringDescription(mass: 0.8, stiffness: 400, damping: 15),
  0, 1, 0, // start, end, velocity
);
```

**Jetpack Compose:**
```kotlin
// Snappy
val scale by animateFloatAsState(
    targetValue = if (isPressed) 0.96f else 1f,
    animationSpec = spring(dampingRatio = 0.7f, stiffness = 400f)
)
```

**React Native (Reanimated):**
```typescript
import { withSpring } from 'react-native-reanimated';

const snappy = withSpring(value, { damping: 15, stiffness: 400, mass: 0.8 });
const gentle = withSpring(value, { damping: 20, stiffness: 150, mass: 1.0 });
```

---

## 2. Haptic Feedback Mapping

| Interaction | Level | Description |
|-------------|-------|-------------|
| Toggle/Switch | Light | Subtle confirmation |
| Button tap | Medium | Standard action feedback |
| Success | Success | Task completed |
| Error/Warning | Heavy + Error | Red flag moment |
| Long press engage | Heavy | "Locked in" moment |
| Scroll snap | Selection | Detent/tick feel |
| Delete action | Warning | Destructive action warning |

### Platform APIs

| Platform | API |
|----------|-----|
| iOS (SwiftUI) | `UIImpactFeedbackGenerator(style: .medium).impactOccurred()` |
| iOS (React Native) | `Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium)` |
| Android (Compose) | `view.performHapticFeedback(HapticFeedbackConstants.CONFIRM)` |
| Flutter | `HapticFeedback.mediumImpact()` |

---

## 3. Choreography — Staggered Entrance

Elements must enter sequentially, never all at once.

### The Stagger Pattern

```
Element 1:    0ms   ████████████
Element 2:   50ms       ████████████
Element 3:  100ms           ████████████
Element 4:  150ms               ████████████
```

- **Stagger delay:** 50ms per element (max 5, then batch)
- **Direction:** Top→Bottom for lists, Center→Out for grids
- **Duration:** Each element's animation = 300-400ms

### Platform Implementations

**SwiftUI:**
```swift
ForEach(Array(items.enumerated()), id: \.element.id) { index, item in
    ItemCard(item: item)
        .transition(.move(edge: .bottom).combined(with: .opacity))
        .animation(.spring(response: 0.4, dampingFraction: 0.7)
            .delay(Double(index) * 0.05), value: isVisible)
}
```

**Flutter:**
```dart
AnimatedList(
  itemBuilder: (context, index, animation) {
    return SlideTransition(
      position: Tween<Offset>(begin: Offset(0, 0.3), end: Offset.zero)
          .animate(CurvedAnimation(parent: animation, curve: Curves.easeOut)),
      child: FadeTransition(opacity: animation, child: ItemCard(items[index])),
    );
  },
)
```

**Jetpack Compose:**
```kotlin
items.forEachIndexed { index, item ->
    AnimatedVisibility(
        enter = fadeIn(animationSpec = tween(300, delayMillis = index * 50))
             + slideInVertically(initialOffsetY = { it / 4 })
    ) { ItemCard(item) }
}
```

**React Native (Reanimated):**
```typescript
{items.map((item, i) => (
  <Animated.View
    key={item.id}
    entering={FadeInDown.delay(i * 50).springify().damping(15)}
  >
    <ItemCard {...item} />
  </Animated.View>
))}
```

---

## 4. Scale-on-Press (The Premium Touch)

Every tappable element should respond with a scale effect.

### The Formula

```
Rest:       scale(1.0)
Pressed:    scale(0.96)  — subtle, professional
Released:   spring back to scale(1.0)
```

### Scale Values by Element

| Element | Press Scale | Notes |
|---------|------------|-------|
| Cards | 0.97 | Subtle, large surface |
| Buttons | 0.95 | Slightly more pronounced |
| Icons | 0.90 | Small target, more feedback needed |
| List items | 0.98 | Very subtle |
| Tab bar items | 0.92 | Medium emphasis |

---

## 5. Transition Patterns

### Sheet/Modal Presentation
- **Enter:** Slide up + fade (300ms, gentle spring)
- **Dimming:** Background dims to 40% black over 200ms
- **Dismiss:** Velocity-based — fast flick = dismiss, slow = snap back
- **Reduced motion:** Instant appear/disappear, no animation

### The "Young" Factor — Acid Luxury Touches
- High saturation accents against deep black/white backgrounds
- **Glow effects:** Subtle radial gradient behind accent elements
- **Variable blur:** Background blur increases on scroll (8→20pt)
