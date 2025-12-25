# Custom Hooks Pattern

## Overview

Custom Hooks are a pattern for extracting and reusing **stateful logic** across React components.

They are the modern alternative to **Higher-Order Components (HOC)** and **Render Props** in most cases.

> **Note:** Hooks do not fully replace render props in rare, advanced composition scenarios.

---

## Core Idea

- A hook is a function that uses React hooks.
- It encapsulates **logic**, not JSX.
- Components decide how to render returned data.
- Hook behavior depends on dependency arrays.
- Custom hooks must start with `use`.

---

## Minimal Example

```ts
function useAuth() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetchUser().then(setUser);
  }, []);

  return { user };
}
```

```tsx
function Profile() {
  const { user } = useAuth();
  return <div>{user?.name}</div>;
}
```

## When to Use

- Reusing logic across multiple components.
- Replacing Higher-Order Components (HOCs) or render props.
- Sharing side effects or stateful behavior.
- Improving readability and composability.

## When **NOT** to Use

- Logic used only once.
- Simple components with minimal behavior.
- When abstraction adds more complexity than value.

## Common Pitfalls

- Over-engineering logic into hooks.
- Creating hooks too early without proven reuse.
- Unnecessary re-renders due to poor dependency management.
- Forgetting cleanup logic, causing memory leaks.
- Incorrect dependency arrays in `useEffect`.

## Cleanup Example

```ts
useEffect(() => {
  const unsubscribe = subscribeUser(setUser);
  return () => unsubscribe();
}, []);
```

## Performance Notes

- Hooks re-run on every render.
- Dependency arrays must be managed carefully.
- Memoize values when needed (`useMemo`, `useCallback`).
- Avoid returning new object or function references unnecessarily.

## Rule of Thumb

- Use hooks to reuse **logic**, not JSX.
- Prefer hooks over HOCs and render props.
- Keep hooks small and focused.
- If a hook grows too complex, split it.

---

## Hook Composition Trees & Hook Anti-Patterns

### Hook Composition Trees

A **Hook Composition Tree** is a pattern where small, focused hooks are composed together to build more complex behavior.

- Hooks can call other hooks.
- Each hook should have one clear responsibility.
- Complexity grows by composition, not by bloating a single hook.

#### Good Composition Example

```tsx
// useUser.ts
export function useUser() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetch('/api/user')
      .then((res) => res.json())
      .then(setUser);
  }, []);

  return user;
}

// usePermissions.ts
export function usePermissions(user) {
  return useMemo(() => {
    if (!user) return [];
    return user.role === 'admin' ? ['read', 'write'] : ['read'];
  }, [user]);
}

// useAuth.ts
export function useAuth() {
  const user = useUser();
  const permissions = usePermissions(user);

  return { user, permissions };
}
```

```tsx
function Dashboard() {
  // Assuming useAuth is used here, renamed from useDashboard for consistency
  const { user, permissions } = useAuth();
  return <View user={user} permissions={permissions} />;
}
```

## Deep Composition Example

```ts
function useAuth() {
  return useUser();
}

function useUser() {
  return useApi('/user');
}
```

```txt
useAuth
 └── useUser
      └── useApi
```

## Why It Matters

- Too many layers make debugging difficult.
- Side effects become implicit.
- Dependencies and re-render behavior get tricky.
- Ownership of logic becomes unclear.

---

## Good Use

- Small, focused hooks.
- Single responsibility per hook.
- Shallow and easy-to-trace composition.

## Bad Use

- Deep chains with unclear ownership.
- Hooks that do too much.
- Hidden side effects inside composed hooks.

### Rule

> If you can’t explain a hook in one sentence, it’s probably too big.

---

## Hook Anti-Pattern Examples

### Anti-Pattern 1: God Hook

```ts
function useEverything() {
  useAuth();
  useTheme();
  useTracking();
  useNotifications();
}
```

**Problem:**
- Hard to test.
- Hard to reuse.
- Hard to reason about.

**Fix:**
- Split into focused hooks.
- Compose them at the component level.

### Anti-Pattern 2: Conditional Hook Calls

```ts
// WRONG
if (isLoggedIn) {
  useUser();
}
```

**Problem:**
- Breaks the Rules of Hooks.
- Causes runtime bugs.

**Fix:**
- Always call hooks unconditionally.

```ts
// RIGHT
useUser(isLoggedIn);
```

### Anti-Pattern 3: Unstable Returns

```ts
// WRONG
function useData() {
  return { value: compute() };
}
```

**Problem:**
- New object created on every render.
- Causes unnecessary re-renders in consumers.

**Fix:**

```ts
// RIGHT
function useData(input) {
  // Ensure 'value' or 'input' is in the dependency array
  const value = compute(input);
  return useMemo(() => ({ value }), [value]);
}
```

### Anti-Pattern 4: Missing Cleanup

```ts
// WRONG
useEffect(() => {
  window.addEventListener('resize', onResize);
}, []);
```

**Problem:**
- Memory leaks when the component unmounts.

**Fix:**

```ts
// RIGHT
useEffect(() => {
  window.addEventListener('resize', onResize);
  return () => window.removeEventListener('resize', onResize);
}, []);
```

## Quick Summary

### Hook Composition Trees
- Hooks calling hooks.
- Good when shallow and focused.
- Bad when deep and implicit.

### Hook Anti-Patterns
- God hooks.
- Conditional hooks.
- Missing cleanup.
- Unstable references.

---

## One-Line Rule

> Hooks scale best when they are small, flat, and boring.

## What “boring” means in this context

When people say **Hooks scale best when they are small, flat, and boring**, they mean:

### 1. Predictable

A boring hook:
- Does exactly what its name says.
- Has no surprising side effects.
- Always behaves the same way given the same inputs.

**Examples:**
```ts
useWindowWidth()
useUser()
useDebounce(value)
```
You can guess what these do without opening the file.

### 2. Single Responsibility

A boring hook:
- Solves one problem.
- Doesn’t mix concerns (auth + analytics + UI state).

**Not boring (bad):**
```ts
useAuthAndThemeAndTracking()
```

**Boring (good):**
```ts
useAuth()
useTheme()
useTracking()
```

### 3. Easy to Read & Debug

A boring hook:
- Has few effects.
- Simple dependency arrays.
- Minimal internal state.

When something breaks, you immediately know where to look.

### 4. Not Clever

A boring hook:
- Avoids clever abstractions.
- Avoids deep composition chains.
- Avoids “magic” behavior.

**Clever (bad):**
```ts
useAutoSyncingCachedAuthWithFallback()
```

**Boring (good):**
```ts
useAuth()
```

### 5. Composable

Boring hooks are powerful when combined, not individually impressive.

```ts
function useApp() {
  const user = useUser();
  const theme = useTheme();
  const permissions = usePermissions(user);

  return { user, theme, permissions };
}
```

- Each hook is boring.
- The composition creates value.

### Short Version

**“Boring” =**
- Simple
- Obvious
- Predictable
- Single-purpose
- Easy to explain in one sentence

If a hook feels “smart” or “clever,” it’s often a sign it’s doing too much.

### One-Sentence Test

> If you can’t explain what a hook does in one sentence, it’s probably not boring enough — and needs to be split.

<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-06/custom-pattern-hooks/src -->
