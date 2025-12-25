# Strategy Pattern

## Overview

The **Strategy Pattern** allows you to define a family of behaviors,
encapsulate each one as a separate function, and select which behavior
to use at runtime.

In React, strategies are often implemented using **functions or hooks**,
with components acting as **consumers** that do not know _how_ the logic works —
only _which_ strategy is chosen.

---

## Core Idea

1. Each strategy is a function (or hook)
2. Strategies are registered in a registry
3. A selector chooses the strategy
4. Consumers use the selected strategy without knowing implementation details

```txt
config → factory → hooks → components
```

- Components are consumers
- They do not know the **“how”**
- They only depend on the selected behavior

## Minimal Example

1. Strategies (Functions)

```ts
const strategies = {
  add: (a, b) => a + b,
  multiply: (a, b) => a * b,
};
```

2. Strategy Selector

```ts
function selectStrategy(type) {
  if (!strategies[type]) {
    throw new Error(`Unknown strategy: ${type}`);
  }
  return strategies[type];
}
```

3. Consumer

```ts
function Calculator({ type, a, b }) {
  const calculate = selectStrategy(type);
  return <div>{calculate(a, b)}</div>;
}
```

## Strategy Pattern with Hooks

## Important Rule (Hooks Rule Reminder)

Hooks must always be called at the top level of a component or hook.
Never inside loops, conditions, or callbacks.

Because of this, **strategy selection must happen outside hook execution**.

## Hook Factory Pattern or Hook-Based Strategy (Factory)

```ts
function useAdd() {
  return (a, b) => a + b;
}

function useMultiply() {
  return (a, b) => a * b;
}

const hookStrategies = {
  add: useAdd,
  multiply: useMultiply,
};
```

```ts
function useStrategy(type) {
  const strategyHook = hookStrategies[type];
  if (!strategyHook) {
    throw new Error(`Unknown hook strategy: ${type}`);
  }
  return strategyHook();
}
```

```tsx
function Calculator({ type, a, b }) {
  const calculate = useStrategy(type);
  return <div>{calculate(a, b)}</div>;
}
```

## Strategy Pattern + Context

- Strategy pattern decides what behavior to use
- Context decides where that decision lives

## Example:

- Context stores selected strategy (theme, pricing, permission)
- Hooks implement behavior
- Components stay unaware of logic and decision source

---

## When to Use Strategy Pattern

- Multiple interchangeable behaviors
- Conditional logic starting to grow
- Feature-based or config-driven behavior
- Avoiding large `if` or `switch` blocks

---

## Common Pitfalls

- Calling hooks conditionally inside strategies
- Letting components decide strategy logic
- Overusing strategy pattern for simple conditions
- Mixing strategy selection and execution in components

---

## Rule of Thumb

Use the Strategy Pattern when behavior varies, but the interface stays the same.

---

<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-13/strategy-pattern/src -->
