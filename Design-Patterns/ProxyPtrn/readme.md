# Proxy Pattern

## Overview

The Proxy Pattern is a **structural pattern** that provides a **substitute or placeholder for another object to control access to it**. The Proxy object has the exact same interface as the "real" object it is proxying, allowing it to be used interchangeably. This lets the proxy perform actions—like checking permissions, caching data, or logging—before or after the request gets to the original object.

## Core Idea

- Provide a surrogate object that controls access to another object
- Maintain the same interface as the real object
- Add behavior like caching, logging, or access control

## When to Use

- Lazy loading, access control, input validation, property watching
- When you need to control access to expensive resources
- To add cross-cutting concerns like logging or caching

## When NOT to Use

- When direct access is always appropriate
- For simple objects where indirection adds no value
- If performance overhead is critical

## Core Rules & Responsibilities

- Implement the same interface as the real subject
- Control access to the real subject
- May create the real subject lazily

## Minimal Example

```javascript
class Proxy {
  constructor(realSubject) {
    this.realSubject = realSubject;
  }
  request() {
    console.log('Proxy: Checking access...');
    return this.realSubject.request();
  }
}
```

## Real-World Problem

Imagine a file system API that needs access control. Without a proxy, every client would need to implement permission checks.

## Solution

The Proxy Pattern provides a wrapper that handles access control transparently, keeping the file system interface clean.

## Code Snippet

```javascript
// 1. The "Real Subject" - The object we want to control access to.
class RealFileSystem {
    readFile(path) {
        return `Contents of ${path}`;
    }

    writeFile(path, content) {
        console.log(`Writing "${content}" to ${path}`);
        return true;
    }
}

// 2. The "Proxy" - It has the same interface as the Real Subject.
class SecureFileSystemProxy {
    constructor(realFileSystem, user) {
        this.fileSystem = realFileSystem;
        this.user = user;
    }

    readFile(path) {
        return this.fileSystem.readFile(path);
    }

    writeFile(path, content) {
        if (this.user.role === 'admin') {
            return this.fileSystem.writeFile(path, content);
        } else {
            console.error("ACCESS DENIED: You do not have permission to write.");
            return false;
        }
    }
}

// --- USAGE ---

const realFS = new RealFileSystem();
const regularUser = { name: 'John', role: 'user' };
const adminUser = { name: 'Jane', role: 'admin' };

// Create a proxy for the regular user.
const userFileSystem = new SecureFileSystemProxy(realFS, regularUser);

// Create a proxy for the admin user.
const adminFileSystem = new SecureFileSystemProxy(realFS, adminUser);

console.log('--- Regular User Actions ---');
console.log(userFileSystem.readFile('config.txt')); // Works
userFileSystem.writeFile('app.log', 'Error occurred');   // Fails

console.log('\n--- Admin User Actions ---');
console.log(adminFileSystem.readFile('config.txt'));   // Works
adminFileSystem.writeFile('app.log', 'System shutdown'); // Works
```

## Modern ES6 Proxy

```javascript
function createStrictConfig(config) {
  return new Proxy(config, {
    get(target, prop) {
      if (!(prop in target)) throw new Error(`Unknown config key: ${String(prop)}`);
      return target[prop];
    },
  });
}

const Config = createStrictConfig({ API_URL: '/api' });
// Config.NON_EXISTENT -> throws
```

## When to Use the Proxy Pattern

- **Virtual Proxy:** Lazy loading. The proxy can represent a large object (like a high-res image or complex data). It only loads the real object when it's actually needed.
- **Protection Proxy:** Controlling access based on permissions, as in our example.
- **Caching Proxy:** The proxy can cache the results of expensive operations. The next time the operation is called with the same arguments, the proxy returns the cached result instead of calling the real object.
- **Logging Proxy:** The proxy can log all calls to the real object's methods for debugging or analytics.

## Pros & Cons

- **Pros:**
    - **Control:** You can manage the lifecycle of the real object or control access to it without the object's or the client's knowledge.
    - **Decoupling:** The proxy decouples the control logic from the business logic of the real object.
    - **Powerful (ES6):** The native `Proxy` object in JavaScript is extremely powerful for metaprogramming, allowing you to intercept almost any interaction with an object.

- **Cons:**
    - **Complexity:** It introduces another layer of abstraction, which can make the code harder to understand.
    - **Performance:** The indirection can add a small performance overhead to every call.
    - **Can be Overused:** It's a powerful tool, but simple conditional checks might be sufficient in many cases.

## Common Pitfalls

- Adding proxies when direct access would suffice
- Creating proxies that don't properly forward all methods
- Performance issues with heavy proxy logic
- Confusing proxies with decorators

## Rule of Thumb

Use the Proxy Pattern when you need to control access to an object or add behavior like caching/logging without changing the object's interface.