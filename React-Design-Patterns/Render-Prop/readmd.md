# Render Props Pattern (Children as a Function)

## Overview

A Render Props pattern uses a prop whose value is a function that returns JSX.

The component provides **how the logic works**,
while the consumer decides **what to render**.

This pattern is commonly implemented using the `children` prop and is conceptually similar to the children-as-function pattern.

---

## Core Idea

- A component owns internal state and logic
- A function is passed as a prop (or via `children`)
- That function receives data and returns JSX
- Consumers control rendering without modifying internal logic
- The render function should remain pure and side-effect free

---

## Minimal Example

```tsx
function MouseTracker({ children }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  return <div onMouseMove={(e) => setPosition({ x: e.clientX, y: e.clientY })}>{children(position)}</div>;
}
```

```tsx
<MouseTracker>
  {({ x, y }) => (
    <p>
      {x}, {y}
    </p>
  )}
</MouseTracker>
```

## When to Use

- When you want to expose internal state to consumers
- When consumers should control JSX rendering
- When extending existing components without rewriting them
- When adding behavior on top of already-written components

---

## Relation to Children Prop Pattern

- Render props are often implemented via the `children` prop
- Both patterns allow passing JSX as a function
- The difference is intent: render props explicitly expose state

---

## Common Pitfalls

- Rendered JSX does not appear as a separate component in React DevTools
- Debugging can be harder due to inline render functions
- Overuse can reduce readability
- Deeply nested render props lead to “render prop hell”
- Rendered JSX does not appear as a distinct component in React DevTools

---

## Performance Notes

- A new inline function is created on every render
- Usually acceptable for small components
- Can become costly as applications grow
- Hooks exist as a cleaner, more scalable alternative

---

## Avoid

- Multiple nested render props
- Components that accept JSX and then expose another render prop inside
- Using render props when a simple prop or hook would suffice

---

## Rule of Thumb

Use render props when you need **full control over rendering** and want to expose internal logic safely.

Prefer hooks when logic reuse matters more than rendering flexibility.

---

## Anti-Pattern Example (Avoid)

### Nested Render Props (Render Prop Hell)

```tsx
<DataFetcher>
  {(data) => (
    <MouseTracker>
      {(position) => (
        <ThemeConsumer>{(theme) => <Component data={data} position={position} theme={theme} />}</ThemeConsumer>
      )}
    </MouseTracker>
  )}
</DataFetcher>
```

## Why This Is a Problem

- Deep nesting hurts readability
- Logic becomes hard to trace
- JSX becomes tightly coupled
- Debugging in React DevTools is difficult

## Better Alternative (Hooks)

```tsx
function Component() {
  const data = useData();
  const position = useMouse();
  const theme = useTheme();

  return <View data={data} position={position} theme={theme} />;
}
```

---

## Key Takeaway

- Avoid stacking render props.
- If logic reuse grows, migrate to hooks.

---

<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-04/render-props-pattern/src -->
