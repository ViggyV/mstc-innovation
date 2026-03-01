# Template: Mobile Application (React Native / Expo)

Copy this to your project's `CLAUDE.md` and customize.

```markdown
# [App Name] — Claude Code Agent

## PROJECT OVERVIEW
**What:** [Mobile app description]
**Goal:** [Primary user outcome]
**Platforms:** iOS + Android
**Stage:** [Prototype | Beta | Production]

## TECH STACK
- **Framework:** React Native + Expo SDK 52
- **Language:** TypeScript (strict mode)
- **Navigation:** React Navigation 7
- **State:** Zustand (local) + React Query (server)
- **Backend:** Supabase / Firebase
- **Auth:** Supabase Auth / Firebase Auth
- **Storage:** AsyncStorage + SecureStore
- **Testing:** Jest + React Native Testing Library
- **OTA Updates:** Expo Updates

## ARCHITECTURE
```
app/
├── (tabs)/              # Tab-based navigation
│   ├── index.tsx        # Home tab
│   ├── search.tsx
│   └── profile.tsx
├── (auth)/              # Auth flow screens
│   ├── login.tsx
│   └── register.tsx
├── [id].tsx             # Dynamic routes
└── _layout.tsx          # Root layout

components/
├── ui/                  # Base UI components
├── forms/               # Form components
└── features/            # Feature components

lib/
├── api/                 # API client
├── auth/                # Auth utilities
├── storage/             # Local storage
└── hooks/               # Custom hooks

constants/
├── colors.ts
├── layout.ts
└── strings.ts
```

## CONVENTIONS
- Expo Router for all navigation (file-based routing)
- Use Expo modules when available over bare React Native
- React Query for ALL server state
- Zustand only for client-only state (theme, preferences)
- All user-facing strings in constants/strings.ts
- Test on both iOS and Android before completing any feature
- Support iOS 16+ and Android 13+

## CONSTRAINTS
- Never store sensitive data in AsyncStorage (use SecureStore)
- Always handle offline state gracefully
- All network requests need loading + error states
- Images must be optimized (use expo-image)
- Never hardcode device-specific dimensions
```
