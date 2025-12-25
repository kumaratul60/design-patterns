# Revealing Module Pattern

## Overview

The Revealing Module Pattern is a variation of the Module Pattern that makes it more explicit which functions and variables are public and private. It defines all functions and variables privately, then returns an object that maps private names to public ones.

## Core Idea

- Define all functions and variables inside the module's private scope
- Return an object that reveals only the public interface
- Map private function names to public properties for clarity

## When to Use

- When you want a consistent style with all functions at the top and exports at the bottom
- To make it obvious what is public vs private
- When refactoring existing code to improve encapsulation

## When NOT to Use

- For very simple modules with few functions
- When ES6 modules provide a cleaner syntax
- If you need complex inheritance patterns

## Core Rules & Responsibilities

- Declare all functions and variables at the top level of the module
- Create a return object that explicitly maps private members to public ones
- Use descriptive names for public methods to indicate their purpose

## Minimal Example

```javascript
const Calculator = (function() {
    // Private variables
    let result = 0;

    // Private functions
    function add(a, b) {
        return a + b;
    }

    function multiply(a, b) {
        return a * b;
    }

    // Reveal public pointers to private functions
    return {
        sum: add,
        product: multiply,
        getResult: function() { return result; },
        setResult: function(value) { result = value; }
    };
})();
```

## Real-World Problem

Imagine you're building a token storage utility for an authentication system. You want to expose only safe methods for setting and getting tokens, while keeping the storage mechanism private.

## Solution

The Revealing Module Pattern provides a clear separation between what's internal and what's exposed, making the public API explicit.

## Code Snippet

```javascript
const TokenStore = (function () {
  const KEY = 'app:token'; // private
  function set(token) {
    localStorage.setItem(KEY, token);
  }
  function get() {
    return localStorage.getItem(KEY);
  }
  function clear() {
    localStorage.removeItem(KEY);
  }

  // reveal what's public
  return { setToken: set, getToken: get, clear };
})();

TokenStore.setToken('abc123');
console.log(TokenStore.getToken()); // 'abc123'
```

## When to Use

- Keep a consistent style: all functions at top, exports at bottom
- Make it obvious what's public
- Improve code readability and maintainability

## Pros & Cons

- **Pros:**
    - Clear separation between public and private
    - All functions defined at the top for easy reading
    - Explicit public API declaration

- **Cons:**
    - Same as Module Pattern: harder to test private parts, not easily extensible
    - Slightly more verbose than standard module pattern
    - Circular references can be harder to manage

## Common Pitfalls

- Forgetting to map private functions to public properties
- Accidental exposure of private variables
- Confusion between private function names and public property names

## Rule of Thumb

Use the Revealing Module Pattern when you want maximum clarity about what's public vs private in your module. It's particularly useful for larger modules with many functions.