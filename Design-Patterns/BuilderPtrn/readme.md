# Builder Pattern

## Overview

The Builder Pattern is a **creational pattern** designed to **separate the construction of a complex object from its representation**. It allows you to create complex objects step-by-step. The final object is only "built" and returned after all the necessary construction steps have been completed.

## Core Idea

- Construct complex objects step by step via a fluent API
- Separate the construction logic from the final object
- Allow optional parameters without telescoping constructors

## When to Use

- Many optional fields / defaulting / validation
- You need readable, staged construction
- When object construction involves multiple steps

## When NOT to Use

- For simple objects with few parameters
- When all parameters are required and known upfront
- If the construction process is trivial

## Core Rules & Responsibilities

- Provide a builder class with methods for each construction step
- Return the builder instance from each method for chaining
- Include a final `build()` method that validates and returns the object
- Handle defaults and validation in the builder

## Minimal Example

```javascript
function UserBuilder(name, email) {
  const user = { name, email };
  return {
    setAge(age) {
      user.age = age;
      return this;
    },
    setBio(bio) {
      user.bio = bio;
      return this;
    },
    build() {
      return { ...user };
    }
  };
}
```

## Real-World Problem

Imagine building a user profile object with many optional fields. Using telescoping constructors leads to confusing method signatures and hard-to-read code.

## Solution

The Builder Pattern provides a fluent, step-by-step interface for object construction, making it readable and less error-prone.

## Code Snippet

```javascript
// The "Product" - The complex object we want to build.
// Its constructor is simple and just accepts the final configuration.
class User {
    constructor({ name, email, bio = '', profilePic = 'default.jpg', location = 'Unknown', receivesNewsletter = false }) {
        this.name = name;
        this.email = email;
        this.bio = bio;
        this.profilePic = profilePic;
        this.location = location;
        this.receivesNewsletter = receivesNewsletter;
    }

    toString() {
        return JSON.stringify(this, null, 2);
    }
}

// The "Builder" - Provides the step-by-step construction interface.
class UserBuilder {
    constructor(name, email) {
        // Required parameters are set in the constructor.
        if (!name || !email) {
            throw new Error("Name and email are required.");
        }
        this.user = { name, email };
    }

    // Each of the following methods represents a step.
    // They return `this` to allow for method chaining.
    withBio(bio) {
        this.user.bio = bio;
        return this;
    }

    withProfilePic(url) {
        this.user.profilePic = url;
        return this;
    }

    withLocation(location) {
        this.user.location = location;
        return this;
    }

    wantsNewsletter(wants) {
        this.user.receivesNewsletter = wants;
        return this;
    }

    // The final step that returns the fully constructed object.
    build() {
        return new User(this.user);
    }
}

// --- USAGE ---

// The client code is now extremely readable and less error-prone.

console.log('--- Building a simple user ---');
const simpleUser = new UserBuilder('John Doe', 'john@test.com').build();
console.log(simpleUser.toString());


console.log('\n--- Building a complex user ---');
const complexUser = new UserBuilder('Jane Smith', 'jane@test.com')
    .withBio('Software developer and dog lover.')
    .withLocation('San Francisco')
    .wantsNewsletter(true)
    .build();

console.log(complexUser.toString());
```

## Modern Implementation

```javascript
function ReportBuilder() {
  const req = { format: 'csv', filters: {}, columns: [] };
  return {
    format(fmt) {
      req.format = fmt;
      return this;
    },
    filter(key, val) {
      req.filters[key] = val;
      return this;
    },
    column(name) {
      req.columns.push(name);
      return this;
    },
    build() {
      if (req.columns.length === 0) throw new Error('At least one column');
      return { ...req };
    },
  };
}

const request = ReportBuilder().format('xlsx').filter('from', '2025-01-01').column('userId').column('revenue').build();
```

## When to Use the Builder Pattern

- When the creation of an object involves many optional parameters or requires a multi-step process.
- When you want to create immutable objects. The builder can gather all the data, and the final object's constructor can set all properties, with no setters exposed afterward.
- When you need to create different representations of the same object. For example, the same build process could be used by a `JSONBuilder` or an `XMLBuilder`.

## Pros & Cons

- **Pros:**
    - **Improved Readability:** The step-by-step, fluent interface makes object creation self-documenting. `new UserBuilder().withBio(...)` is much clearer than `new User(..., 'some bio', ...)`
    - **Encapsulates Complexity:** The logic and validation for constructing the object are hidden away in the builder, not in a massive constructor.
    - **Flexibility:** It's easy to add new optional parameters by simply adding a new method to the builder, without breaking existing code.
    - **Finer Control:** You can add validation logic inside each builder step.

- **Cons:**
    - **Increased Verbosity:** Requires creating a separate `Builder` class for each complex object, which can feel like boilerplate code for simpler cases.
    - **Coupling:** The builder is tightly coupled to the product it creates.

## Common Pitfalls

- Forgetting to return `this` from builder methods
- Not validating required fields in the `build()` method
- Making builders mutable after build
- Using builders for very simple objects

## Rule of Thumb

Use the Builder Pattern when object construction is complex with many optional parameters. For simple objects, prefer direct instantiation or factory functions.