# Flyweight Pattern

## Overview

The Flyweight Pattern is a **structural pattern** that shares intrinsic state between many similar objects to save memory. It separates intrinsic (shared) state from extrinsic (unique) state.

## Core Idea

- Share common data between multiple objects
- Store intrinsic state in flyweight objects
- Pass extrinsic state as parameters

## When to Use

- Thousands of similar objects (e.g., map markers, document characters) where most data is shared
- When memory usage is a concern
- For optimizing large numbers of similar objects

## When NOT to Use

- When objects have mostly unique state
- If memory isn't a bottleneck
- For simple objects where sharing adds complexity

## Core Rules & Responsibilities

- Identify intrinsic vs extrinsic state
- Create flyweight factory to manage shared instances
- Store shared state in flyweights, unique state externally

## Minimal Example

```javascript
const flyweightFactory = {
  flyweights: {},
  get(key) {
    if (!this.flyweights[key]) {
      this.flyweights[key] = { sharedData: key };
    }
    return this.flyweights[key];
  }
};
```

## Real-World Problem

Imagine rendering thousands of emoji in a chat app. Without flyweights, each emoji object would store the same image URL repeatedly.

## Solution

The Flyweight Pattern shares the image URL between all instances of the same emoji, storing only unique position data separately.

## Code Snippet

```javascript
const EmojiMeta = (function () {
  const cache = new Map(); // name -> meta
  return {
    get(name) {
      if (!cache.has(name)) cache.set(name, { url: `/emoji/${name}.png`, size: 24 });
      return cache.get(name);
    },
  };
})();

function createMessage(text, emojis) {
  return { text, emojis: emojis.map((n) => EmojiMeta.get(n)) };
}
```

## When to Use

- When you have many objects with shared state
- To reduce memory usage
- When extrinsic state can be computed or passed

## Pros & Cons

- **Pros:**
    - Memory win for large numbers of similar objects
    - Can improve performance by reducing object creation
    - Separates concerns between shared and unique state

- **Cons:**
    - Complexity increases
    - Requires careful state separation
    - Can be overkill for small numbers of objects

## Common Pitfalls

- Mixing intrinsic and extrinsic state
- Not properly managing the flyweight pool
- Performance overhead of lookups vs memory savings

## Rule of Thumb

Use the Flyweight Pattern when you have many similar objects and memory optimization is critical.