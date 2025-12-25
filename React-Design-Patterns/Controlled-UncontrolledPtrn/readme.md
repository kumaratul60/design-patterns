# Controlled vs Uncontrolled Component Patterns

## Overview

Controlled and Uncontrolled components define **where form state lives**:

- React state
- Refs
- The DOM itself

Choosing the right pattern improves correctness, performance, and maintainability—especially in forms.

---

## Core Idea

### Controlled Components

- Form state is managed by React
- Input value comes from state
- Updates flow through `onChange`

### Uncontrolled Components

- Form state is managed by the DOM
- Values are accessed via refs
- Minimal React involvement

---

## When to Use Controlled Components

- Complex forms
- Multi-step forms
- Forms with validation
- Forms tied to global state (Redux, Zustand, URL state)
- Business-critical data flows

---

## When to Use Uncontrolled Components

- Simple login forms
- Newsletter signup
- Quick prototypes
- Forms with minimal interaction or validation

---

## Core Rules & Responsibilities

- Controlled components use React state as the source of truth
- Uncontrolled components rely on the DOM as the source of truth
- Choose one pattern per input
- Avoid mixing state and refs for the same value
- Predictability matters more than convenience
- Controlled inputs re-render on every change; uncontrolled inputs do not
- Uncontrolled components may use refs or direct DOM access on submit

---

## Comparison Table

| Aspect          | Controlled               | Uncontrolled (Ref) | Uncontrolled (DOM) |
| --------------- | ------------------------ | ------------------ | ------------------ |
| Source of Truth | React State              | DOM via Ref        | DOM                |
| Code Style      | Verbose                  | Minimal            | Minimal            |
| Validation      | Easy                     | Manual             | Manual             |
| Reset Form      | Simple (state reset)     | Imperative         | Imperative         |
| Readability     | High                     | Medium             | Low                |
| Complexity      | Higher                   | Low                | Low                |
| Predictability  | High                     | Medium             | Low                |
| Best Use Case   | Complex forms, workflows | Simple forms       | Quick prototypes   |

---

## Key Learnings

- Controlled components favor correctness and control
- Uncontrolled components favor speed and simplicity
- Mixing patterns for the same input leads to bugs
- Choose based on form complexity, not preference

---

## Rule of Thumb

- Complex logic or validation → Controlled
- Simple data capture → Uncontrolled
- Never mix state and refs for the same field

---

## Common Anti-Pattern (Avoid)

- Using `useState` and `useRef` for the same input
- Reading from DOM while also controlling value via state
- Switching patterns mid-component

These cause inconsistent UI and hard-to-debug bugs.

---

## Controlled Component Example

```tsx
function LoginForm() {
  const [email, setEmail] = useState('');

  return <input value={email} onChange={(e) => setEmail(e.target.value)} />;
}
```

## Characteristics

- Predictable
- Easy to validate
- Easy to debug
- Slightly more verbose

---

## Uncontrolled Component Example

```tsx
function NewsletterForm() {
  const emailRef = useRef(null);

  const submit = () => {
    console.log(emailRef.current.value);
  };

  return <input ref={emailRef} />;
}
```

## Characteristics

- Less code
- Faster to build
- Harder to validate
- DOM-driven state

---

## Controlled Contact Form

```tsx
function ContactFormControlled() {
  const [name, setName] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={name} onChange={(e) => setName(e.target.value)} />
    </form>
  );
}
```

## Uncontrolled Contact Form (With Ref)

```tsx
function ContactFormUncontrolledRef() {
  const nameRef = useRef(null);

  const handleSubmit = () => {
    console.log(nameRef.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input ref={nameRef} />
    </form>
  );
}
```

## Uncontrolled Contact Form (Without Ref)

```tsx
function ContactFormUncontrolledDOM() {
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(e.target.name.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="name" />
    </form>
  );
}
```

## Hybrid Pattern

In some cases, a form may mix patterns **across different fields**.

- Controlled inputs for fields with validation or conditional logic
- Uncontrolled inputs for simple, non-critical fields

**Rule:** Never mix patterns for the same input.

---

## Performance Note (Large Forms)

- Controlled inputs re-render on every change
- Large forms with many controlled fields may feel slow
- Uncontrolled inputs reduce re-renders

**Tip:** Prefer uncontrolled inputs or form libraries for large or high-frequency forms.

---

## Form Library Mapping

| Library         | Pattern Used             |
| --------------- | ------------------------ |
| React Hook Form | Uncontrolled (ref-based) |
| Formik          | Controlled               |
| Final Form      | Controlled               |

**Guideline:**
Use libraries when form complexity outweighs manual state management.

<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-02/controlled-uncontrolled-comp-pattern/src -->
