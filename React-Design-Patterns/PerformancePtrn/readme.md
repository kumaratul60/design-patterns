# Performance Patterns in React

## Overview

Performance patterns focus on:

- Reducing unnecessary re-renders
- Minimizing computation
- Scaling UI efficiently as applications grow

Apply these patterns **intentionally**, not prematurely.

---

## Memoization

### What It Is

Caching values or functions so they are not recreated on every render.

### Tools

- `useMemo`
- `useCallback`
- `React.memo`

### When to Use

- Expensive computations
- Stable props passed to memoized components
- Preventing unnecessary re-renders

### Pitfall

- Overusing memoization adds complexity with little benefit

---

## Derived State

### What It Is

A **derived state** is any value that can be computed from existing state or props.

```ts
const fullName = `${firstName} ${lastName}`;
```

## Rule

- Derived state should NOT be stored in React state.

## Anti-Pattern: Derived Values in State

```ts
const [fullName, setFullName] = useState('');
```

## Why This Is a Problem

- Causes unnecessary re-renders
- Requires syncing original state and derived state
- Leads to stale or inconsistent UI
- Increases cognitive complexity

## Correct Approach

- Compute derived values during render
- Memoize only if the computation is expensive

```ts
const fullName = `${firstName} ${lastName}`;

// or, if expensive
const fullName = useMemo(() => `${firstName} ${lastName}`, [firstName, lastName]);
```

# Debounce / Throttle

## What It Is

Controls how often a function runs:

- **Debounce** → runs after inactivity
- **Throttle** → runs at fixed intervals

## Use Cases

- Search inputs
- Window resize
- Scroll handlers
- Autosave

## Pitfalls

- Forgetting cleanup causes memory leaks
- Incorrect dependencies cause stale closures

---

# Code Splitting

Code splitting, dynamic imports, and lazy-loaded assets improve performance when bundle size or initial load becomes a problem.

They reduce:

- Initial JavaScript payload
- Time to interactive (TTI)
- Unnecessary work on first render

**Apply them based on user flow, not everywhere.**

## Lazy Loaded Components

Load components only when they are actually needed.

```ts
const SettingsPage = React.lazy(() => import('./SettingsPage'));

<Suspense fallback={<Loader />}>
  <SettingsPage />
</Suspense>;
```

# Lazy Loading

## When to Use

- Routes not visited immediately
- Heavy components (e.g., charts, editors)
- Admin or secondary screens

## Pitfall

- Lazy-loading everything increases complexity
- Too many loading boundaries hurt UX

## Dynamic Imports

Load code at runtime instead of upfront.

```ts
if (isAdmin) {
  import('./admin-utils').then(...)
}

```

## When to Use

- Feature flags
- Role-based features
- Rarely used logic (PDF export, analytics, maps)

## Pitfall

- Harder error handling
- Can hide architectural issues if overused

---

## Lazy Loading Images

Load images only when they enter the viewport.

```html
<img src="image.jpg" loading="lazy" />
```

## When to Use

- Image-heavy pages
- Long scrolling pages
- Feeds, galleries, blogs

## Pitfall

- Above-the-fold images should not be lazy-loaded
- Poor placeholders cause layout shift (CLS)

---

# Layout Components & Performance

## Role

- Prevent unnecessary remounts
- Enable route-level code splitting
- Keep shared UI stable (header, sidebar)

## Best Practice

- Load layouts eagerly
- Lazy-load only the content inside layouts

---

# Virtualization

## What It Is

Rendering only visible items instead of entire lists.

## Use Cases

- Large lists
- Tables
- Infinite scrolling

## Benefits

- Massive performance improvements
- Lower memory usage

---

# Context Optimizations

## Problem

Any context value change re-renders all consuming components.

## Solutions

- Split contexts by responsibility
- Avoid frequently changing values in context
- Memoize provider values
- Keep context scope minimal

---

# React Compiler

## What It Is

Automatically optimizes React code at compile time.

## Benefits

- Reduces manual memoization
- Fewer unnecessary re-renders
- Simpler mental model

## Notes

- Still evolving
- Does not replace good architectural decisions
- Does not eliminate the need for good component design

---

# Common Performance Pitfalls

- Storing derived state in `useState`
- Memoizing everything blindly
- Putting frequently changing data in context
- Large components doing too much work

## Rule of Thumb

- Fix architecture before optimizing
- Avoid unnecessary state
- Measure before optimizing
- Prefer clarity, then performance

---

## Performance Anti-Patterns in React

## Overview

Performance anti-patterns are mistakes that look harmless but slowly degrade performance and maintainability.
Most performance problems come from architecture decisions, not React itself.

## 1. Storing Derived State in `useState`

```ts
const [fullName, setFullName] = useState('');
```

### Why This Is Bad

- Causes extra re-renders
- Requires synchronization
- Leads to bugs and stale data

### Fix

- Compute during render
- Memoize only if expensive

---

## 2. Overusing Context

### Anti-Pattern

Using context for frequently changing values

### Why This Is Bad

- All consumers re-render
- Becomes a hidden global store
- Hard to debug

### Fix

- Split contexts
- Keep values stable
- Prefer local state

---

## 3. Memoizing Everything

### Anti-Pattern

Blindly using `useMemo`, `useCallback`, `React.memo`

### Why This Is Bad

- Adds complexity
- Often provides no real benefit

### Fix

- Memoize only when profiling proves it helps

---

## 4. Large Components Doing Too Much

### Anti-Pattern

One component handling data, layout, and logic

### Fix

- Split by responsibility

---

## 5. Frequent State Updates

### Anti-Pattern

Updating state on every keystroke or scroll

### Fix

- Debounce
- Throttle
- Use uncontrolled inputs where appropriate

---

## Rule of Thumb

If performance issues exist, remove state first — then optimize.

---

# Render Optimization Checklist

## Component Design

- Is state minimal and necessary?
- Is derived state computed, not stored?
- Are responsibilities clearly split?

## Rendering

- Are large lists virtualized?
- Are expensive computations memoized?
- Are unnecessary re-renders visible in DevTools?

## State & Context

- Is context scope minimal?
- Are fast-changing values kept out of context?
- Are provider values memoized?

## Effects

- Are dependency arrays correct?
- Are effects cleaned up?
- Are effects doing too much work?

## Measurement

- Did you measure before optimizing?
- Are you fixing the slowest path first?

---

# “Before You Memo” Decision Tree

## Step 1: Is There a Performance Problem?

- ❌ No → Do nothing
- ✅ Yes → Continue

## Step 2: Is the Component Re-rendering Too Often?

- ❌ No → Memoization won’t help
- ✅ Yes → Continue

## Step 3: Is the Work Expensive?

- ❌ No → Memoization not needed
- ✅ Yes → Continue

## Step 4: Is the Input Stable?

- ❌ No → Fix data flow first
- ✅ Yes → Continue

## Step 5: Can You Reduce State Instead?

- ✅ Yes → Reduce state
- ❌ No → Memoize

## derived-state Example:

### Anti-Pattern (Storing Derived State)

```ts
const [filtered, setFiltered] = useState([]);

useEffect(() => {
  setFiltered(items.filter((i) => i.active));
}, [items]);
```

## Problems

- filtered is derived from items
- Extra state causes unnecessary re-renders
- Requires synchronization via useEffect
- Easy to introduce stale bugs

## Correct Pattern: Compute Derived State

```ts
const filtered = items.filter((i) => i.active);
```

Use this when filtering is cheap.

## If the Computation Is Expensive (Optional Memoization)

```ts
const filtered = useMemo(() => items.filter((i) => i.active), [items]);
```

## Key Rule

If a value can be computed from props or state, do not store it in state.

## Fix Broken Memoization

```ts
const Child = React.memo(({ onClick }) => {
  console.log('Child render');
  return <button onClick={onClick}>Click</button>;
});

export default function App() {
  const [count, setCount] = useState(0);

  return (
    <>
      <button onClick={() => setCount((c) => c + 1)}>Increment</button>
      <Child onClick={() => console.log('Child clicked')} />
    </>
  );
}
```

Why <Child> Was Re-rendering

Even though Child is wrapped in React.memo, it still re-renders because:

```ts
<Child onClick={() => console.log('Child clicked')} />
```

- A new function is created on every render
- React.memo does a shallow prop comparison
- New function reference ≠ previous one
- So Child re-renders every time count changes

## Fix: Stabilize the Callback with useCallback

```ts
const Child = React.memo(({ onClick }) => {
  console.log('Child render');
  return <button onClick={onClick}>Click</button>;
});

export default function App() {
  const [count, setCount] = useState(0);

  const handleChildClick = useCallback(() => {
    console.log('Child clicked');
  }, []);

  return (
    <>
      <button onClick={() => setCount((c) => c + 1)}>Increment</button>
      <Child onClick={handleChildClick} />
    </>
  );
}
```

## Result

- Clicking Increment updates only App
- <Child> renders once
- Callback reference stays stable
- React.memo now works as intended

## Key Rule

- React.memo only works if props are referentially stable.
- Use useCallback when passing functions to memoized children.

<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-11/performance-patterns/src -->
