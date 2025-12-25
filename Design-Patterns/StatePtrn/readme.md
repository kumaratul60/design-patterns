# State Pattern

## Overview

The State Pattern is a **behavioral pattern** that lets an object change its behavior when its internal state changes. It's similar to the Strategy Pattern but the transitions are controlled by the object's state rather than external selection.

## Core Idea

- Allow an object to alter its behavior when its internal state changes
- Encapsulate state-specific behavior in separate classes
- Make state transitions explicit and manageable

## When to Use

- UI components with modes (idle/loading/error), order lifecycle (draft/paid/shipped), auth guards
- When an object's behavior depends on its state
- To avoid large conditional blocks based on state

## When NOT to Use

- When state transitions are simple and don't affect behavior much
- For stateless objects
- If the number of states is very large

## Core Rules & Responsibilities

- Define a state interface that all concrete states implement
- Create concrete state classes for each possible state
- Allow the context object to delegate behavior to the current state

## Minimal Example

```javascript
function createTrafficLight() {
  let state = 'red';
  const actions = {
    red: () => { state = 'green'; console.log('Go!'); },
    yellow: () => { state = 'red'; console.log('Stop!'); },
    green: () => { state = 'yellow'; console.log('Caution!'); }
  };
  return {
    change() { actions[state](); },
    getState() { return state; }
  };
}
```

## Real-World Problem

Imagine building an async button that behaves differently based on its loading state. Without the state pattern, you'd have complex conditionals throughout the code.

## Solution

The State Pattern encapsulates each state's behavior, making state transitions clean and the code more maintainable.

## Code Snippet

```javascript
function createAsyncButton() {
  let state = 'idle'; // idle | loading | success | error
  const actions = {
    idle: () => {
      state = 'loading';
      console.log('Starting request...');
    },
    loading: () => {
      console.log('Already loading...');
    },
    success: () => {
      state = 'idle';
      console.log('Success! Reset to idle.');
    },
    error: () => {
      state = 'idle';
      console.log('Error! Reset to idle.');
    },
  };
  return {
    click() {
      actions[state]();
    },
    setState(s) {
      state = s;
    },
    getState() {
      return state;
    },
  };
}

const button = createAsyncButton();
button.click(); // Starting request...
button.click(); // Already loading...
button.setState('success');
button.click(); // Success! Reset to idle.
```

## When to Use

- When an object's behavior changes based on its internal state
- To eliminate state-dependent conditional logic
- When state transitions need to be explicit and controlled

## Pros & Cons

- **Pros:**
    - Removes giant `if/else` blocks
    - Makes state transitions explicit
    - Follows Single Responsibility Principle

- **Cons:**
    - Can introduce many small classes for complex state machines
    - May be overkill for simple state logic
    - Requires careful management of state transitions

## Common Pitfalls

- Creating too many state classes
- Allowing invalid state transitions
- Making states too interdependent
- Forgetting to update state references

## Rule of Thumb

Use the State Pattern when an object's behavior changes significantly based on its state and you want to avoid complex conditional logic.