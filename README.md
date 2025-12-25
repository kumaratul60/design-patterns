# Design Patterns

A curated collection of **JavaScript and React design patterns** with clear explanations and practical implementations.

> Each pattern includes: **Problem → When to Use → Trade-offs → JS / React Example**

---

## Contents

## Pattern Mapping

- **Creational** → object creation, factories, configuration
- **Structural** → composition, API boundaries, reuse
- **Behavioral** → state, events, async flows
- **React Patterns** → component architecture, state ownership, performance

---

## [JavaScript Design Patterns](https://www.patterns.dev/vanilla)

Fundamental **creational, structural, and behavioral patterns** implemented in pure JavaScript.

### Creational Patterns

- **[Singleton Pattern](Design-Patterns/SingletonPtrn/readme.md)** — Single shared instance
- **[Factory Pattern](Design-Patterns/FactoryPtrn/readme.md)** — Delegate object instantiation to a separate factory
- **[Builder Pattern](Design-Patterns/BuilderPtrn/readme.md)** — Construct complex objects step-by-step with a fluent API
- **[Prototype Pattern](Design-Patterns/PrototypePtrn/readme.md)** — Create objects by cloning existing ones

### Structural Patterns

- **[Module Pattern](Design-Patterns/ModulePtrn/readme.md)** — Encapsulate private data/functions and expose a public API
- **[Revealing Module Pattern](Design-Patterns/Revealing-ModulePtrn/readme.md)** — Explicit public/private mapping
- **[Decorator Pattern](Design-Patterns/DecoratorPtrn/readme.md)** — Add behavior dynamically without subclassing
- **[Adapter Pattern](Design-Patterns/AdapterPtrn/readme.md)** — Bridge or Convert incompatible interfaces
- **[Facade Pattern](Design-Patterns/FacadePtrn/readme.md)** — Simplified subsystem interface or Provide a simplified interface to complex subsystems
- **[Proxy Pattern](Design-Patterns/ProxyPtrn/readme.md)** — Controlled access (to objects)
- **[Flyweight Pattern](Design-Patterns/FlyweightPtrn/readme.md)** — Shared state, reduced memory or Share state between similar objects to save memory

### Behavioral Patterns

- **[Strategy Pattern](Design-Patterns/StrategyPtrn/readme.md)** — Swap algorithms at runtime
- **[State Pattern](Design-Patterns/StatePtrn/readme.md)** — Behavior varies by state or Change behavior based on internal state
- **[Observer Pattern](Design-Patterns/ObserverPtrn/readme.md)** — Publish–subscribe updates or Notify multiple objects about state changes
- **[Mediator Pattern](Design-Patterns/MediatorPtrn/readme.md)** — Centralized coordination or Coordinate complex interactions between objects
- **[Command Pattern](Design-Patterns/CommandPtrn/readme.md)** — Encapsulated actions (undo/redo) or encapsulate requests as objects for undo/redo
- **[Iterator Pattern](Design-Patterns/IteratorPtrn/readme.md)** — Safe collection traversal or Traverse collections without exposing internals
- **[Chain of Responsibility Pattern](Design-Patterns/Chain-ResponsibilityPtrn/readme.md)** — Request pipeline or Pass requests along a chain of handlers
- **[Template Method Pattern](Design-Patterns/Template-MethodPtrn/readme.md)** — Define algorithm skeleton with customizable steps

---

## [React Design Patterns](https://www.patterns.dev/react)

React-specific patterns for **scalable, maintainable UI architecture**.

> React patterns focus on **composition and state management**, not classical GoF OO class hierarchies.

---

## GoF (Gang of Four) Context

### What “Object-Oriented” Means

- Class-based design
- Inheritance + polymorphism
- Encapsulation via objects
- Originally designed for C++ / Java-style OOP

### The 3 GoF Categories (Classic)

#### Creational

- Singleton
- Factory / Abstract Factory
- Builder
- Prototype

#### Structural

- Adapter
- Decorator
- Facade
- Proxy
- Flyweight
- Bridge
- Composite

#### Behavioral

- Strategy
- Observer
- State
- Command
- Iterator
- Mediator
- Chain of Responsibility
- Template Method
- Visitor
- Memento
- Interpreter

---

## Important Interview Clarity (JS / React)

- GoF patterns ≠ React patterns
- React favors **functions + composition**, not inheritance
- GoF ideas still apply, but are **expressed functionally**

### Concept Mapping

- **Strategy** → function maps / hook selectors
- **Observer** → events, RxJS, React state updates
- **Decorator** → HOCs / function composition
- **Factory** → config-based object or hook creation

---

## React Pattern Categories

### Structural / Composition

- **[Compound Component Pattern](React-Design-Patterns/Compound-ComponentPtrn/readme.md)** — Allowing multiple related components to work together implicitly via shared parent state
- **[Slot Pattern](React-Design-Patterns/SlotPtrn/readme.md)** — Flexible component composition with predefined slots
- **[Facade Pattern](React-Design-Patterns/FacadePtrn/readme.md)** — Simplified UI APIs or Providing a simplified interface to complex subsystems
- **[Provider Pattern](React-Design-Patterns/ProviderPtrn/readme.md)** — Context-based state sharing or Sharing state across component tree with Context API
- **[Container–Presenter Pattern](React-Design-Patterns/Container–PresenterPtrn/readme.md)** — Logic vs UI separation

### Behavioral / State

- **[Strategy Pattern](React-Design-Patterns/StrategyPtrn/readme.md)** — Selecting algorithms at runtime
- **[State Reducer Pattern](React-Design-Patterns/StateReducerPtrn/readme.md)** — Controlled state transitions or Managing complex state transitions with reducers
- **[Controlled–Uncontrolled Pattern](React-Design-Patterns/Controlled-UncontrolledPtrn/readme.md)** — Flexible state ownership or Managing form state in React components
- **[Publish–Subscribe Pattern](React-Design-Patterns/Pub-SubPtrn/readme.md)** — Decoupled event flow or Decoupling event producers from consumers
- **[Optimistic Hook Pattern](React-Design-Patterns/OptimisticHookPtrn/readme.md)** — Instant UI with async reconciliation or Updating UI immediately while handling async operations

### Reusability

- **[Custom Hook Pattern](React-Design-Patterns/Custom-hook/readme.md)** — Reusable stateful logic
- **[Higher-Order Component (HOC) Pattern](React-Design-Patterns/HOCPtrn/readme.md)** — Wrapper-based reuse
- **[Render Props Pattern](React-Design-Patterns/Render-PropPtrn/readme.md)** — Function-based reuse or Sharing code between components using render functions.

### Performance

- **[Performance Pattern](React-Design-Patterns/PerformancePtrn/readme.md)** — Memoization, rendering, scheduling

---

## Getting Started

- Start with `Design-Patterns/` for core concepts
- Move to `React-Design-Patterns/` for real-world UI architecture

---

## Contributing

Contributions are welcome!
Open a pull request or raise an [issue](https://github.com/kumaratul60/design-patterns/issues/new) here.
