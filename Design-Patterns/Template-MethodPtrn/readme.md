# Template Method Pattern

## Overview

The Template Method Pattern is a **behavioral pattern** that defines the skeleton of an algorithm in a method, deferring some steps to subclasses. It lets subclasses redefine certain steps without changing the algorithm's structure.

## Core Idea

- Define algorithm skeleton in base class
- Allow subclasses to customize specific steps
- Maintain overall algorithm structure

## When to Use

- Similar workflows with pluggable steps
- When you want to avoid code duplication
- To enforce algorithm structure

## When NOT to Use

- When algorithms are completely different
- If subclasses need to change the overall flow
- For simple, non-repeating processes

## Core Rules & Responsibilities

- Create abstract base class with template method
- Define hook methods for customization
- Ensure subclasses implement required steps

## Minimal Example

```javascript
class BaseAlgorithm {
  templateMethod() {
    this.step1();
    this.step2();
    this.hook();
  }
  step1() { /* default */ }
  step2() { /* default */ }
  hook() { /* optional */ }
}
```

## Real-World Problem

Imagine data import processes that have similar steps but different validation or transformation logic. Without templates, you'd duplicate the workflow code.

## Solution

The Template Method defines the import skeleton, allowing subclasses to customize validation and transformation while keeping the overall process the same.

## Code Snippet

```javascript
function runImport({ before = async () => {}, transform, after = async () => {} }) {
  return async function (input) {
    await before(input);
    const out = await transform(input);
    await after(out);
    return out;
  };
}

const importCsv = runImport({
  before: async () => console.log('start'),
  transform: async (csv) =>
    csv
      .trim()
      .split('\n')
      .map((r) => r.split(',')),
  after: async (rows) => console.log('rows:', rows.length),
});
```

## When to Use

- When you have invariant behavior mixed with variant behavior
- To avoid code duplication in similar algorithms
- When subclasses should customize steps but not the overall flow

## Pros & Cons

- **Pros:**
    - Code reuse through inheritance
    - Enforces algorithm structure
    - Easy to add new variations

- **Cons:**
    - Can lead to complex inheritance hierarchies
    - Tightly coupled to base class
    - Hook methods may not be used

## Common Pitfalls

- Making template methods too rigid
- Forgetting to call parent methods
- Overusing inheritance when composition would work

## Rule of Thumb

Use the Template Method Pattern when you have a fixed algorithm structure with customizable steps.