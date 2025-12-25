# Strategy Pattern

## Overview

The Strategy Pattern is a **behavioral pattern** that enables selecting an algorithm at runtime. Instead of implementing a single algorithm directly, code receives run-time instructions as to which in a family of algorithms to use. The pattern **encapsulates each algorithm into a separate class** and makes their objects interchangeable.

## Core Idea

- Define a family of algorithms, encapsulate each one
- Make algorithms interchangeable at runtime
- Allow the client to choose the appropriate strategy

## When to Use

- You switch algorithms based on context (e.g., sorting, pricing rules, discount logic)
- When multiple variants of an algorithm exist
- To avoid large conditional blocks

## When NOT to Use

- When there's only one algorithm or it never changes
- For simple conditional logic
- If performance is critical and indirection would be costly

## Core Rules & Responsibilities

- Define a strategy interface that all concrete strategies implement
- Create concrete strategy classes for each algorithm
- Allow the context to switch strategies dynamically

## Minimal Example

```javascript
const strategies = {
  quick: (data) => data.sort((a, b) => a - b),
  bubble: (data) => {
    // bubble sort implementation
  }
};

function sortData(data, strategy = 'quick') {
  return strategies[strategy](data);
}
```

## Real-World Problem

Imagine building an e-commerce system with different discount strategies (percentage discount, fixed amount, buy-one-get-one). Without the strategy pattern, you'd have complex conditional logic scattered throughout the code.

## Solution

The Strategy Pattern encapsulates each discount algorithm in its own class, making them interchangeable and the code more maintainable.

## Code Snippet

```javascript
// --- The "Context" ---
// It maintains a reference to a Strategy object and delegates work to it.
class Shipping {
    constructor() {
        this.companyStrategy = null;
    }

    setStrategy(companyStrategy) {
        this.companyStrategy = companyStrategy;
    }

    calculate(package) {
        if (!this.companyStrategy) {
            throw new Error("Shipping strategy has not been set.");
        }
        // Delegate the calculation to the chosen strategy.
        return this.companyStrategy.calculate(package);
    }
}

// --- The "Strategies" ---
// Each strategy is a class with the same interface (a 'calculate' method).

class UPSShipping {
    calculate(package) {
        // In a real app, this would be a more complex calculation.
        console.log('Calculating shipping cost with UPS...');
        return package.weight * 0.45;
    }
}

class FedExShipping {
    calculate(package) {
        console.log('Calculating shipping cost with FedEx...');
        return package.weight * 0.55;
    }
}

class StandardShipping {
    calculate(package) {
        console.log('Calculating shipping cost with Standard Mail...');
        return package.weight * 0.25 + 5;
    }
}

// --- USAGE ---

// Create the context
const shipping = new Shipping();

// Create a sample package
const myPackage = { from: 'New York', to: 'Los Angeles', weight: 10 };

// 1. Calculate using UPS
shipping.setStrategy(new UPSShipping());
const upsCost = shipping.calculate(myPackage);
console.log(`UPS Cost: $${upsCost}`);

// 2. Switch the strategy to FedEx at runtime
shipping.setStrategy(new FedExShipping());
const fedexCost = shipping.calculate(myPackage);
console.log(`FedEx Cost: $${fedexCost}`);

// 3. Switch again to Standard Mail
shipping.setStrategy(new StandardShipping());
const standardCost = shipping.calculate(myPackage);
console.log(`Standard Mail Cost: $${standardCost}`);


// Output:
// Calculating shipping cost with UPS...
// UPS Cost: $4.5
// Calculating shipping cost with FedEx...
// FedEx Cost: $5.5
// Calculating shipping cost with Standard Mail...
// Standard Mail Cost: $7.5
```

## Modern Implementation

```javascript
const strategies = {
  none: (amount) => amount,
  silver: (amount) => amount * 0.95,
  gold: (amount) => amount * 0.9,
};

function applyDiscount(amount, tier = 'none') {
  const fn = strategies[tier] || strategies.none;
  return fn(amount);
}

applyDiscount(100, 'gold'); // 90
```

## When to Use the Strategy Pattern

- When you have multiple variants of an algorithm for a task, and you want to let the client choose which one to use.
- When you want to avoid exposing complex, algorithm-specific data structures to the client.
- When a class defines many behaviors, and these appear as multiple conditional statements in its operations. Strategy lets you move each branch of the conditional into its own class.
- Common examples: Payment gateways (`StripeStrategy`, `PayPalStrategy`), Sorting algorithms (`QuickSortStrategy`, `BubbleSortStrategy`), Validation rules (`EmailValidationStrategy`, `PasswordStrengthStrategy`).

## Pros & Cons

- **Pros:**
    - **Interchangeable Algorithms:** You can swap algorithms at runtime.
    - **Follows SOLID Principles:** Adheres to the Open/Closed Principle (you can introduce new strategies without changing the context) and Single Responsibility Principle (each strategy class has one job).
    - **Cleaner Code:** Eliminates large conditional blocks, making the context class simpler and easier to read.
    - **Isolation & Testing:** The algorithms are isolated in their own classes, making them easier to test independently.

- **Cons:**
    - **Increased Complexity:** Introduces more objects into the application, which can be overkill for a small number of simple algorithms.
    - **Client Awareness:** The client must understand the differences between strategies to be able to select the appropriate one.

## Common Pitfalls

- Creating too many small strategy classes for simple cases
- Forgetting to implement the strategy interface consistently
- Making strategies stateful when they should be stateless
- Overusing the pattern for very simple conditional logic

## Rule of Thumb

Use the Strategy Pattern when you have multiple algorithms that need to be selected at runtime. For simple conditionals, prefer basic if/else statements.