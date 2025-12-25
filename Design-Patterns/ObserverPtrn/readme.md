# Observer Pattern

## Overview

The Observer Pattern is a **behavioral pattern** that defines a **one-to-many dependency** between objects. When one object (the "Subject" or "Observable") changes its state, all of its dependents (the "Observers") are notified and updated automatically.

## Core Idea

- Subject maintains a list of observers
- When subject's state changes, it notifies all observers
- Observers can subscribe/unsubscribe dynamically

## When to Use

- Decouple data producers from UI, analytics, or logging
- When changes in one object require changes in others
- For event-driven architectures

## When NOT to Use

- When the relationship is simple and direct
- For performance-critical code with many observers
- If notifications are not needed

## Core Rules & Responsibilities

- Subject provides methods to attach/detach observers
- Subject notifies observers when state changes
- Observers implement an update interface

## Minimal Example

```javascript
class Subject {
  constructor() {
    this.observers = [];
  }
  subscribe(observer) {
    this.observers.push(observer);
  }
  notify(data) {
    this.observers.forEach(obs => obs.update(data));
  }
}
```

## Real-World Problem

Imagine building a shopping cart where multiple UI components (header icon, mini-cart, checkout) need to update when items are added. Without the observer pattern, you'd have tight coupling between components.

## Solution

The Observer Pattern allows components to subscribe to cart changes, decoupling the cart logic from UI updates.

## Code Snippet

```javascript
// The Subject (the thing being watched)
class ShoppingCart {
    constructor() {
        this.observers = [];
        this.items = [];
    }

    // Add an observer (component) to the list
    subscribe(observer) {
        this.observers.push(observer);
    }

    // Remove an observer from the list
    unsubscribe(observer) {
        this.observers = this.observers.filter(obs => obs !== observer);
    }

    // Notify all observers about a change
    notify(data) {
        this.observers.forEach(observer => observer.update(data));
    }

    // A method that changes the state and triggers notification
    addItem(item) {
        this.items.push(item);
        console.log(`--- Added "${item.name}" to cart ---`);
        // Notify everyone that the cart has changed!
        this.notify({ items: this.items, count: this.items.length });
    }
}

// An Observer (a thing that watches)
class HeaderCartIcon {
    constructor() {
        this.count = 0;
    }

    // This method is called by the Subject when it's updated
    update(data) {
        this.count = data.count;
        console.log(`Header Icon: Cart count is now ${this.count}.`);
        // In a real app, you would update the DOM here.
    }
}

// Another Observer
class NotificationService {
    update(data) {
        const latestItem = data.items[data.items.length - 1];
        console.log(`Notification: "${latestItem.name}" was added to your cart.`);
    }
}


// --- USAGE ---

// 1. Create the Subject instance
const cart = new ShoppingCart();

// 2. Create the Observer instances
const headerIcon = new HeaderCartIcon();
const notificationService = new NotificationService();

// 3. Subscribe the observers to the subject
cart.subscribe(headerIcon);
cart.subscribe(notificationService);

// 4. Trigger state changes in the subject
cart.addItem({ name: 'Laptop', price: 1500 });
cart.addItem({ name: 'Mouse', price: 50 });

// Let's unsubscribe the notification service and add another item
console.log('\n--- Unsubscribing notifications ---');
cart.unsubscribe(notificationService);
cart.addItem({ name: 'Keyboard', price: 100 });

// Output:
// --- Added "Laptop" to cart ---
// Header Icon: Cart count is now 1.
// Notification: "Laptop" was added to your cart.
// --- Added "Mouse" to cart ---
// Header Icon: Cart count is now 2.
// Notification: "Mouse" was added to your cart.
//
// --- Unsubscribing notifications ---
// --- Added "Keyboard" to cart ---
// Header Icon: Cart count is now 3.
// (Notice: No notification was logged this time!)
```

## Modern Implementation

```javascript
function createPubSub() {
  const topics = new Map(); // topic -> Set(listeners)
  return {
    subscribe(topic, listener) {
      if (!topics.has(topic)) topics.set(topic, new Set());
      topics.get(topic).add(listener);
      return () => topics.get(topic)?.delete(listener); // unsubscribe
    },
    publish(topic, data) {
      topics.get(topic)?.forEach((l) => l(data));
    },
  };
}

const bus = createPubSub();
const off = bus.subscribe('user:login', (u) => console.log('welcome', u.name));
bus.publish('user:login', { name: 'Atul' });
off();
```

## When to Use the Observer Pattern

- **UI Development:** Keeping different parts of a UI synchronized with the application's state (this is the core of libraries/frameworks like React, Vue, and Angular).
- **Event Systems:** The native DOM `addEventListener` is a perfect example of the Observer pattern. The DOM element is the subject, and your event listener function is the observer.
- **State Management:** Centralized state stores (like Redux or Vuex) use this pattern. Components subscribe to changes in the state store and re-render when the state is updated.
- **Real-time Data:** Pushing updates from a server to multiple clients (e.g., stock tickers, live sports scores).

## Pros & Cons

- **Pros:**
    - **Loose Coupling:** The Subject knows nothing about the concrete Observers, other than that they implement the `update` method. This makes your system more flexible and reusable.
    - **Open/Closed Principle:** You can introduce new observer types without having to modify the subject's code.
    - **Dynamic Relationships:** You can add or remove observers at any time during runtime.

- **Cons:**
    - **Performance Issues:** If there are many observers or the update logic is complex, the notification process can be slow.
    - **Unexpected Update Order:** The subject typically notifies observers in an unpredictable order. If the order is important, you may need a more complex implementation.
    - **Memory Leaks:** A common pitfall is the "lapsed listener" problem. If an observer is destroyed but not unsubscribed, the subject will hold a reference to it, preventing it from being garbage collected.

## Common Pitfalls

- Forgetting to unsubscribe observers, causing memory leaks
- Not handling observer errors that could break the notification chain
- Making observers too tightly coupled to subjects
- Overusing the pattern for simple direct relationships

## Rule of Thumb

Use the Observer Pattern when you need to notify multiple objects about changes in another object. For simple relationships, direct method calls may suffice.