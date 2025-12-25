# Factory Pattern

## Overview

The Factory Pattern is a **creational pattern** that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created. More simply, it's a way to **delegate the responsibility of object instantiation to a separate "factory" function or class**.

## Core Idea

- Separate object creation logic from the client code
- Provide a common interface for creating different types of objects
- Delegate instantiation decisions to a factory method

## When to Use

- You create many variants of a type based on input
- Instantiation logic changes often (e.g., from config/env)
- When the creation process is complex and you want to encapsulate it

## When NOT to Use

- For simple object creation with `new`
- When the types are known at compile time and don't change
- If you need inheritance hierarchies that the factory would complicate

## Core Rules & Responsibilities

- Define a factory function or class that handles creation logic
- Return objects that conform to a common interface
- Hide complex instantiation details from clients

## Minimal Example

```javascript
function createVehicle(type) {
  if (type === 'car') {
    return { type: 'car', wheels: 4, drive: () => 'Vroom!' };
  } else if (type === 'bike') {
    return { type: 'bike', wheels: 2, drive: () => 'Zoom!' };
  }
}
```

## Real-World Problem

Imagine you're building a content management system that supports different types of content creators (Developer, Tester, Designer). Each type has different hourly rates and responsibilities. Without a factory, client code becomes cluttered with conditional logic.

## Solution

The Factory Pattern abstracts creation logic into a dedicated factory, making client code clean and decoupled.

## Code Snippet

```javascript
// --- The "Products" (different types of employees) ---

class Developer {
    constructor(name, hourlyRate = 100) {
        this.name = name;
        this.hourlyRate = hourlyRate;
        this.type = 'Developer';
    }

    sayHello() {
        console.log(`Hi, I am ${this.name}, a ${this.type}. My rate is $${this.hourlyRate}/hour.`);
    }
}

class Tester {
    constructor(name, hourlyRate = 80) {
        this.name = name;
        this.hourlyRate = hourlyRate;
        this.type = 'Tester';
    }

    sayHello() {
        console.log(`Hi, I am ${this.name}, a ${this.type}. My rate is $${this.hourlyRate}/hour.`);
    }
}

// --- The "Factory" ---

class EmployeeFactory {
    create(name, type) {
        switch (type.toLowerCase()) {
            case 'developer':
                return new Developer(name);
            case 'tester':
                return new Tester(name);
            default:
                throw new Error(`Invalid employee type: ${type}`);
        }
    }
}

// --- USAGE ---

const factory = new EmployeeFactory();
const employees = [];

// The client code is now clean and decoupled!
employees.push(factory.create('John Doe', 'developer'));
employees.push(factory.create('Jane Smith', 'tester'));

// If we wanted to add a 'Designer', we would only modify the factory,
// not this client code.
// employees.push(factory.create('Peter Pan', 'designer')); // Would throw error until we update the factory

employees.forEach(emp => emp.sayHello());

// Output:
// Hi, I am John Doe, a Developer. My rate is $100/hour.
// Hi, I am Jane Smith, a Tester. My rate is $80/hour.
```

## Modern Implementation

```javascript
function createHttpClient({ env = 'prod', token } = {}) {
  const base = env === 'dev' ? 'https://api.dev.local' : 'https://api.example.com';
  return {
    get(path) {
      return fetch(base + path, { headers: { Authorization: `Bearer ${token}` } });
    },
    post(path, body) {
      return fetch(base + path, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json', Authorization: `Bearer ${token}` },
        body: JSON.stringify(body),
      });
    },
  };
}

const client = createHttpClient({ env: 'dev', token: 't123' });
```

## When to Use the Factory Pattern

- **Complex Object Creation:** When creating an object involves more than just a simple `new` call (e.g., setting up dependencies, fetching data, configuring properties).
- **Decoupling:** When you want the client code to work with a general interface/class without knowing the specific implementation details (e.g., creating a `DatabaseConnection` object that could be `MySQLConnection`, `PostgreSQLConnection`, or `MongoConnection` based on a configuration setting).
- **Frameworks and Libraries:** When you are building a library and want to allow users to add their own custom types without changing the library's core code. For example, a charting library might use a factory to create different chart types (`BarChart`, `PieChart`, etc.).

## Pros & Cons

- **Pros:**
    - **Decouples your code:** The client code isn't tied to concrete classes.
    - **Centralizes creation logic:** Makes the code easier to maintain, as all creation logic is in one place.
    - **Follows SOLID principles:** Adheres to the Single Responsibility and Open/Closed principles. You can introduce new product types without breaking existing client code.

- **Cons:**
    - **Increased Complexity:** You introduce a new class (the factory), which adds more complexity to the overall code structure, especially for simple cases.
    - **Can become a large `switch` statement:** If you have many product types, the factory method can become a large and unwieldy `switch` statement (though this can be mitigated with more advanced patterns).

## Common Pitfalls

- Making factories too complex with too many parameters
- Forgetting to handle invalid input types
- Creating factories when simple constructors would suffice
- Hardcoding type checks instead of using polymorphism

## Rule of Thumb

Use the Factory Pattern when object creation logic is complex or when you need to decouple client code from specific classes. For simple cases, prefer direct instantiation.