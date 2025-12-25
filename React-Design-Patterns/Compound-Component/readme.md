# Compound Components Pattern

## Overview

The Compound Components pattern allows multiple related components
to work together implicitly via a shared parent.

It eliminates prop drilling and enables declarative, flexible APIs—
commonly used in UI libraries and design systems.

---

## Core Idea

- A parent component owns shared state and behavior
- Child components consume that state implicitly
- Consumers compose UI using a natural, semantic structure

---

## When to Use

- Building UI libraries or design systems
- Components with tightly related sub-parts
- When children structure and ordering matter
- When prop drilling becomes noisy or fragile

---

## When NOT to Use

- Flat, independent components
- Components without shared state
- When composition order does not matter
- Simple parent–child relationships

---

## Core Rules & Responsibilities

- Parent component owns state and logic
- Sub-components communicate via context
- Avoid prop drilling between nested children
- Sub-components must belong to the parent semantically
- Do not attach unrelated components radially
- Do not re-export sub-components separately
- Use only when component structure matters

---

## Minimal Example

```tsx
function Tabs({ children }) {
  const [active, setActive] = useState(0);

  return <TabsContext.Provider value={{ active, setActive }}>{children}</TabsContext.Provider>;
}
```

```tsx
function TabsList({ children }) {
  return <div>{children}</div>;
}
```

```tsx
function TabsTrigger({ index, children }) {
  const { setActive } = useContext(TabsContext);
  return <button onClick={() => setActive(index)}>{children}</button>;
}
```

```tsx
function TabsPanel({ index, children }) {
  const { active } = useContext(TabsContext);
  return active === index ? <div>{children}</div> : null;
}
```

## Usages

```tsx
<Tabs>
  <TabsList>
    <TabsTrigger index={0}>Tab 1</TabsTrigger>
    <TabsTrigger index={1}>Tab 2</TabsTrigger>
  </TabsList>

  <TabsPanel index={0}>Content 1</TabsPanel>
  <TabsPanel index={1}>Content 2</TabsPanel>
</Tabs>
```

## Common Pitfalls

- Attaching unrelated components as sub-components
- Re-exporting sub-components independently
- Using compound components when simple props suffice
- Hiding too much logic, reducing clarity

---

## Rule of Thumb

If components are meaningless without each other,
the Compound Components pattern is a good fit.

<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-03/compound-components-patterns/src -->
