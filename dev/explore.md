# Codebase Exploration: react-vite2

## Project Overview

This is a modern **React 19 + Vite + TypeScript** starter template with comprehensive testing setup, dark mode support, responsive design, and shadcn/ui component library integration.

---

## Directory Structure & File Organization

```
/home/erez/repos/react-vite2/
├── src/                          # Application source code
│   ├── components/               # Reusable UI components
│   │   ├── __tests__/           # Component unit tests (Vitest)
│   │   ├── ui/                  # shadcn/ui components (pre-built primitives)
│   │   ├── icons/               # Icon components
│   │   ├── app-header.tsx       # Main navigation header
│   │   ├── app-sidebar.tsx      # Mobile sidebar navigation
│   │   ├── app-layout.tsx       # Main app layout wrapper
│   │   ├── app-footer.tsx       # Footer with theme toggle
│   │   ├── app-logo.tsx         # Brand logo
│   │   ├── mode-toggle.tsx      # Light/dark theme switcher
│   │   └── page-header.tsx      # Page section header component
│   ├── pages/                    # Page components (route targets)
│   │   ├── __tests__/           # Page-level tests
│   │   ├── Dashboard.tsx        # Home page
│   │   └── Sample.tsx           # Sample page example
│   ├── contexts/                 # React Context API providers
│   │   └── ThemeContext.tsx     # Dark mode theme management
│   ├── hooks/                    # Custom React hooks
│   │   └── use-mobile.ts        # Mobile breakpoint detection
│   ├── config/                   # Application configuration
│   │   └── menu.ts              # Navigation menu structure
│   ├── lib/                      # Utility functions
│   │   └── utils.ts             # Helper utilities (cn() class merging)
│   ├── test/                     # Testing configuration
│   │   ├── setup.ts             # Vitest setup & mocks
│   │   └── test-utils.tsx       # Custom test render function
│   ├── App.tsx                  # Root app component
│   ├── Router.tsx               # Route definitions
│   ├── main.tsx                 # Entry point
│   ├── index.css                # Global styles (Tailwind + CSS variables)
│   └── vite-env.d.ts            # Vite type definitions
├── tests/                        # End-to-end tests (Playwright)
│   ├── dashboard.spec.ts        # Dashboard page E2E tests
│   ├── demo-todo-app.spec_.ts   # (Example/template)
│   └── sample-page.spec.ts      # Sample page E2E tests
├── public/                       # Static assets
│   ├── avatars/                 # User avatar images
│   └── favicon.svg              # Site favicon
├── dev/                         # Development utilities
├── .claude/                     # Claude Code settings
└── Configuration Files
    ├── vite.config.ts           # Vite bundler config
    ├── vitest.config.ts         # Unit test configuration
    ├── playwright.config.ts     # E2E test configuration
    ├── tsconfig.json            # TypeScript base config
    ├── tsconfig.app.json        # App TypeScript config
    ├── tsconfig.node.json       # Node TypeScript config
    ├── eslint.config.js         # ESLint linting rules
    ├── components.json          # shadcn/ui CLI config
    ├── .prettierrc.json         # Code formatting config
    ├── package.json             # Dependencies & scripts
    └── index.html               # HTML entry point
```

---

## Entry Points & Application Flow

### HTML Entry Point

`index.html` — Single-page app root with `<div id="root">` mount point. Imports `main.tsx` as ES module.

### Application Initialization

1. `main.tsx` — Creates React root and mounts `App.tsx` in strict mode
2. `App.tsx` — Wraps the app with:
   - `ThemeProvider` — Manages light/dark mode state
   - `BrowserRouter` — Enables client-side routing
   - `Router` component — Defines all routes

---

## Routing Architecture

`Router.tsx` uses React Router v7 with nested routes:

```
<Routes>
  <Route element={<AppLayout />}>   // wrapper with header/footer
    <Route path="/" element={<Dashboard />} />
    <Route path="/sample" element={<Sample />} />
  </Route>
</Routes>
```

All pages are wrapped by `AppLayout`, which provides consistent header, footer, and centered content area with `max-w-7xl` constraint.

---

## Component Architecture

### Layout Components

| Component         | Purpose                                                                       |
| ----------------- | ----------------------------------------------------------------------------- |
| `app-layout.tsx`  | Main layout wrapper with header/footer and `<Outlet />` for route content     |
| `app-header.tsx`  | Sticky top navigation — displays menu items, user avatar dropdown, logo       |
| `app-sidebar.tsx` | Mobile-only navigation (hidden on md+ breakpoint) — hamburger menu in popover |
| `app-footer.tsx`  | Footer container with theme toggle                                            |
| `app-logo.tsx`    | Brand logo linked to home                                                     |
| `page-header.tsx` | Reusable section header component for pages                                   |

### Navigation & Menu

`config/menu.ts` — Centralized menu structure (TypeScript):

```typescript
mainMenu: [
  { title: 'Dashboard', url: '/', icon: Gauge },
  { title: 'Sample Page', url: '/sample', icon: Files },
];
```

Used by:

- `app-header.tsx` — Renders desktop navigation (hidden on mobile)
- `app-sidebar.tsx` — Renders mobile popover menu

Supports nested menu items with dropdowns.

### UI Components (`components/ui/`)

Pre-built shadcn/ui components using Radix UI primitives:

- `button.tsx` — Styled button with CVA variants (default, destructive, outline, secondary, ghost, link)
- `card.tsx` — Card container with header/content areas
- `avatar.tsx` — User avatar with image/fallback
- `dropdown-menu.tsx` — Dropdown/context menu
- `sidebar.tsx` — Sidebar navigation structure
- `collapsible.tsx` — Expandable sections
- `popover.tsx` — Floating content popup
- `sheet.tsx`, `tooltip.tsx`, `separator.tsx`, `input.tsx`, `skeleton.tsx` — Additional UI primitives

### Styling System

Uses CVA (class-variance-authority) for variant management. `cn()` utility in `lib/utils.ts` merges Tailwind classes without conflicts via `tailwind-merge`.

---

## State Management & Context

### Theme Context (`contexts/ThemeContext.tsx`)

- **Storage**: Persists theme preference to localStorage
- **Modes**: `'light'`, `'dark'`, `'system'` (respects OS preference)
- **Hook**: `useTheme()` for accessing theme state and setter
- **Implementation**: Applies class to `<html>` root element for CSS variable switching

CSS variables defined in `index.css` with light/dark variants using OKLch color space.

### Custom Hooks

`hooks/use-mobile.ts` — Reactive mobile breakpoint detection:

- Triggers on window resize
- Compares `innerWidth` against 768px breakpoint
- Used to conditionally show sidebar vs. header nav

---

## Configuration & Build

### TypeScript

- Strict mode enabled
- Path alias: `@/*` → `./src/*`
- Target: ES2020, Module: ESNext

### Vite (`vite.config.ts`)

- Plugins: React + Tailwind CSS Vite plugin
- Alias: `@` → `./src`
- Base URL configurable via `VITE_BASE_URL` env var

### Vitest (`vitest.config.ts`)

- Environment: jsdom
- Setup files: `src/test/setup.ts` (mocks localStorage, matchMedia)
- Globals enabled

### Playwright (`playwright.config.ts`)

- Test directory: `./tests`
- Base URL: `http://localhost:5173`
- Browser: Chromium
- HTML reporter; trace on first retry

---

## Testing Setup

### Unit Testing (Vitest + React Testing Library)

`src/test/test-utils.tsx` — Custom render wrapper:

- Wraps components with `BrowserRouter` + `ThemeProvider`
- Mocks `window.localStorage` and `window.matchMedia`
- Re-exports all `@testing-library/react` functions

### E2E Testing (Playwright)

- Tests in `tests/` with `.spec.ts` extension
- Tests cover page loading, navigation, responsive viewports, semantic HTML, and JS error monitoring

---

## Development Scripts

| Script                    | Purpose                                  |
| ------------------------- | ---------------------------------------- |
| `npm run dev`             | Start Vite dev server (localhost:5173)   |
| `npm run build`           | TypeScript check + Vite production build |
| `npm run lint`            | Run ESLint                               |
| `npm run format`          | Format with Prettier                     |
| `npm run typecheck`       | TypeScript compiler check                |
| `npm test`                | Vitest watch mode                        |
| `npm run test:run`        | Run tests once (CI)                      |
| `npm run test:coverage`   | Coverage report                          |
| `npm run test:e2e`        | Playwright headless                      |
| `npm run test:e2e:ui`     | Playwright UI mode                       |
| `npm run test:e2e:headed` | Playwright with visible browser          |

---

## Key Dependencies

### Core

- `react` 19, `react-dom` 19, `react-router-dom` 7

### UI & Styling

- `tailwindcss` 4, `@radix-ui/*`, `class-variance-authority`, `tailwind-merge`, `clsx`, `lucide-react`

### Dev Tools

- `vite` 6, `typescript` 5.7, `vitest` 3, `@testing-library/react` 16, `@playwright/test` 1.54, `eslint` 9, `prettier` 3

---

## Notable Patterns & Conventions

1. **Path aliasing**: `@/` prefix for absolute imports from `src/`
2. **CVA pattern**: Component variants defined once, reusable across the codebase
3. **Centralized menu config**: Single source of truth consumed by both header and sidebar
4. **Custom render wrapper**: `src/test/test-utils.tsx` handles all providers for tests
5. **Mobile-first responsive**: Sidebar on mobile, horizontal nav on desktop (md: 768px breakpoint)
6. **Context error boundary**: `useTheme()` throws if used outside provider
7. **Semantic HTML**: Navigation uses `<nav>`, pages use `<section>` headers
8. **Accessibility**: Role-based test queries, ARIA attributes, sr-only text
