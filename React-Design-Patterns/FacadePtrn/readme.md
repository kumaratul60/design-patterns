## Facade Pattern (Hook Facade (fa-SAHD)) :

### Overview

The Facade Pattern provides a single, simplified interface over multiple hooks, APIs, or logic branches.

In React, this is commonly implemented as a custom hook.

Components talk to one hook only.
That hook manages all complexity internally.

---

## Core Idea

- One hook = one responsibility for the component
- Facade coordinates multiple hooks and logic paths
- Component stays simple and declarative
- Facade owns orchestration, not business decisions


## Minimal Example

### Without Facade (Problem)

```tsx
function Profile() {
  const user = useUser();
  const permissions = usePermissions();
  const settings = useSettings();

  // component knows too much
}
```

## With Facade (Solution)

```tsx
function useProfileFacade() {
  const user = useUser();
  const permissions = usePermissions();
  const settings = useSettings();

  return { user, permissions, settings };
}
```

```tsx
function Profile() {
  const profile = useProfileFacade();
  return <View profile={profile} />;
}
```

## When to Use Facade Pattern

- Components depend on multiple hooks
- Logic becomes scattered across the component
- You want a clean, focused component API
- Improving readability and testability

---

## Common Pitfalls

- Creating facades that simply re-export everything
- Making facades too generic
- Hiding important control from components unnecessarily

---

## Strategy vs Facade (Quick Comparison)

| Pattern      | Purpose                        |
| ------------ | ------------------------------ |
| **Strategy** | Decide which behavior to use   |
| **Facade**   | Simplify how logic is accessed |

---

## Rule of Thumb

- Use **Strategy** to choose behavior
- Use **Facade** to hide complexity
- Components should know **what**, not **how**



```ts
Headless UI
= hooks (logic)
+ compound components (structure)
+ slots / render props (rendering)
```
