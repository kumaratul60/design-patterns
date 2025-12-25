# State Reducer Pattern

## Overview

The State Reducer Pattern gives **control over state transitions** to the consumer
by allowing them to override or extend internal state logic.

Instead of hard-coding behavior, the component exposes a reducer-like mechanism
to customize how state updates occur.

---

## Core Idea

- Component manages state internally
- State updates go through a reducer
- Consumer can intercept or modify state transitions
- Default behavior remains intact unless overridden

---

## When to Use

- Reusable components with complex state logic
- UI libraries and design systems
- Components that need customizable behavior
- When consumers need control without rewriting logic

---

## Minimal Example

```ts
function toggleReducer(state, action) {
  if (action.type === 'toggle' && state.disabled) {
    return state; // override default behavior
  }
  return { ...state, on: !state.on };
}
```

```ts
function useToggle({ reducer = toggleReducer } = {}) {
  const [state, dispatch] = useReducer(reducer, { on: false });
  return { state, dispatch };
}
```

## Core Rules & Responsibilities

- Reducer controls how state changes
- Default behavior must always exist
- Consumers may override behavior selectively
- State logic stays predictable and testable

---

## Common Use Cases

- Toggle components
- Accordions, dropdowns, modals
- Feature-flagged behavior
- Advanced component customization

---

## Common Pitfalls

- Implementing state reducers at the component level unnecessarily
- Over-engineering simple state
- Making reducers too complex or generic

---

## Important Guideline

- Keep the State Reducer pattern at the provider or hook level
- Avoid pushing reducer logic down into leaf components

---

## Rule of Thumb

> Use the State Reducer Pattern when you need controlled extensibility.
> If consumers don’t need to change behavior, a simple hook or state is enough.

<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-09/state-reducer-pattern/src -->
