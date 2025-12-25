# Singleton Pattern

## Overview

The Singleton pattern is a **creational pattern** that ensures a class has **only one instance**, while providing a **global access point** to this instance.

## Core Idea

- Ensure only one instance of a class exists
- Provide a global point of access to that instance
- Control instantiation to prevent multiple objects

## When to Use

- App-wide config, logger, cache, DB connection, feature flag provider
- You truly need single ownership of a resource
- When exactly one instance is required and must be accessible globally

## When NOT to Use

- When you need multiple instances of a class
- In testing scenarios where you need to mock or isolate instances
- When dependency injection can provide better decoupling

## Core Rules & Responsibilities

- Private constructor to prevent direct instantiation
- Static method to provide access to the single instance
- Lazy initialization (create instance only when needed)
- Thread-safe implementation in multi-threaded environments

## Minimal Example

```javascript
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    this.value = Math.random();
    Singleton.instance = this;
  }

  static getInstance() {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }
}
```

## Real-World Problem

Imagine you are building a web application with multiple components that need to access shared configuration data. Without a singleton, each component might create its own configuration object, leading to inconsistencies.

## Solution

The Singleton pattern guarantees that there's only one configuration object for the entire application, ensuring consistent settings across all components.

## Classic Implementation

```javascript
class AppSettings {
    // 1. A private static property to hold the single instance.
    static instance;

    // 2. The constructor is where the magic happens.
    constructor() {
        // If an instance already exists, return that instance instead of creating a new one.
        if (AppSettings.instance) {
            return AppSettings.instance;
        }

        // If no instance exists, create one and store it.
        this.settings = {
            theme: 'light',
            apiUrl: 'https://api.myapp.com',
            language: 'en'
        };
        AppSettings.instance = this;
    }

    // Public method to get a setting
    get(key) {
        return this.settings[key];
    }

    // Public method to set a setting
    set(key, value) {
        this.settings[key] = value;
    }
}

// To ensure the object isn't accidentally created with `new`, we freeze it.
const settingsInstance = new AppSettings();
Object.freeze(settingsInstance);

// --- USAGE ---

// Get the settings instance from anywhere in your app.
// Note: We don't use `new` again. We just use the instance we created.
const settings1 = settingsInstance;
const settings2 = settingsInstance; // This will point to the exact same object.

// Verify that they are the same instance
console.log(settings1 === settings2); // true

// Now, let's change a setting from one part of the app
console.log('Initial theme:', settings1.get('theme')); // 'light'
settings1.set('theme', 'dark');
console.log('Theme after change:', settings1.get('theme')); // 'dark'

// Another part of the app checks the theme and sees the updated value
console.log('Theme seen by another module:', settings2.get('theme')); // 'dark'
```

## Modern Implementation

```javascript
// Safe singleton across duplicate bundles
const AnalyticsSingleton = (function () {
  const GLOBAL_KEY = '__app_analytics__';
  if (!globalThis[GLOBAL_KEY]) {
    globalThis[GLOBAL_KEY] = {
      queue: [],
      track(event, payload) {
        this.queue.push({ event, payload, ts: Date.now() });
      }
    };
  }
  return globalThis[GLOBAL_KEY];
})();

AnalyticsSingleton.track('page_view', { path: location.pathname });
```

## When to Use the Singleton Pattern

- **Configuration Management:** A global configuration object for an application.
- **Logging Service:** A single logger instance that all parts of the application use to write logs to a file or console.
- **Database Connection Pool:** Managing a single pool of connections to a database to avoid the overhead of creating new connections.
- **Hardware Interface Access:** If you are controlling a single piece of hardware (like a printer or a specific port), a singleton can prevent conflicts.

## Pros & Cons

- **Pros:**
    - Guarantees a single instance, controlling access to a shared resource.
    - Provides a convenient global point of access.
    - The singleton is only created when it's first needed (lazy initialization).

- **Cons:**
    - Can be considered an anti-pattern by some because it introduces a **global state** into an application, which can make testing difficult.
    - Can hide dependencies. Instead of passing a dependency (like a config object) into a module, the module might just grab the global singleton, making the code less explicit.
    - Violates the Single Responsibility Principle (the class is responsible for its own logic *and* for managing its own instantiation).
    - Hard to test and can complicate unit testing.

## Common Pitfalls

- Using singletons for everything, leading to tight coupling
- Forgetting to handle multi-threading in server environments
- Making singletons mutable, leading to unexpected side effects
- Using singletons when dependency injection would be better

## Rule of Thumb

Only use Singleton when you absolutely need exactly one instance of a class. Prefer dependency injection for better testability and decoupling.