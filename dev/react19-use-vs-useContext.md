# React 19: `use()` vs `useContext`

## TL;DR

For reading context, `use(Context)` and `useContext(Context)` return the same value with the same re-render behavior. `use()` is strictly more capable and is the React 19 idiomatic choice.

## Side-by-side

```tsx
// Pre-React 19
import { useContext } from 'react';
const context = useContext(ThemeContext);

// React 19
import { use } from 'react';
const context = use(ThemeContext);
```

## Key differences

### 1. `use()` can be called conditionally

`useContext` obeys the Rules of Hooks — never inside `if`, loops, or early returns. `use()` breaks that restriction:

```tsx
// ❌ Illegal with useContext
if (isAdmin) {
  const theme = useContext(ThemeContext);
}

// ✅ Legal with use()
if (isAdmin) {
  const theme = use(ThemeContext);
}
```

### 2. `use()` works with Promises (its primary purpose)

`use()` was designed to unwrap Promises inside components, suspending until resolved:

```tsx
function UserProfile({ userPromise }: { userPromise: Promise<User> }) {
  const user = use(userPromise); // suspends until resolved
  return <div>{user.name}</div>;
}
```

Context reading is a secondary capability of `use()`.

## Bonus: React 19 dropped the `.Provider` suffix

```tsx
// Pre-React 19
<ThemeContext.Provider value={...}>{children}</ThemeContext.Provider>

// React 19 — context itself is the provider
<ThemeContext value={...}>{children}</ThemeContext>
```

Both forms still work in React 19. This project uses the shorter form.

## This project

`ThemeContext.tsx` uses both React 19 patterns:

- `use(ThemeContext)` in the `useTheme()` hook (`ThemeContext.tsx:59`)
- `<ThemeContext value={...}>` as the provider (`ThemeContext.tsx:43`)
