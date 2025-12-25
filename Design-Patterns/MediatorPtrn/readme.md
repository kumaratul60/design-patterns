# Mediator Pattern

## Overview

The Mediator Pattern is a **behavioral pattern** that encapsulates how a set of objects interact; it promotes loose coupling by preventing direct references between objects.

## Core Idea

- Central mediator coordinates communication between components
- Components don't communicate directly with each other
- Reduces dependencies between interacting objects

## When to Use

- Complex UIs where components must coordinate (wizard steps, chat presence + typing + messages)
- When many objects need to communicate in complex ways
- To reduce coupling between components

## When NOT to Use

- For simple direct interactions
- When components have clear hierarchical relationships
- If the mediator becomes a bottleneck

## Core Rules & Responsibilities

- Define a mediator interface
- Create a concrete mediator that coordinates interactions
- Components communicate through the mediator instead of directly

## Minimal Example

```javascript
class ChatMediator {
  constructor() {
    this.users = [];
  }
  addUser(user) {
    this.users.push(user);
  }
  sendMessage(message, sender) {
    this.users.filter(u => u !== sender).forEach(u => u.receive(message));
  }
}
```

## Real-World Problem

Imagine a complex form with multiple interdependent fields (country affects state options, state affects city). Without a mediator, fields would need to know about each other, creating tight coupling.

## Solution

The Mediator Pattern provides a central coordinator that manages interactions between components, keeping them loosely coupled.

## Code Snippet

```javascript
function createWizardMediator(steps) {
  let current = 0;
  return {
    next() {
      if (steps[current].validate()) current++;
      steps[current]?.render();
    },
    prev() {
      if (current > 0) current--;
      steps[current].render();
    },
    currentIndex() {
      return current;
    },
  };
}

// Usage
const steps = [
  { validate: () => true, render: () => console.log('Step 1') },
  { validate: () => true, render: () => console.log('Step 2') },
];
const mediator = createWizardMediator(steps);
mediator.next(); // Step 2
```

## When to Use

- When components need to coordinate in complex ways
- To centralize communication logic
- When direct component coupling would be problematic

## Pros & Cons

- **Pros:**
    - Reduces coupling between components
    - Centralizes complex communication logic
    - Makes components more reusable

- **Cons:**
    - Mediator can become complex and hard to maintain
    - May become a performance bottleneck
    - Can hide the flow of communication

## Common Pitfalls

- Making the mediator too large and complex
- Components becoming dependent on the mediator
- Overusing the pattern for simple interactions

## Rule of Thumb

Use the Mediator Pattern when you have complex interactions between multiple objects that would otherwise be tightly coupled.