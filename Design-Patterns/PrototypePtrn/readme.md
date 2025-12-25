# Prototype Pattern

## Overview

The Prototype Pattern is a **creational pattern** that creates new objects by cloning existing ones (prototypal inheritance). It allows you to create new instances by copying existing objects rather than creating them from scratch.

## Core Idea

- Create objects by cloning existing prototypes
- Avoid expensive initialization by copying
- Share common structure between objects

## When to Use

- Many objects share shape/behavior; you want cheap cloning
- When object creation is expensive
- To avoid subclassing for variation

## When NOT to Use

- When objects are simple to create
- If deep cloning is complex or unnecessary
- For immutable objects

## Core Rules & Responsibilities

- Define a prototype with shared properties/methods
- Provide a clone method to create new instances
- Handle deep vs shallow cloning appropriately

## Minimal Example

```javascript
const prototype = {
  clone() {
    return Object.assign(Object.create(Object.getPrototypeOf(this)), this);
  }
};

const obj1 = Object.assign(Object.create(prototype), { value: 1 });
const obj2 = obj1.clone();
```

## Real-World Problem

Imagine creating many similar configuration objects. Without prototypes, you'd repeat initialization code for each object.

## Solution

The Prototype Pattern allows you to clone a base configuration and customize only the differences.

## Code Snippet

```javascript
const rowProto = {
  activate() {
    this.active = true;
  },
  deactivate() {
    this.active = false;
  },
};

function createRow(data) {
  return Object.assign(Object.create(rowProto), { ...data, active: false });
}

const r1 = createRow({ id: 1, name: 'Alice' });
const r2 = Object.create(Object.getPrototypeOf(r1));
Object.assign(r2, { id: 2, name: 'Bob', active: false });
```

## When to Use

- When many similar objects need to be created
- To avoid expensive object initialization
- For creating variations of existing objects

## Pros & Cons

- **Pros:**
    - Lightweight reuse of object structure
    - Avoids expensive initialization
    - Can be more flexible than class inheritance

- **Cons:**
    - Deep cloning can be complex
    - `this` and prototype chains can be confusing
    - Requires careful handling of references

## Common Pitfalls

- Shallow cloning when deep cloning is needed
- Not properly setting up prototype chains
- Confusing prototype inheritance with class inheritance

## Rule of Thumb

Use the Prototype Pattern when you need to create many similar objects and object creation is expensive or complex.