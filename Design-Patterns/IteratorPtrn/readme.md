# Iterator Pattern

## Overview

The Iterator Pattern is a **behavioral pattern** that provides a way to access elements of an aggregate sequentially without exposing its internal representation.

## Core Idea

- Provide a standard way to traverse collections
- Hide internal data structures
- Allow multiple traversal algorithms

## When to Use

- Custom data sources, paginated APIs
- When you need different traversal methods
- To hide complex data structures

## When NOT to Use

- For simple arrays (use built-in iterators)
- When traversal is always the same
- If exposing internals is not a concern

## Core Rules & Responsibilities

- Define an iterator interface with next/hasNext methods
- Implement concrete iterators for different traversal needs
- Provide a way to get iterators from collections

## Minimal Example

```javascript
function createIterator(array) {
  let index = 0;
  return {
    next() {
      return index < array.length ? { value: array[index++], done: false } : { done: true };
    }
  };
}
```

## Real-World Problem

Imagine iterating over a paginated API where data comes in chunks. Without iterators, clients would need to handle pagination logic.

## Solution

The Iterator Pattern provides a uniform interface for traversing different data sources, abstracting away the complexity.

## Code Snippet

```javascript
async function* fetchPages(url) {
  let next = url;
  while (next) {
    const res = await fetch(next).then((r) => r.json());
    yield res.items;
    next = res.nextPageUrl; // or null
  }
}

(async () => {
  for await (const items of fetchPages('/api/list')) {
    // process each page
  }
})();
```

## When to Use

- For traversing complex data structures
- When you need multiple iteration strategies
- To provide a consistent iteration interface

## Pros & Cons

- **Pros:**
    - Clean consumption of data
    - Hides implementation details
    - Allows multiple iteration patterns

- **Cons:**
    - Can be overkill for simple collections
    - Async iterators require modern runtime
    - Additional abstraction layer

## Common Pitfalls

- Implementing iterators for simple use cases
- Not handling iteration termination properly
- Creating stateful iterators that can't be reused

## Rule of Thumb

Use the Iterator Pattern when you need to provide a standard way to traverse complex data structures or when built-in iterators aren't sufficient.