# Observer / Pub-Sub Pattern

## Overview

The Observer (Pub-Sub) pattern enables **communication between decoupled parts**
of an application through events.

Publishers emit events.
Subscribers listen and react.

In React, this pattern is used **only when props, state, and context are not viable**.

---

## Core Idea

- Components publish events
- Other components subscribe to those events
- No direct parent–child relationship required
- Communication is indirect and decoupled

---

## Typical Use Cases

- Two separate React applications or micro-frontends communicating
- Micro-frontend architectures
- Cross-cutting concerns where props/state/context fail
- Global events like notifications or analytics
- Event buses shared across isolated modules

---

## Key Concepts

- Event Bus
- Publish
- Subscribe
- Unsubscribe
- Broadcast

---

## Minimal Example

```ts
const eventBus = {
  events: {},
  subscribe(event, callback) {
    this.events[event] = this.events[event] || [];
    this.events[event].push(callback);
    return () => {
      this.events[event] = this.events[event].filter((cb) => cb !== callback);
    };
  },
  publish(event, data) {
    (this.events[event] || []).forEach((cb) => cb(data));
  },
};
```

```tsx
// Subscriber
useEffect(() => {
  const unsubscribe = eventBus.subscribe('LOGIN', (user) => {
    console.log(user);
  });
  return unsubscribe;
}, []);
```

```ts
// Publisher
eventBus.publish('LOGIN', { name: 'User' });
```

## When to Use

- When two independent systems or applications must communicate
- When component hierarchy cannot be shared
- When props, lifted state, or context are not viable
- For truly global, cross-cutting events

---

## When NOT to Use

Before using Pub-Sub, always try:

- Local component state
- Props
- Lifted state
- Context
- Custom hooks

If any of these work, **do not use Pub-Sub**.

---

## Common Pitfalls

- Using Pub-Sub everywhere instead of React primitives
- Treating the event bus as a hidden global store
- Excessive or uncontrolled event emission
- Poorly named or undocumented events
- Forgetting to unsubscribe
- Creating circular event chains

---

## Why These Pitfalls Are Dangerous

- Debugging becomes extremely difficult
- Data flow becomes unpredictable
- Side effects become invisible
- Event storms create fragile systems
- Circular events can crash applications

---

## Best Practices

- Use Pub-Sub sparingly and intentionally
- Keep event scope narrow and well-defined
- Name events clearly and consistently
- Always unsubscribe to prevent memory leaks
- Centralize event definitions
- Avoid emitting events from event handlers recursively

---

## Rule of Thumb

- Use Observer / Pub-Sub **only for cross-cutting concerns**
- Never treat it as a hidden global state manager
- If debugging feels non-deterministic, you’ve gone too far

# Observer / Pub-Sub Pattern — Notifications System (Dummy Example)

This example demonstrates a **complete but minimal Pub-Sub–based notification system** used for **cross-cutting communication**, including scenarios where **two separate React applications** (or micro-frontends) need to talk to each other.

---

## Why Pub-Sub Here?

- Notifications are **global**
- Publishers and subscribers are **decoupled**
- Props, lifted state, and context are not practical
- Works across app boundaries (shared event bus)

---

## Architecture

```txt
Publisher (Buttons)
   ↓ publish
Event Bus
   ↓ subscribe
Notification Panel
```

## eventBus.js (Core Pub-Sub)

```js
const eventBus = {
  events: {},

  subscribe(event, callback) {
    this.events[event] = this.events[event] || [];
    this.events[event].push(callback);

    return () => {
      this.events[event] = this.events[event].filter((cb) => cb !== callback);
    };
  },

  // publish(event, payload) {
  //   (this.events[event] || []).forEach(cb => cb(payload));
  // },

  publish(event, payload) {
    if (!this.events[event]) return; // avoids crashes during edge cases and improves robustness
    this.events[event].forEach((cb) => cb(payload));
  },
};

export default eventBus;
```

## useEvent.js (Safe Subscription Hook)

```js
import { useEffect } from 'react';
import eventBus from './eventBus';

export function useEvent(event, handler) {
  useEffect(() => {
    const unsubscribe = eventBus.subscribe(event, handler);
    return unsubscribe;
  }, [event, handler]);
}
```

## Publisher (Emit Notifications)

```js
import eventBus from './eventBus';

function NotificationButtons() {
  return (
    <>
      <button
        onClick={() =>
          eventBus.publish('NOTIFY', {
            type: 'success',
            message: 'Saved successfully',
          })
        }
      >
        Success
      </button>

      <button
        onClick={() =>
          eventBus.publish('NOTIFY', {
            type: 'error',
            message: 'Something went wrong',
          })
        }
      >
        Error
      </button>

      <button
        onClick={() =>
          eventBus.publish('NOTIFY', {
            type: 'warning',
            message: 'Be careful',
          })
        }
      >
        Warning
      </button>
    </>
  );
}
```

## Subscriber (Notification Panel)

```js
import { useState, useEffect } from 'react';
import { useEvent } from './useEvent';

function NotificationPanel() {
  const [notifications, setNotifications] = useState(() => {
    return JSON.parse(localStorage.getItem('notifications') || '[]');
  });

  useEvent('NOTIFY', (notification) => {
    setNotifications((prev) => [...prev, notification]);
  });

  useEffect(() => {
    localStorage.setItem('notifications', JSON.stringify(notifications));
  }, [notifications]);

  const dismiss = (index) => {
    setNotifications((prev) => prev.filter((_, i) => i !== index));
  };

  return (
    <div>
      {notifications.map((n, i) => (
        <div key={i}>
          <strong>{n.type.toUpperCase()}</strong>: {n.message}
          <button onClick={() => dismiss(i)}>×</button>
        </div>
      ))}
    </div>
  );
}
```

## Auto-Hide (Optional Enhancement)

```ts
useEvent('NOTIFY', (notification) => {
  setNotifications((prev) => [...prev, notification]);

  setTimeout(() => {
    setNotifications((prev) => prev.slice(1));
  }, 3000);
});
```

## When to Use Pub-Sub (Validated by This Example)

- Notifications, alerts, analytics
- Cross-app or micro-frontend communication
- No shared component tree
- Loose coupling is required

---

<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-10/pub-sub-pattern/src -->
