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

- **[Singleton Pattern](Design-Patterns/SingletonPtrn/readme.md)** — Ensure a single shared instance
- **[Factory Pattern](Design-Patterns/FactoryPtrn/readme.md)** — Delegate object creation to a factory
- **[Builder Pattern](Design-Patterns/BuilderPtrn/readme.md)** — Step-by-step construction of complex objects
- **[Prototype Pattern](Design-Patterns/PrototypePtrn/readme.md)** — Create objects by cloning existing ones

### Structural Patterns

- **[Module Pattern](Design-Patterns/ModulePtrn/readme.md)** — Encapsulate private state with a public API
- **[Revealing Module Pattern](Design-Patterns/Revealing-ModulePtrn/readme.md)** — Explicitly define public/private members
- **[Decorator Pattern](Design-Patterns/DecoratorPtrn/readme.md)** — Add behavior dynamically without inheritance
- **[Adapter Pattern](Design-Patterns/AdapterPtrn/readme.md)** — Convert incompatible interfaces
- **[Facade Pattern](Design-Patterns/FacadePtrn/readme.md)** — Simplify access to complex subsystems
- **[Proxy Pattern](Design-Patterns/ProxyPtrn/readme.md)** — Control access to an object
- **[Flyweight Pattern](Design-Patterns/FlyweightPtrn/readme.md)** — Share state to reduce memory usage

### Behavioral Patterns

- **[Strategy Pattern](Design-Patterns/StrategyPtrn/readme.md)** — Select algorithms at runtime
- **[State Pattern](Design-Patterns/StatePtrn/readme.md)** — Change behavior based on internal state
- **[Observer Pattern](Design-Patterns/ObserverPtrn/readme.md)** — Notify subscribers of state changes
- **[Mediator Pattern](Design-Patterns/MediatorPtrn/readme.md)** — Centralize object communication
- **[Command Pattern](Design-Patterns/CommandPtrn/readme.md)** — Encapsulate actions for undo/redo
- **[Iterator Pattern](Design-Patterns/IteratorPtrn/readme.md)** — Traverse collections without exposing internals
- **[Chain of Responsibility Pattern](Design-Patterns/Chain-ResponsibilityPtrn/readme.md)** — Pass requests through a handler chain
- **[Template Method Pattern](Design-Patterns/Template-MethodPtrn/readme.md)** — Define an algorithm skeleton with overridable steps

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

- **[Compound Component Pattern](React-Design-Patterns/Compound-ComponentPtrn/readme.md)** — Implicit coordination via shared parent state
- **[Slot Pattern](React-Design-Patterns/SlotPtrn/readme.md)** — Flexible component composition
- **[Facade Pattern](React-Design-Patterns/FacadePtrn/readme.md)** — Simplified UI APIs
- **[Provider Pattern](React-Design-Patterns/ProviderPtrn/readme.md)** — Context-based state sharing
- **[Container–Presenter Pattern](React-Design-Patterns/Container–PresenterPtrn/readme.md)** — Separate logic from presentation

### Behavioral / State

- **[Strategy Pattern](React-Design-Patterns/StrategyPtrn/readme.md)** — Runtime logic selection
- **[State Reducer Pattern](React-Design-Patterns/StateReducerPtrn/readme.md)** — Controlled state transitions
- **[Controlled–Uncontrolled Pattern](React-Design-Patterns/Controlled-UncontrolledPtrn/readme.md)** — Flexible state ownership
- **[Publish–Subscribe Pattern](React-Design-Patterns/Pub-SubPtrn/readme.md)** — Decoupled event flow
- **[Optimistic Hook Pattern](React-Design-Patterns/OptimisticHookPtrn/readme.md)** — Instant UI with async reconciliation

### Reusability

- **[Custom Hook Pattern](React-Design-Patterns/Custom-hook/readme.md)** — Reusable stateful logic
- **[Higher-Order Component (HOC) Pattern](React-Design-Patterns/HOCPtrn/readme.md)** — Wrapper-based reuse
- **[Render Props Pattern](React-Design-Patterns/Render-PropPtrn/readme.md)** — Function-based reuse

### Performance

- **[Performance Pattern](React-Design-Patterns/PerformancePtrn/readme.md)** — Rendering, memoization, scheduling

---

## WEB-Design
- **[How Web works](Web-Design/web-works.md)**

## Getting Started

- Start with `Design-Patterns/` for core concepts
- Move to `React-Design-Patterns/` for real-world UI architecture

---

## Contributing

Contributions are welcome!
Open a pull request or raise an [issue](https://github.com/kumaratul60/design-patterns/issues/new) here.
