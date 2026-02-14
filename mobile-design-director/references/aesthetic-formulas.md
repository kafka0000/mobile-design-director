# Aesthetic Formulas & The Science of "Premium"

When the user asks for "High-End," "Premium," or "Modern," apply these theorems.

---

## 1. The Law of Visual Silence

**Theorem:** `Premium = Space / Information`
Luxury is defined by what you *remove*, not what you add.

### Spacing System (8pt Grid)

| Element | Standard | Premium | Ultra-Premium |
|---------|----------|---------|---------------|
| Horizontal margin | 16pt | 24pt | 32pt |
| Card padding | 12pt | 16pt | 20pt |
| Section spacing | 24pt | 32pt | 48pt |
| Grid system | 8pt | 8pt strict | 8pt strict |

- **Negative Space:** Treat whitespace as an active element, not empty space.
- **Information Density:** Max 3 visual "zones" per viewport in luxury mode.
- **The Breath Rule:** Every content block needs a "breath" (min 24pt vertical gap).

### Platform Spacing Implementations

**SwiftUI:**
```swift
VStack(spacing: 32) { // Premium section spacing
    content
}
.padding(.horizontal, 24) // Premium horizontal margin
```

**Flutter:**
```dart
Padding(
  padding: EdgeInsets.symmetric(horizontal: 24, vertical: 32),
  child: content,
)
```

**Jetpack Compose:**
```kotlin
Column(
    modifier = Modifier.padding(horizontal = 24.dp),
    verticalArrangement = Arrangement.spacedBy(32.dp)
) { content() }
```

**React Native (NativeWind):**
```tsx
<View className="px-6 gap-8">{/* 24pt / 32pt */}</View>
```

---

## 2. Typographic Confidence

**Theorem:** Small text + wide spacing = Sophistication. Large text + tight spacing = Power.

### Scale System (Perfect Fourth — 1.333 ratio)

```
Caption:     11pt
Body:        14pt
Subhead:     18pt → 14 × 1.333
Title:       24pt → 18 × 1.333
Display:     32pt → 24 × 1.333
Hero:        42pt → 32 × 1.333
```

### Tracking Rules

| Type | Tracking | Weight | Case |
|------|----------|--------|------|
| Display/Headlines | -1% to -3% | Black/Heavy (800-900) | Sentence case |
| Subheadings | -0.5% | Semibold (600) | Sentence case |
| Body | 0% (default) | Regular (400) | Sentence case |
| Captions/Overlines | +10% to +15% | Medium (500) | ALL CAPS |
| Buttons | +5% | Semibold (600) | ALL CAPS or Title case |

### Platform Font Choices

| Platform | System Font | Premium Alternative |
|----------|------------|-------------------|
| iOS | SF Pro Display / Text | Inter, Outfit |
| Android | Roboto | Google Sans, Inter |
| Flutter | Platform-adaptive | Inter (custom) |
| Cross-platform | System default | Inter, Outfit, Manrope |

---

## 3. Depth & Materiality (Glassmorphism 2.0)

**Theorem:** Objects must feel tactile.

### Micro-Borders (Dark Mode)
```
Border:      0.5px solid rgba(255, 255, 255, 0.10)
Inner glow:  inset 0 0.5px 0 rgba(255, 255, 255, 0.05)
```

### Layered Shadows
```
Ambient:     0 16px 48px rgba(0, 0, 0, 0.12)   — wide, soft
Contact:     0 2px 8px rgba(0, 0, 0, 0.24)      — tight, dark
Combined:    Both layers for premium depth
```

### Corner Radii

| Element | Radius | Notes |
|---------|--------|-------|
| Cards | 16-20pt | Use continuous curvature (Squircle) where possible |
| Buttons | 12pt | Match card inner radius |
| Chips/Tags | height/2 | Full pill shape |
| Input fields | 10pt | Slightly subdued vs buttons |
| Modal/Sheet | 24pt (top) | iOS/Android sheet pattern |

### Squircle Availability

| Platform | Support | Implementation |
|----------|---------|---------------|
| iOS (SwiftUI) | ✅ Native | `.clipShape(.rect(cornerRadius: 16, style: .continuous))` |
| iOS (React Native) | ✅ iOS 16+ | `borderCurve: 'continuous'` |
| Android (Compose) | ⚠️ Custom | `RoundedCornerShape(16.dp)` (standard only) |
| Flutter | ⚠️ Custom | `ContinuousRectangleBorder(borderRadius: 16)` |

---

## 4. Color Psychology for "Young Luxury"

### The Tension Palette

```
Base Layer:     Deep blacks (#0A0A0A) or pure whites (#FAFAFA)
Content Layer:  Neutral grays (#1A1A1A dark / #F5F5F5 light)
Accent:         High saturation "Acid" tones
                Electric Blue: #0066FF
                Acid Green:    #00FF88
                Warm Gold:     #FFB800
Surface Glass:  rgba(255,255,255, 0.06) on dark
                rgba(0,0,0, 0.02) on light
```

### Usage Rules
- **Max 1 accent color per screen** for impact
- **Accent only on CTAs and key metrics** — never on decorative elements
- **Gradients:** Subtle, max 2 stops, same hue family
