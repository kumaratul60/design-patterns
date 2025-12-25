## Slot Pattern (Named Placeholders)

## Overview

The **Slot Pattern** allows a component to expose **explicit placeholders**
(where consumers can inject UI) instead of relying on rigid props or deep composition.

Think of slots as **named holes** inside a component.

---

## Core Idea

- Component defines **where** content can go
- Consumer decides **what** goes into each slot
- Improves flexibility without breaking structure
- Keeps layout control with the component author
- Slots define structure; consumers fill content

---

## Types of Slots

### 1. Default Slot

A single, unnamed placeholder (usually `children`).

```tsx
function Card({ children }) {
  return <div className="card">{children}</div>;
}
```

### 2. Named Slots

Multiple explicit placeholders.

```tsx
function Modal({ header, body, footer }) {
  return (
    <div className="modal">
      <div className="header">{header}</div>
      <div className="body">{body}</div>
      <div className="footer">{footer}</div>
    </div>
  );
}
```

### Usage:

```tsx
<Modal header={<h2>Title</h2>} body={<p>Content</p>} footer={<button>Close</button>} />
```

# Slot Pattern in Layouts

## Why Use Slot Pattern

- Avoids prop drilling for layout content
- Prevents overuse of conditional rendering
- Makes components more expressive and reusable
- Preserves layout while allowing customization

---

## Common Use Cases

- **Modal / Dialog** — header / body / footer
- **Card** — media / content / actions
- **Layout** — header / sidebar / content / footer
- **Toolbar** — left / center / right regions
- **Form** — label / help / error slots
- **Data Table** — header cell slot, row renderer slot
- **Notification / Toast** — icon / content / action
- **Editor** — toolbar slots, block renderer slots
- **CMS Blocks** — content regions injected dynamically

---

## Slot Pattern vs Children

| Aspect                | Children Only       | Slot Pattern               |
| --------------------- | ------------------- | -------------------------- |
| **Structure control** | Low                 | High                       |
| **Flexibility**       | Medium              | High                       |
| **Readability**       | Can degrade quickly | Explicit and clear         |
| **Scalability**       | Limited             | Better for complex layouts |

---

## Common Pitfalls

- Creating too many slots without real need
- Making slots optional without clear defaults
- Overlapping responsibilities between slots
- Using slots when simple props would suffice

---

## Best Practices

- Start with a default slot
- Add named slots only when structure matters
- Provide sensible defaults where possible
- Keep slot responsibilities clear and documented

---

## Rule of Thumb

Use the Slot Pattern when layout structure matters and consumers need controlled flexibility.
If content is meaningless without structure, slots are usually a good fit.

<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-12/slot-pattern/src -->
