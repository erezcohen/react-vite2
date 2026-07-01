# Routing in This Application

## Overview

The app uses **React Router DOM v7** (`^7.3.0`) with a **`BrowserRouter`** — meaning URLs use the HTML5 History API (e.g., `/dashboard`, not `/#/dashboard`).

---

## Files

| File             | Role                                                   |
| ---------------- | ------------------------------------------------------ |
| `src/App.tsx`    | Mounts `BrowserRouter` as the routing context provider |
| `src/Router.tsx` | Defines all routes using `Routes` and `Route`          |

---

## `App.tsx` — Router Provider

```tsx
import { BrowserRouter } from 'react-router-dom';

export default function App() {
  return (
    <ThemeProvider>
      <BrowserRouter>
        {' '}
        {/* wraps the entire app */}
        <Router />
      </BrowserRouter>
    </ThemeProvider>
  );
}
```

`BrowserRouter` is the outermost routing context. It must wrap any component that uses routing hooks (`useNavigate`, `useParams`, etc.) or `Link` components.

---

## `Router.tsx` — Route Definitions

```tsx
import { Routes, Route } from 'react-router-dom';

export default function Router() {
  return (
    <Routes>
      <Route element={<AppLayout />}>
        {' '}
        {/* layout wrapper */}
        <Route path="" element={<Dashboard />} /> {/* → / */}
        <Route path="sample" element={<Sample />} /> {/* → /sample */}
      </Route>
    </Routes>
  );
}
```

### Key concepts

- **`Routes`** — picks the best-matching route from all children
- **Layout route** — `<Route element={<AppLayout />}>` wraps child routes with shared UI (header, footer, sidebar). `AppLayout` renders child routes via `<Outlet />`
- **Index route** — `path=""` renders `Dashboard` at `/`
- **Child route** — `path="sample"` renders `Sample` at `/sample`

---

## Adding a New Route

1. Create a page component in `src/pages/`
2. Add a `<Route>` inside the layout route in `Router.tsx`:

```tsx
import NewPage from './pages/NewPage';

<Route element={<AppLayout />}>
  <Route path="" element={<Dashboard />} />
  <Route path="sample" element={<Sample />} />
  <Route path="new-page" element={<NewPage />} /> {/* → /new-page */}
</Route>;
```

3. Optionally add a menu item in `src/config/menu.ts`

---

## Summary

| Concept          | Value                                             |
| ---------------- | ------------------------------------------------- |
| Package          | `react-router-dom`                                |
| Version          | `^7.3.0`                                          |
| Router type      | `BrowserRouter` (HTML5 history)                   |
| Route definition | `Routes` + `Route` components                     |
| Layout pattern   | Nested routes with a layout wrapper (`AppLayout`) |
