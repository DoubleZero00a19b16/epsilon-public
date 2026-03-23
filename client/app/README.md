
# MÜTLƏQ OXU!

***APP apk şəkildə deploy olunub, siz apk'nı əsas folder directory'dən apk folder'ına keçid edərək tapa bilərsiniz.***

# Contributer Aziz: https://github.com/AzizAlizade473

# Epsilon – OBA Market Mobile Client

> **Retail Intelligence, Gamified.** A React Native / Expo app that transforms supermarket receipts into structured, real-time product feedback — the consumer-facing layer of Epsilon's B2B2C data platform, built for OBA Market (Veysəloğlu Group).

---

## What This App Does

Epsilon's mobile client is the data collection engine of a larger retail intelligence platform. The app gamifies the post-purchase experience: users scan their loyalty card barcode, their receipt syncs automatically, and they rate the products they bought. Every rating feeds Epsilon's AI clustering backend, which gives retail managers real-time **Stop / Scale / Export** decisions on product lines.

The reward system is intentional: standard ratings earn micro-rewards in AZN, while ratings on **NEW** products earn 2× — accelerating feedback collection on exactly the items management cares about most. A **Trust Score** (called "Reliability" in the UI) tracks rating quality per user and directly scales their reward multiplier, making spam economically self-defeating.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | React Native (Expo SDK) |
| Language | TypeScript (strict) |
| Navigation | Expo Router — file-based routing |
| Animations | React Native Animated API + react-native-reanimated |
| Icons | FontAwesome5 via `@expo/vector-icons` |
| Vector Graphics | react-native-svg |
| Fonts | `@expo-google-fonts/libre-barcode-39` (barcode rendering) |
| Localization | Custom i18n module (`useTranslation` hook) |
| Haptics | `expo-haptics` (iOS light impact on tab press) |
| In-app Browser | `expo-web-browser` |
| Safe Area | `react-native-safe-area-context` |
| Build | EAS Build (Expo Application Services) |
| Linting | ESLint with custom config |

---

## Project Structure

```
client/app/
├── App.tsx                        # Entry point — renders <OBAApp />
├── app/
│   ├── _layout.tsx                # Root layout: Stack navigator, dark status bar
│   └── (tabs)/
│       ├── index.tsx              # Tab entry — delegates to App.tsx
│       └── explore.tsx            # Expo default explore screen
├── components/
│   ├── OBAApp.tsx                 # Root orchestrator — all state, modals, screens
│   ├── NavBar.tsx                 # Floating pill bottom nav with QR center button
│   ├── StarRow.tsx                # 1–5 star rating input
│   ├── Toast.tsx                  # Animated success/error toast
│   ├── BarcodeModal.tsx           # Loyalty card barcode display
│   ├── FeedbackModal.tsx          # Post-rating feedback: reason chips + comment
│   ├── InstructionsModal.tsx      # Onboarding: 3-step how-it-works modal
│   ├── NotificationsPanel.tsx     # Right-side slide-in notifications drawer
│   ├── ProfileModal.tsx           # Edit name, surname, phone
│   ├── screens/
│   │   ├── AuthScreen.tsx         # Login / Register
│   │   ├── HomeScreen.tsx         # Receipt list, balance, rate products
│   │   ├── ProductsScreen.tsx     # Paginated product catalog with search/filter
│   │   ├── Top10Screen.tsx        # Top-rated products leaderboard
│   │   └── SettingsScreen.tsx     # Profile, language, notifications, logout
│   └── ui/                        # Shared primitives (collapsible, icon-symbol, etc.)
├── hooks/
│   ├── useAppState.ts             # All app-level state: auth, tabs, data fetching
│   └── useRating.ts               # Rating flow: star input → feedback modal → submit
├── constants/
│   └── colors.ts                  # Brand color tokens (C.green, C.lime, C.black…)
├── styles/
│   └── styles.ts                  # Global StyleSheet (s.header, s.toast, s.card…)
├── types/
│   └── index.ts                   # Shared TypeScript types (Lang, User, Order…)
├── i18n/
│   └── useTranslation.ts          # Multi-language hook (AZ / EN) + TranslationDict type
└── assets/images/                 # App icons and image assets
```

---

## Architecture

The app follows a **single root orchestrator pattern**. `OBAApp.tsx` is the central component — it owns all state and wires everything together. Two custom hooks handle the two biggest concerns independently:

**`useAppState`** manages everything at the application level: authentication flow, active tab, data loading, receipt/product lists, modal visibility flags, toast queue, notifications, and the pull-to-refresh cycle.

**`useRating`** manages the entire rating interaction flow as a self-contained state machine: capturing which product is being rated, opening the star input, transitioning to the feedback modal (reason chips + free-text comment), submitting the rating, updating the user's balance and reliability score, and triggering the success toast.

Screens receive only the props they need via explicit prop drilling — there is no global state library, keeping the data flow predictable and traceable.

```
App.tsx
  └── OBAApp.tsx  (orchestrator)
        ├── useAppState()       → auth, data, tabs, modals, notifications
        ├── useRating()         → star input → feedback → submit → toast
        ├── AuthScreen          → shown when currentView === 'auth'
        ├── ScrollView
        │     ├── HomeScreen    → activeTab === 'home'
        │     ├── ProductsScreen→ activeTab === 'products'
        │     ├── Top10Screen   → activeTab === 'top10'
        │     └── SettingsScreen→ activeTab === 'settings'
        ├── NavBar              → floating pill, triggers tab changes + barcode modal
        └── Modals (layered)
              ├── BarcodeModal
              ├── FeedbackModal
              ├── NotificationsPanel
              ├── InstructionsModal
              ├── ProfileModal
              └── ReliabilityInfoModal
```

---

## Key Components

### `OBAApp.tsx` — Root Orchestrator
The single source of truth. Loads custom fonts (LibreBarcode39 for the loyalty card barcode), guards the auth wall, renders the active screen inside a `ScrollView` with pull-to-refresh, mounts all modals as a layer stack, and renders the floating `NavBar`. The Reliability badge in the header is interactive — tapping it opens an explanatory modal with an inline SVG shield icon built with `react-native-svg`.

### `NavBar.tsx` — Floating Pill Navigation
A fixed-position pill bar (`position: absolute, bottom: 32`) with four tab icons (Home, Search, Star, User) and a raised circular QR button in the center (`marginTop: -40` to lift it above the bar). Active tab icons highlight in brand lime (`#D4F238`); inactive icons render at 50% white opacity. Tab buttons trigger haptic feedback on iOS via `expo-haptics`.

### `FeedbackModal.tsx` — Post-Rating Feedback Sheet
A bottom sheet modal (`justifyContent: 'flex-end'`) with a custom spring enter animation — three `Animated` values (translateY, scale, opacity) running in parallel via `Animated.parallel`. Contains selectable reason chips (wrap layout, toggles between white/brand-black styles on selection) and a free-text comment field. The `useEnterAnimation` hook is defined locally and reused across multiple modals with identical spring physics.

### `NotificationsPanel.tsx` — Slide-in Drawer
A right-side panel that slides in from off-screen using `Animated.spring` on `translateX`, starting at `Dimensions.get('window').width`. Notification cards show a colored dot (lime `#D4F238` for reward type, blue `#3B82F6` for info type) with title, message, and timestamp. Tapping the semi-transparent backdrop closes it.

### `InstructionsModal.tsx` — Onboarding Steps
A centered card modal showing the 3-step how-it-works flow. Each step has a colored rounded square number badge with step-specific background colors (green-50 for step 1, orange-50 for step 2, lime/20 for step 3). Uses the same `useEnterAnimation` spring pattern as `FeedbackModal`.

### `StarRow.tsx` — Rating Input
Minimal and reusable — renders five star characters, coloring filled stars in brand lime (`#D4F238`) and empty ones in light gray (`#E5E7EB`). Accepts a `disabled` prop to lock the input after submission. Designed to embed inline within product cards.

### `Toast.tsx` — Animated Feedback Toast
A non-interactive overlay (`pointerEvents="none"`) that fades in/out using `Animated.timing` on opacity with a 200ms duration. Driven entirely by a `visible` boolean from parent state — the parent controls timing, the Toast just animates.

### `ProfileModal.tsx` — Edit Profile
Keyboard-aware (`KeyboardAvoidingView` with platform-specific `padding` on iOS and `height` on Android) modal for editing name, surname, and phone number. Syncs local form state from the `user` prop via `useEffect` so it always reflects current saved data when reopened. Calls `showToast` on successful save.

### `BarcodeModal.tsx` — Loyalty Card Display
Renders the user's loyalty card barcode using the `LibreBarcode39_400Regular` Google Font — the card number digits are rendered as styled text in the barcode font. This approach requires no native barcode library, keeping the dependency footprint small while producing a scannable barcode.

---

## Animation System

All modals share the same `useEnterAnimation` hook pattern, defined locally in each modal file:

```typescript
function useEnterAnimation(trigger: boolean) {
  const translateY = useRef(new Animated.Value(10)).current;
  const scale     = useRef(new Animated.Value(0.98)).current;
  const opacity   = useRef(new Animated.Value(0)).current;

  useEffect(() => {
    if (trigger) {
      Animated.parallel([
        Animated.spring(translateY, { toValue: 0, damping: 20, stiffness: 300, useNativeDriver: true }),
        Animated.spring(scale,     { toValue: 1, damping: 20, stiffness: 300, useNativeDriver: true }),
        Animated.timing(opacity,   { toValue: 1, duration: 180, useNativeDriver: true }),
      ]).start();
    }
  }, [trigger]);

  return { translateY, scale, opacity };
}
```

Spring parameters (damping: 20, stiffness: 300) produce a snappy, slightly bouncy feel consistent across all modals. All animations run on the native thread (`useNativeDriver: true`) for 60fps performance with zero JS thread involvement.

---

## Localization

The app supports Azerbaijani (`az`) and English (`en`) via a custom `useTranslation` hook. The active language is stored as a `Lang` type in `OBAApp` state. The hook exposes:

- `getText(key)` — returns the translated string for a given key
- `getExactMonth(date)` — locale-aware month formatting for receipt dates
- `getCategoryLabel(category)` — translates product category identifiers for the filter UI
- `t` — the full `TranslationDict` object for cases where components need the entire dictionary (e.g. `FeedbackModal` uses `t.feedbackOptions` to dynamically render the reason chip list)

Language can be switched live from the Settings screen with no app restart.

---

## Design System

The app uses a consistent brand palette defined in `constants/colors.ts` and referenced as `C.*` throughout components:

| Token | Hex | Usage |
|---|---|---|
| `C.green` | `#004D3B` | Primary CTAs, trust score badge, submit buttons |
| `C.lime` | `#D4F238` | Active nav icons, filled stars, reward highlights |
| `C.black` | `#1A1A1A` | Nav bar, secondary buttons, backdrop overlays |
| White | `#FFFFFF` | Card and modal backgrounds |
| Gray-50 | `#F9FAFB` | Input backgrounds, notification cards |

Global layout styles live in `styles/styles.ts` and cover shared primitives (`s.header`, `s.appContainer`, `s.toast`, `s.loadingOverlay`) used across the orchestrator and screens.

Modal card corners use `borderRadius: 32` throughout, bottom sheets use `borderTopLeftRadius/borderTopRightRadius: 32`, and all interactive elements use `activeOpacity` values between 0.8–0.95 for consistent press feedback.

---

## Trust Score / Reliability System

The Reliability score is the anti-spam backbone of the platform. It is surfaced prominently in the app header as a percentage badge with an SVG shield icon. Tapping it opens a modal explaining the mechanics clearly to the user.

The design intent is economic: users who submit random, inconsistent, or dishonest ratings see their reliability percentage drop, which directly multiplies down their reward earnings. This makes honest, thoughtful ratings the financially rational choice — no manual moderation required. The backend runs its own clustering-based Trust Score algorithm; the mobile app simply displays the result and communicates the incentive clearly.

---

## Business Context

This app is the **B2C data collection layer** of a three-tier platform:

```
Mobile App (this repo)          ← shoppers rate purchased products
        ↓
Backend API (Python / Node.js)  ← AI clusters ratings, runs Trust Score spam detection
        ↓
Analytics Dashboard             ← Stop / Scale / Export signals for retail management
        ↓
FMCG Suppliers                  ← pay for benchmarked competitor data dashboards
```

Epsilon operates on a **budget reallocation model**: loyalty rewards that supermarkets already spend blindly are redirected only to users generating actionable data — making the marginal cost of structured R&D data effectively zero.

Currently incubated by **Veysəloğlu Group** and integrating with the OBA Market loyalty card ecosystem — the largest retail conglomerate in Azerbaijan.

---

## Getting Started

### Prerequisites
- Node.js 18+
- npm or yarn
- Expo Go on your phone, or an Android/iOS emulator

### Install & Run

```bash
cd client/app
npm install
npx expo start
```

Scan the QR code with Expo Go, or press `a` for Android emulator / `i` for iOS simulator.

---

## Team

| Role | Responsibility |
|---|---|
| Lead Product Engineer | Frontend / Mobile — this codebase |
| Lead Backend Engineer | Data pipeline, AI clustering, API, Trust Score |
| Project Coordinator / Lead Designer | UX, design system, product direction |
