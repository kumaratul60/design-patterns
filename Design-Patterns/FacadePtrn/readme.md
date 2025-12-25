# Facade Pattern

## Overview

The Facade Pattern is a **structural pattern** that provides a **simplified, higher-level interface** to a larger body of code, such as a complex subsystem or a set of libraries.

## Core Idea

- Provide a simple interface to complex subsystems
- Hide complexity behind a unified API
- Simplify client interaction with complex systems

## When to Use

- Hide messy multi-step workflows behind a clean API
- When you have a complex subsystem with many moving parts
- To provide a stable interface to changing subsystems

## When NOT to Use

- When the subsystem is already simple
- If clients need access to advanced features
- For thin wrappers that don't add value

## Core Rules & Responsibilities

- Create a facade class that exposes a simplified interface
- The facade coordinates calls to subsystem components
- Hide internal complexity and interactions

## Minimal Example

```javascript
class SubsystemA { method() {} }
class SubsystemB { method() {} }

class Facade {
  constructor() {
    this.a = new SubsystemA();
    this.b = new SubsystemB();
  }
  simpleOperation() {
    this.a.method();
    this.b.method();
  }
}
```

## Real-World Problem

Imagine uploading a file to cloud storage involves multiple steps: getting upload URL, uploading, registering metadata, sending notifications. Without a facade, clients would need to know all these details.

## Solution

The Facade Pattern provides a single `uploadAsset()` method that handles all the complexity internally.

## Code Snippet

```javascript
// --- The Complex Subsystems ---
class AuthService {
  isLoggedIn(userToken) {
    return userToken === 'valid-token';
  }
}

class MovieDB {
  findMovie(movieId) {
    console.log(`Finding movie with ID: ${movieId}`);
    return { name: 'The Matrix', price: 12 };
  }
}

class SeatingSystem {
  reserve(movieId, seatNumber) {
    console.log(`Reserving seat ${seatNumber} for movie ${movieId}`);
    return true;
  }
  unreserve(movieId, seatNumber) {
    console.log(`Seat ${seatNumber} for ${movieId} has been released.`);
  }
}

class PaymentGateway {
  charge(cardInfo, amount) {
    console.log(`Charging $${amount} to card ${cardInfo.number}`);
    return true;
  }
}

class NotificationService {
  sendEmail(email, message) {
    console.log(`Sending email to ${email}: ${message}`);
  }
}

// --- The Facade ---
class TicketPurchaseFacade {
  constructor() {
    this.auth = new AuthService();
    this.movies = new MovieDB();
    this.seating = new SeatingSystem();
    this.payment = new PaymentGateway();
    this.notify = new NotificationService();
  }

  purchaseTicket(user, movieId, seatNumber) {
    console.log('--- Starting ticket purchase process ---');

    if (!this.auth.isLoggedIn(user.token)) {
      return { success: false, error: 'User not authenticated.' };
    }

    const movie = this.movies.findMovie(movieId);
    if (!movie) {
      return { success: false, error: 'Movie not found.' };
    }

    if (!this.seating.reserve(movieId, seatNumber)) {
      return { success: false, error: 'Seat could not be reserved.' };
    }

    if (!this.payment.charge(user.creditCard, movie.price)) {
      this.seating.unreserve(movieId, seatNumber);
      return { success: false, error: 'Payment failed.' };
    }

    this.notify.sendEmail(user.email, `Your ticket for "${movie.name}" is confirmed!`);

    console.log('--- Ticket purchase process finished successfully! ---');
    return { success: true };
  }
}

// --- USAGE ---
const facade = new TicketPurchaseFacade();

const user = {
  token: 'valid-token',
  email: 'john.doe@example.com',
  creditCard: { number: '1234-5678-9012-3456' }
};

// The client code is now incredibly simple!
const result = facade.purchaseTicket(user, 'movie-123', 'F7');

console.log('Purchase result:', result);
```

## Modern Implementation

```javascript
function uploadAsset(file) {
  return fetch('/api/uploadUrl')
    .then((r) => r.json())
    .then(({ url }) => fetch(url, { method: 'PUT', body: file }))
    .then(() => fetch('/api/assets', { method: 'POST', body: JSON.stringify({ name: file.name }) }))
    .then((r) => r.json());
}

// Caller sees just one method
```

## When to Use the Facade Pattern

- When you have a complex subsystem and you want to provide a simple, limited interface to it for clients.
- When you want to **decouple** your application's client code from the inner workings of its subsystems. This makes the subsystem easier to update or replace later.
- When you want to create a layered architecture. Facades can be the entry points to each layer. For example, a `DataAccessFacade` could hide the complexities of your database interactions (whether you're using SQL, NoSQL, or local storage).

## Pros & Cons

- **Pros:**
    - **Simplicity:** Hides complexity from the client, making the code easier to use and understand.
    - **Decoupling:** Isolates the client from changes in the subsystem. If you swap your payment gateway from Stripe to PayPal, you only need to update the Facade, not every piece of client code that uses it.
    - **Centralized Logic:** Provides a single "choke point" for a complex task, which is great for logging, error handling, and security.

- **Cons:**
    - **Can Become a "God Object":** The facade can become coupled to too many subsystems and take on too many responsibilities, becoming an anti-pattern itself.
    - **Hides Features:** A simplified interface might hide useful features of the underlying subsystems that some advanced clients might need. (Good facade design often allows "power users" to bypass the facade and access the subsystem directly if needed).

## Common Pitfalls

- Making facades too complex by including too much logic
- Facades becoming tightly coupled to subsystems
- Hiding important error details from clients
- Creating facades that don't add real value

## Rule of Thumb

Use the Facade Pattern when you want to provide a simple interface to a complex subsystem. Ensure the facade adds real value and doesn't just wrap everything without simplification.