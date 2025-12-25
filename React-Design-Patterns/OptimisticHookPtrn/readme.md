# Optimistic Hook Pattern (`useOptimistic`)

## Overview

The Optimistic Hook pattern updates the UI **before** the server confirms an action.
It temporarily assumes success to deliver a fast, responsive user experience.

This pattern is ideal for **user-driven interactions** where waiting for a network response would feel slow or disruptive.

---

## Core Idea

- Update UI optimistically
- Trigger the API request in parallel
- Roll back to the previous state if the request fails
- Always treat the server as the source of truth

---

## Common Use Cases

- Social media likes or reactions
- Poll voting
- Adding or removing items from cart
- Toggling bookmarks or follows

---

## Minimal Example

```ts
function useOptimisticToggle(initialValue) {
  const [value, setValue] = useState(initialValue);
  const [snapshot, setSnapshot] = useState(initialValue);

  const update = async (nextValue, apiCall) => {
    setSnapshot(value);
    setValue(nextValue);

    try {
      await apiCall();
    } catch {
      setValue(snapshot); // rollback
    }
  };

  return [value, update];
}
```

```tsx
function LikeButton() {
  const [liked, toggleLike] = useOptimisticToggle(false);

  const onClick = () => {
    toggleLike(!liked, () => api.likePost());
  };

  return <button>{liked ? 'Liked' : 'Like'}</button>;
}
```

```md
# Core Rules & Responsibilities

- UI updates optimistically, not permanently
- Always keep a rollback snapshot
- Server response must confirm or reject the action
- Failure handling is mandatory

---

## Common Pitfalls

- Assuming optimistic updates will always succeed
- Forgetting rollback logic
- Ignoring error or retry flows
- Using optimistic updates for critical or irreversible actions

---

## When to Use

- Actions are reversible
- Failure impact is low
- Retry is meaningful
- UX speed is more important than immediate consistency

---

## When NOT to Use

- Financial transactions
- Security-sensitive actions
- Destructive or irreversible operations
- When strong data consistency is required
- When users expect explicit confirmation

---

## Performance Notes

- Dramatically improves perceived performance
- Reduces blocking loaders
- Increases implementation complexity
- Must be paired with clear rollback UX

---

## Optimistic vs Pessimistic Updates

### Optimistic Updates

- UI updates immediately
- Assumes success
- Rolls back on failure

**Pros**

- Fast and responsive
- Great user experience

**Cons**

- Requires rollback logic
- Temporary inconsistency possible

**Best For**

- Likes, reactions, votes
- Add/remove from cart
- Non-critical toggles

---

### Pessimistic Updates

- UI updates after server confirmation
- Waits for API response

**Pros**

- Always consistent
- Safer and simpler

**Cons**

- Slower feedback
- Requires loading states

**Best For**

- Payments
- Account and security actions
- Destructive operations

---

## Rule of Thumb

- Optimistic updates are about speed, not certainty.
- Use optimistic updates for UX-first interactions
- Use pessimistic updates for correctness and safety
- Always plan a rollback
- If rollback UX feels complex, choose pessimistic instead
```

### Quick Comparison

| Aspect                | Optimistic Update     | Pessimistic Update |
| --------------------- | --------------------- | ------------------ |
| UI Feedback           | Immediate             | After API response |
| Perceived Performance | High                  | Lower              |
| Complexity            | Higher                | Lower              |
| Error Handling        | Rollback required     | Straightforward    |
| Data Safety           | Medium                | High               |
| Best Use Case         | UX-first interactions | Critical actions   |

---


<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-08/optimistic-pattern/src -->
