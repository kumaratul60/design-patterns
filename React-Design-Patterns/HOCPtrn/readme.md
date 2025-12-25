# Higher-Order Components (HOC) & Higher-Order Functions (HOF)

## Overview

Higher-Order Components (HOC) and Higher-Order Functions (HOF) are patterns
used to **reuse logic across multiple components**.

They wrap existing behavior and enhance it without modifying the original implementation.

---

## Core Idea

### Higher-Order Component (HOC)

- A function that takes a component
- Returns a new component with added behavior

```tsx
const withAuth = (Component) => {
  return function WrappedComponent(props) {
    return <Component {...props} />;
  };
};
```

## HOF

```tsx
const withLogging = (fn) => {
  return (...args) => {
    console.log(args);
    return fn(...args);
  };
};
```
---

## When to Use

- When multiple components share the same logic
- Cross-cutting concerns (auth, logging, analytics)
- Enhancing existing components without rewriting them
- Working with legacy React codebases

---

## When NOT to Use

- New code that can be expressed with hooks
- Simple logic that does not justify abstraction
- When component hierarchy becomes hard to trace

---

## Common Pitfalls

- HOCs are harder to debug due to component wrapping
- Deeply nested HOCs create wrapper hell
- Wrapped components appear deeply nested in React DevTools
- Props collisions and naming conflicts
- Overuse leads to complex, unreadable component trees

---

## Performance Notes

- Each HOC adds an extra component layer
- Usually negligible, but can add overhead when stacked
- Memoization is required in some cases

---

## Rule of Thumb

- Prefer hooks for new development
- Use HOCs mainly for legacy or cross-cutting concerns
- Avoid nesting multiple HOCs
- If debugging becomes hard, reconsider the pattern
- Hooks provide better composability and debugging than HOCs


<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-05/hoc-pattern/src -->
