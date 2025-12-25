# Command Pattern

## Overview

The Command Pattern is a **behavioral pattern** that turns a request into a **stand-alone object** containing all information about the request. This transformation lets you parameterize methods with different requests, delay or queue a request's execution, and support undoable operations.

## Core Idea

- Encapsulate requests as objects with execute/undo methods
- Decouple invoker from receiver
- Support queuing, logging, and undo operations

## When to Use

- Editors (text/canvas), macro systems, retry queues
- When you need undo/redo functionality
- To queue or log operations

## When NOT to Use

- For simple synchronous operations
- When undo is not needed
- If the command objects would be very lightweight

## Core Rules & Responsibilities

- Define a command interface with execute and undo methods
- Create concrete command classes for each operation
- Use an invoker to execute commands and manage history

## Minimal Example

```javascript
class Command {
  constructor(receiver, ...args) {
    this.receiver = receiver;
    this.args = args;
  }
  execute() {
    this.receiver.doSomething(...this.args);
  }
  undo() {
    this.receiver.undoSomething(...this.args);
  }
}
```

## Real-World Problem

Imagine building a text editor with undo/redo. Without the command pattern, implementing undo would require complex state tracking and reversal logic.

## Solution

The Command Pattern encapsulates each operation as an object that knows how to execute itself and undo itself, making undo/redo straightforward.

## Code Snippet

```javascript
// 1. The "Receiver" - The object that will perform the actual work.
class Calculator {
    constructor() {
        this.value = 0;
        console.log(`Calculator starting at: ${this.value}`);
    }

    add(amount) {
        this.value += amount;
        console.log(`Added ${amount}. Current value: ${this.value}`);
    }

    subtract(amount) {
        this.value -= amount;
        console.log(`Subtracted ${amount}. Current value: ${this.value}`);
    }
}

// 2. The "Command" Interface (conceptually) and Concrete Commands.
class AddCommand {
    constructor(calculator, amount) {
        this.calculator = calculator;
        this.amount = amount;
    }

    execute() {
        this.calculator.add(this.amount);
    }

    undo() {
        this.calculator.subtract(this.amount);
    }
}

class SubtractCommand {
    constructor(calculator, amount) {
        this.amount = amount;
        this.calculator = calculator;
    }

    execute() {
        this.calculator.subtract(this.amount);
    }

    undo() {
        this.calculator.add(this.amount);
    }
}

// 3. The "Invoker" - Knows how to execute a command and can keep track of history.
class CommandProcessor {
    constructor() {
        this.history = [];
    }

    execute(command) {
        console.log(`--- Executing Command: ${command.constructor.name} ---`);
        command.execute();
        this.history.push(command);
    }

    undo() {
        console.log('--- Undoing last command ---');
        const command = this.history.pop();
        if (command) {
            command.undo();
        } else {
            console.log("No commands to undo.");
        }
    }
}


// --- USAGE ---

// Client code
const calculator = new Calculator();
const invoker = new CommandProcessor();

// Create and execute commands
invoker.execute(new AddCommand(calculator, 10));
invoker.execute(new AddCommand(calculator, 5));
invoker.execute(new SubtractCommand(calculator, 3));

// Now, let's undo the last two operations
invoker.undo();
invoker.undo();
```

## Modern Implementation

```javascript
function createEditor() {
  let text = '';
  const history = [];
  const undone = [];

  const exec = (command) => {
    history.push(command);
    command.do();
    undone.length = 0;
  };
  const undo = () => {
    const c = history.pop();
    if (c) {
      c.undo();
      undone.push(c);
    }
  };
  const redo = () => {
    const c = undone.pop();
    if (c) {
      c.do();
      history.push(c);
    }
  };

  return {
    insert(pos, str) {
      exec({
        do: () => (text = text.slice(0, pos) + str + text.slice(pos)),
        undo: () => (text = text.slice(0, pos) + text.slice(pos + str.length)),
      });
    },
    remove(pos, len) {
      const old = text.slice(pos, pos + len);
      exec({
        do: () => (text = text.slice(0, pos) + text.slice(pos + len)),
        undo: () => (text = text.slice(0, pos) + old + text.slice(pos)),
      });
    },
    getText: () => text,
    undo,
    redo,
  };
}
```

## When to Use the Command Pattern

- **To implement undo/redo functionality.** This is the classic use case.
- **When you want to queue operations.** A command object has all the information needed to be executed later, so you can put it in a queue for background processing.
- **When you want to parameterize objects with an action.** For example, UI buttons or menu items can be configured with a command object to execute, rather than having their logic hard-coded.
- **To create a history of requests.** You can log commands as they are executed, which can be useful for debugging or creating macros.

## Pros & Cons

- **Pros:**
    - **Decouples Objects:** Decouples the object that invokes the operation from the one that knows how to perform it.
    - **Follows SOLID Principles:** Adheres to the Single Responsibility Principle (commands are focused on one thing) and the Open/Closed Principle (you can add new commands without changing existing code).
    - **Undo/Redo is Easy:** Implementing undo/redo becomes straightforward.
    *   **Composition:** You can assemble a sequence of commands into a single "macro" command.

- **Cons:**
    - **Increased Complexity:** Introduces many new classes/objects, which can feel like overkill for simple applications. Every single action requires a new command class, adding boilerplate.
    - **Memory Usage:** Command objects can accumulate in memory, especially for long-running applications with extensive undo histories.

## Common Pitfalls

- Creating too many command classes for simple operations
- Not implementing undo properly
- Memory leaks from accumulating command history
- Making commands stateful when they should be stateless

## Rule of Thumb

Use the Command Pattern when you need undo/redo functionality or when you want to decouple the execution of operations from their invocation.