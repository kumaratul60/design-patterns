# Decorator Pattern

## Overview

The Decorator Pattern is a **structural pattern** that allows you to **dynamically add new behaviors or responsibilities to an object** without modifying its source code. It achieves this by wrapping the original object in a special "decorator" object that shares the same interface.

## Core Idea

- Wrap objects to add behavior dynamically
- Maintain the same interface as the wrapped object
- Compose decorators for multiple enhancements

## When to Use

- Add logging, caching, retry, rate limiting to existing functions/clients without modifying them
- When you need to extend functionality at runtime
- To avoid subclass explosion

## When NOT to Use

- When behavior is fixed at creation time
- For simple static extensions
- If the interface needs to change

## Core Rules & Responsibilities

- Define a common interface for components and decorators
- Create decorator classes that wrap components
- Forward calls to wrapped objects while adding behavior

## Minimal Example

```javascript
function withLogging(fn) {
  return function(...args) {
    console.log('Calling function');
    const result = fn.apply(this, args);
    console.log('Function completed');
    return result;
  };
}
```

## Real-World Problem

Imagine you have a data fetching function that you want to add caching and error retry logic to. Without decorators, you'd modify the function directly or create multiple versions.

## Solution

The Decorator Pattern allows you to wrap the original function with additional behaviors without changing its code.

## Code Snippet

```javascript
function withCache(fn, ttlMs = 5_000) {
  const cache = new Map();
  return async function (key) {
    const hit = cache.get(key);
    const now = Date.now();
    if (hit && now - hit.ts < ttlMs) return hit.value;
    const value = await fn(key);
    cache.set(key, { value, ts: now });
    return value;
  };
}

const fetchUser = async (id) => (await fetch(`/api/users/${id}`)).json();
const fetchUserCached = withCache(fetchUser, 10_000);
```

## Class-based Implementation

```javascript
// The "Component" Interface (the base object)
class Car {
    constructor() {
        this.cost = 20000;
        this.description = 'Basic Car';
    }

    getCost() {
        return this.cost;
    }

    getDescription() {
        return this.description;
    }
}

// --- Decorators ---
class Sunroof {
    constructor(car) {
        this.car = car;
    }

    getCost() {
        return this.car.getCost() + 2000;
    }

    getDescription() {
        return this.car.getDescription() + ', with a Sunroof';
    }
}

class LeatherSeats {
    constructor(car) {
        this.car = car;
    }

    getCost() {
        return this.car.getCost() + 2500;
    }

    getDescription() {
        return this.car.getDescription() + ', with Leather Seats';
    }
}

// --- USAGE ---

let myCar = new Car();
console.log(`Cost: $${myCar.getCost()}, Description: ${myCar.getDescription()}`);

myCar = new Sunroof(myCar);
console.log(`Cost: $${myCar.getCost()}, Description: ${myCar.getDescription()}`);

myCar = new LeatherSeats(myCar);
console.log(`Cost: $${myCar.getCost()}, Description: ${myCar.getDescription()}`);
```

## When to Use the Decorator Pattern

- When you want to add responsibilities to objects dynamically without creating a complex hierarchy of subclasses.
- When you want to avoid feature-laden classes. You can move optional functionality into decorators.
- When the core object's functionality is fixed, but you need to combine it with various extensions. For example, adding different types of logging or validation to a data source.

## Pros & Cons

- **Pros:**
    - **Great Flexibility:** Far more flexible than inheritance. You can add any number of decorators in any combination at runtime.
    - **Adheres to SOLID Principles:** Follows the Single Responsibility Principle (each decorator does one thing) and the Open/Closed Principle (you can add new decorators without changing existing code).
    - **Clean Code:** Keeps your component classes lean and focused on their core task.

- **Cons:**
    - **Increased Complexity:** Can lead to a lot of small, similar-looking objects in your system, which can be hard to manage.
    - **Debugging Can Be Tricky:** Tracing behavior through a long chain of decorators can be difficult.
    - **Order Matters:** The order in which decorators are applied can affect the outcome, which may not be intuitive.

## Common Pitfalls

- Creating decorators that don't properly forward all methods
- Breaking the interface contract
- Over-nesting decorators making debugging hard
- Forgetting that decorators can be composed

## Rule of Thumb

Use the Decorator Pattern when you need to add optional behaviors to objects at runtime without modifying their classes.