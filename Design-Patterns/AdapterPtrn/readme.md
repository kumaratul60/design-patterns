# Adapter Pattern

## Overview

The Adapter Pattern is a **structural pattern** that converts one interface to another that clients expect. It allows incompatible interfaces to work together by wrapping one interface with another.

## Core Idea

- Convert incompatible interfaces to compatible ones
- Wrap existing code to match expected interface
- Enable integration of third-party libraries

## When to Use

- Swap a library/service without rewriting callers
- When you need to use existing code with incompatible interfaces
- To create a common interface for different implementations

## When NOT to Use

- When interfaces are already compatible
- For new code where you can design interfaces properly
- If adaptation logic becomes too complex

## Core Rules & Responsibilities

- Define a target interface that clients expect
- Create an adapter class that implements the target interface
- Translate calls from target interface to adaptee interface

## Minimal Example

```javascript
class Adapter {
  constructor(adaptee) {
    this.adaptee = adaptee;
  }
  targetMethod() {
    return this.adaptee.differentMethod();
  }
}
```

## Real-World Problem

Imagine integrating two different analytics SDKs that have different APIs. Your code expects a common interface, but the SDKs are incompatible.

## Solution

The Adapter Pattern creates wrapper objects that translate between the expected interface and the actual SDK interfaces.

## Code Snippet

```javascript
function makeAnalyticsAdapter(sdk) {
  return {
    track(event, props) {
      if (sdk.logEvent) return sdk.logEvent(event, props); // Firebase style
      if (sdk.trackEvent) return sdk.trackEvent({ name: event, properties: props }); // Other SDK
      throw new Error('Unsupported SDK');
    },
  };
}

const analytics = makeAnalyticsAdapter(window.thirdPartySdk);
analytics.track('signup', { plan: 'pro' });
```

## When to Use

- To integrate third-party libraries with different interfaces
- When you need to change interfaces without affecting clients
- For legacy code integration

## Pros & Cons

- **Pros:**
    - Enables integration of incompatible interfaces
    - Follows Open/Closed Principle
    - Isolates interface changes

- **Cons:**
    - Can add complexity and indirection
    - May leak if adapter is incomplete
    - Requires defining contract clearly

## Common Pitfalls

- Incomplete adapters that don't handle all cases
- Adapters that become too complex
- Not properly documenting the interface contract

## Rule of Thumb

Use the Adapter Pattern when you need to make incompatible interfaces work together without changing existing code.