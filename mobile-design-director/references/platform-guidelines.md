# Platform Design Guidelines

Quick reference for cross-platform mobile design decisions.

---

## 1. Apple HIG — Core Touch Points

### Touch Targets & Safe Areas

| Element | Minimum | Recommended |
|---------|---------|-------------|
| Buttons | 44×44pt | 48×48pt |
| Icons (tappable) | 44×44pt | 44×44pt (expand hit area) |
| List rows | 44pt height | 52-60pt |
| Tab bar items | 48×49pt | Standard |

| Device Feature | Inset |
|---------------|-------|
| Status bar | 54pt (Dynamic Island) / 44pt |
| Home indicator | 34pt bottom |
| Navigation bar | 96pt total |

### iOS Navigation Patterns

| Pattern | When | Compliance |
|---------|------|-----------|
| Tab bar | 3-5 top-level sections | ✅ Strongly recommended |
| Push navigation | Drill-down hierarchy | ✅ Standard |
| Modal/Sheet | Focused task, creation | ✅ Use bottom sheet |
| Swipe-back | Return to previous | ✅ Never disable |

---

## 2. Material Design 3 — Android Best Practices

### Touch Targets

| Element | Minimum | Recommended |
|---------|---------|-------------|
| All interactive | 48×48dp | 48×48dp |
| FAB | 56×56dp | Standard |
| Navigation items | 48×48dp | Standard |

### Key Patterns

| Pattern | When | Notes |
|---------|------|-------|
| Navigation bar (bottom) | 3-5 destinations | M3 standard |
| Navigation rail | Tablets/landscape | M3 responsive |
| FAB | Primary creation action | Bottom-right, optional |
| Top app bar | Screen title + actions | Collapsible recommended |

---

## 3. iOS vs Android — Decision Matrix

| Aspect | iOS (HIG) | Android (M3) |
|--------|-----------|------------|
| Corner radius | Continuous (Squircle) | Standard rounded |
| Primary action | Bottom-right or inline | FAB or inline |
| Back navigation | Swipe from left edge | System back button/gesture |
| Navigation | Tab bar (bottom) | Navigation bar (bottom) |
| Typography | SF Pro | Roboto / Material type scale |
| Elevation | Shadows + blur | Tonal elevation (color shift) |
| Sheets | Pull-to-dismiss | Handle + drag |
| Switches | Green/gray toggle | Track + thumb |
| Status bar | Light/dark adaptation | Edge-to-edge |

### Cross-Platform Rules

```
✅ Use for both:  Bottom navigation, bottom sheets, cards
⚠️ Platform-adapt: Back gesture, status bar, system fonts
❌ Avoid mixing:   FAB on iOS, iOS-style swipe-back on Android
```

---

## 4. Multi-Framework Quick Reference

### Premium Card Pattern

**SwiftUI:**
```swift
RoundedRectangle(cornerRadius: 16, style: .continuous)
    .fill(.ultraThinMaterial)
    .overlay(RoundedRectangle(cornerRadius: 16, style: .continuous)
        .stroke(Color.white.opacity(0.1), lineWidth: 0.5))
    .shadow(color: .black.opacity(0.12), radius: 24, y: 8)
    .shadow(color: .black.opacity(0.24), radius: 4, y: 1)
```

**Flutter:**
```dart
Container(
  decoration: BoxDecoration(
    borderRadius: BorderRadius.circular(16),
    color: Colors.white.withOpacity(0.06),
    border: Border.all(color: Colors.white.withOpacity(0.1), width: 0.5),
    boxShadow: [
      BoxShadow(color: Colors.black.withOpacity(0.12), blurRadius: 24, offset: Offset(0, 8)),
      BoxShadow(color: Colors.black.withOpacity(0.24), blurRadius: 4, offset: Offset(0, 1)),
    ],
  ),
)
```

**Jetpack Compose:**
```kotlin
Card(
    shape = RoundedCornerShape(16.dp),
    colors = CardDefaults.cardColors(containerColor = Color.White.copy(alpha = 0.06f)),
    border = BorderStroke(0.5.dp, Color.White.copy(alpha = 0.1f)),
    elevation = CardDefaults.cardElevation(defaultElevation = 8.dp),
)
```

**React Native (NativeWind):**
```tsx
<View className="bg-white/5 rounded-2xl border border-white/10
                  shadow-lg shadow-black/10 p-5" />
```

---

## 5. Accessibility Non-Negotiables

These are **not optional**, regardless of platform or design approach:

| Rule | Minimum | Recommendation |
|------|---------|----------------|
| Color contrast (text) | 4.5:1 (AA) | 7:1 (AAA) |
| Color contrast (large text) | 3:1 | 4.5:1 |
| Touch target | 44pt (iOS) / 48dp (Android) | Larger is always better |
| Focus indicators | Visible | 2px+ solid ring |
| Motion | Respect reduced-motion settings | Always |
| Screen reader | All interactive elements labeled | Platform-specific APIs |
| Text scaling | Support Dynamic Type / Font Scale | Up to 2× |

### Platform Accessibility APIs

| Need | SwiftUI | Flutter | Compose | React Native |
|------|---------|---------|---------|-------------|
| Label | `.accessibilityLabel()` | `Semantics(label:)` | `contentDescription` | `accessibilityLabel` |
| Role | `.accessibilityAddTraits()` | `Semantics(button:)` | `Role.Button` | `accessibilityRole` |
| Hint | `.accessibilityHint()` | `Semantics(hint:)` | `stateDescription` | `accessibilityHint` |
| Hide decorative | `.accessibilityHidden(true)` | `ExcludeSemantics` | `invisibleToUser()` | `importantForAccessibility="no"` |
