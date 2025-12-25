# Chain of Responsibility Pattern

## Overview

The Chain of Responsibility is a **behavioral pattern** that lets you pass a request along a **chain of potential handlers**. Upon receiving a request, each handler decides either to **process the request** or to **pass it to the next handler** in the chain.

## Core Idea

- Create a chain of handlers for processing requests
- Each handler can process or pass to next
- Decouple sender from specific handler

## When to Use

- Validation pipelines, middleware stacks
- When handlers are determined at runtime
- To avoid coupling sender to receivers

## When NOT to Use

- When there's only one possible handler
- If the chain is always the same and simple
- For performance-critical code

## Core Rules & Responsibilities

- Define a handler interface with handle/setNext methods
- Each handler implements its own logic and decides to handle or pass
- Allow dynamic chain construction

## Minimal Example

```javascript
class Handler {
  setNext(handler) {
    this.next = handler;
    return handler;
  }
  handle(request) {
    if (this.next) return this.next.handle(request);
  }
}
```

## Real-World Problem

Imagine a web request that needs multiple processing steps: authentication, validation, logging, business logic. Without the pattern, you'd have nested conditionals.

## Solution

The Chain of Responsibility allows each step to be a separate handler that can process or pass the request along.

## Code Snippet

```javascript
// The "Handler" Interface (or base class)
class AbstractValidator {
    constructor() {
        this.nextValidator = null;
    }

    setNext(validator) {
        this.nextValidator = validator;
        return validator; // Return the next validator to allow for chaining setup
    }

    handle(data) {
        if (this.nextValidator) {
            return this.nextValidator.handle(data);
        }
        return { valid: true };
    }
}

// --- Concrete Handlers ---

class RequiredFieldsValidator extends AbstractValidator {
    handle(data) {
        console.log("Checking for required fields...");
        if (!data.username || !data.email || !data.password) {
            return { valid: false, error: 'Required fields are missing.' };
        }
        return super.handle(data);
    }
}

class EmailFormatValidator extends AbstractValidator {
    handle(data) {
        console.log("Checking email format...");
        const emailRegex = /^\S+@\S+\.\S+$/;
        if (!emailRegex.test(data.email)) {
            return { valid: false, error: 'Invalid email format.' };
        }
        return super.handle(data);
    }
}

class PasswordStrengthValidator extends AbstractValidator {
    handle(data) {
        console.log("Checking password strength...");
        if (data.password.length < 8) {
            return { valid: false, error: 'Password must be at least 8 characters long.' };
        }
        return super.handle(data);
    }
}

// --- USAGE ---

const required = new RequiredFieldsValidator();
const email = new EmailFormatValidator();
const password = new PasswordStrengthValidator();

// Build the chain
required.setNext(email).setNext(password);

// Start the process
console.log("--- Testing valid data ---");
const validData = { username: 'john_doe', email: 'john@test.com', password: 'password123' };
console.log(required.handle(validData));

console.log("\n--- Testing invalid email ---");
const invalidEmailData = { username: 'jane_doe', email: 'jane@test', password: 'password123' };
console.log(required.handle(invalidEmailData));
```

## Modern Implementation

```javascript
function chain(...handlers) {
  return (ctx) => handlers.reduce((c, h) => h(c), ctx);
}

const withAuth = (ctx) => {
  if (!ctx.user) throw new Error('unauthorized');
  return ctx;
};
const withRole = (role) => (ctx) => {
  if (ctx.user.role !== role) throw new Error('forbidden');
  return ctx;
};

const handler = chain(withAuth, withRole('admin'), (ctx) => ({ ok: true }));
```

## When to Use the Chain of Responsibility Pattern

- When more than one object may handle a request, and the handler isn't known beforehand. The handler is determined automatically at runtime.
- When you want to issue a request to one of several objects without specifying the receiver explicitly.
- When the set of objects that can handle a request should be specified dynamically.

## Pros & Cons

- **Pros:**
    - **Loose Coupling:** The sender only knows about the first handler in the chain. Handlers don't need to know about each other, only how to pass a request to the *next* one.
    - **Follows SOLID Principles:** Adheres to the Single Responsibility Principle (each handler does one thing) and the Open/Closed Principle (you can add new handlers to the chain without modifying existing ones).
    - **Increased Flexibility:** You can change the order of the chain or add/remove handlers dynamically at runtime.

- **Cons:**
    - **Request May Go Unhandled:** There's no guarantee that a request will be handled if it falls off the end of the chain.
    - **Debugging Can Be Difficult:** It can be hard to trace the flow of a request and observe which handler processed it, especially with long chains.
    - **Circular Chains:** Care must be taken to avoid creating a circular reference where a request gets passed around in a loop forever.

## Common Pitfalls

- Not handling the case where no handler processes the request
- Creating circular references in the chain
- Making handlers too interdependent
- Debugging which handler processed a request

## Rule of Thumb

Use the Chain of Responsibility Pattern when you have a series of potential handlers for a request and want to decouple the sender from the specific handler.