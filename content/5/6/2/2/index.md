---
linkTitle: "6.2.2 Implementing the Mediator Pattern in JavaScript"
title: "Mediator Pattern in JavaScript: Implementation and Best Practices"
description: "Explore the implementation of the Mediator Pattern in JavaScript, focusing on communication management between Colleague objects, practical applications, best practices, and strategies for scalability and performance."
categories:
- Design Patterns
- JavaScript
- Software Architecture
tags:
- Mediator Pattern
- JavaScript Design Patterns
- Software Design
- Behavioral Patterns
- Colleague Objects
date: 2024-10-25
type: docs
nav_weight: 622000
---


## 6.2.2 Implementing the Mediator Pattern in JavaScript

The Mediator Pattern is a behavioral design pattern that facilitates communication between different components or objects, referred to as Colleagues, without them having to refer to each other directly. This pattern promotes loose coupling by keeping objects from referring to each other explicitly, allowing their interactions to be managed by a Mediator object. In this article, we will delve into the implementation of the Mediator Pattern in JavaScript, exploring its practical applications, best practices, and strategies for scalability and performance.

### Understanding the Mediator Pattern

Before diving into implementation, let's understand the core idea of the Mediator Pattern. The pattern introduces a Mediator object that encapsulates how a set of objects (Colleagues) interact. The Colleagues delegate their communication to the Mediator, which ensures that they remain decoupled from each other.

**Key Components:**

- **Mediator:** The central hub that manages communication between Colleague objects.
- **Colleagues:** Objects that interact with each other through the Mediator.

**Benefits:**

- Reduces the dependencies between Colleague objects.
- Simplifies object protocols by centralizing complex communications.
- Enhances flexibility in changing communication between objects by altering the Mediator.

### Implementing the Mediator Pattern in JavaScript

Let's start by creating a simple Mediator class in JavaScript and see how it manages communication between Colleague objects.

#### Step 1: Define the Mediator Class

The Mediator class will have methods to register Colleagues and facilitate communication between them.

```javascript
class Mediator {
    constructor() {
        this.colleagues = {};
    }

    register(colleague) {
        this.colleagues[colleague.name] = colleague;
        colleague.setMediator(this);
    }

    send(message, from, to) {
        if (to in this.colleagues) {
            this.colleagues[to].receive(message, from);
        }
    }
}
```

**Explanation:**

- `register(colleague)`: Registers a Colleague with the Mediator.
- `send(message, from, to)`: Sends a message from one Colleague to another via the Mediator.

#### Step 2: Create Colleague Classes

Colleague objects will interact with each other through the Mediator. Each Colleague will have a reference to the Mediator to send and receive messages.

```javascript
class Colleague {
    constructor(name) {
        this.name = name;
        this.mediator = null;
    }

    setMediator(mediator) {
        this.mediator = mediator;
    }

    send(message, to) {
        console.log(`${this.name} sends message to ${to}: ${message}`);
        this.mediator.send(message, this.name, to);
    }

    receive(message, from) {
        console.log(`${this.name} received message from ${from}: ${message}`);
    }
}
```

**Explanation:**

- `setMediator(mediator)`: Sets the Mediator for the Colleague.
- `send(message, to)`: Sends a message to another Colleague via the Mediator.
- `receive(message, from)`: Receives a message from another Colleague.

#### Step 3: Demonstrate Communication

Let's demonstrate how Colleagues communicate through the Mediator.

```javascript
// Create a mediator
const mediator = new Mediator();

// Create colleagues
const alice = new Colleague('Alice');
const bob = new Colleague('Bob');

// Register colleagues with the mediator
mediator.register(alice);
mediator.register(bob);

// Alice sends a message to Bob
alice.send('Hello, Bob!', 'Bob');

// Bob sends a message to Alice
bob.send('Hi, Alice!', 'Alice');
```

**Output:**

```
Alice sends message to Bob: Hello, Bob!
Bob received message from Alice: Hello, Bob!
Bob sends message to Alice: Hi, Alice!
Alice received message from Bob: Hi, Alice!
```

### Practical Applications

The Mediator Pattern is useful in scenarios where multiple objects need to communicate in a decoupled manner. Here are some practical applications:

- **Chat Rooms:** In a chat application, the Mediator can manage message exchanges between users.
- **UI Component Interactions:** In complex UIs, the Mediator can coordinate actions among components, such as updating a view when a model changes.
- **Event Dispatchers:** The Mediator can act as an event bus, managing event propagation and handling among different parts of an application.

### Best Practices

- **Keep the Mediator's Interface Clear:** Define a clear and concise interface for the Mediator to avoid complexity. The Mediator should expose only the necessary methods for communication.
- **Decouple Colleagues from the Mediator:** Use events or messages to decouple Colleagues from the Mediator. This can be achieved by implementing an event-driven architecture where Colleagues emit events that the Mediator listens to.
- **Avoid Monolithic Mediators:** Ensure that the Mediator does not become a monolithic component by distributing responsibilities and keeping logic simple.
- **Design for Reusability and Configurability:** Design the Mediator to be reusable across different contexts and configurable to accommodate various communication protocols.

### Strategies for Decoupling

Decoupling Colleagues from the Mediator can be achieved through event-driven architectures. Here's an example using JavaScript's EventEmitter:

```javascript
const EventEmitter = require('events');

class Mediator extends EventEmitter {
    register(colleague) {
        colleague.setMediator(this);
    }

    send(message, from, to) {
        this.emit('message', { message, from, to });
    }
}

class Colleague {
    constructor(name) {
        this.name = name;
        this.mediator = null;
    }

    setMediator(mediator) {
        this.mediator = mediator;
        this.mediator.on('message', (data) => {
            if (data.to === this.name) {
                this.receive(data.message, data.from);
            }
        });
    }

    send(message, to) {
        console.log(`${this.name} sends message to ${to}: ${message}`);
        this.mediator.send(message, this.name, to);
    }

    receive(message, from) {
        console.log(`${this.name} received message from ${from}: ${message}`);
    }
}
```

**Explanation:**

- The Mediator extends `EventEmitter` to handle events.
- Colleagues listen to messages from the Mediator and act accordingly.

### Testing Considerations

Testing the Mediator Pattern involves mocking the Mediator or Colleagues to simulate interactions.

- **Mocking the Mediator:** Use a mock Mediator to test Colleague interactions without relying on the actual Mediator implementation.
- **Mocking Colleagues:** Create mock Colleagues to test the Mediator's handling of different communication scenarios.

### Performance and Scalability

- **Avoid Bottlenecks:** Ensure that the Mediator does not become a performance bottleneck by managing large volumes of messages efficiently.
- **Handle Asynchronous Communications:** Implement asynchronous handling of messages using Promises or async/await to prevent blocking operations.

### Handling Asynchronous Communications

To handle asynchronous communications, modify the Mediator to support async operations:

```javascript
class AsyncMediator {
    constructor() {
        this.colleagues = {};
    }

    register(colleague) {
        this.colleagues[colleague.name] = colleague;
        colleague.setMediator(this);
    }

    async send(message, from, to) {
        if (to in this.colleagues) {
            await this.colleagues[to].receive(message, from);
        }
    }
}

class AsyncColleague {
    constructor(name) {
        this.name = name;
        this.mediator = null;
    }

    setMediator(mediator) {
        this.mediator = mediator;
    }

    async send(message, to) {
        console.log(`${this.name} sends message to ${to}: ${message}`);
        await this.mediator.send(message, this.name, to);
    }

    async receive(message, from) {
        console.log(`${this.name} received message from ${from}: ${message}`);
    }
}
```

**Explanation:**

- The `send` and `receive` methods are asynchronous, allowing for non-blocking communication.

### Conclusion

The Mediator Pattern is a powerful tool for managing communication between objects in a decoupled manner. By centralizing interactions within a Mediator, you can simplify object relationships and enhance flexibility. When implementing the Mediator Pattern, consider best practices such as keeping the interface clear, decoupling Colleagues, and designing for scalability. By doing so, you can create robust and maintainable systems that are easy to extend and adapt.

## Quiz Time!

{{< quizdown >}}

### What is the primary role of the Mediator Pattern?

- [x] To manage communication between Colleague objects
- [ ] To encapsulate data within objects
- [ ] To provide a singleton instance
- [ ] To implement a factory for object creation

> **Explanation:** The Mediator Pattern's primary role is to manage communication between Colleague objects, promoting loose coupling.

### Which method is used to register a Colleague with the Mediator?

- [ ] send
- [x] register
- [ ] receive
- [ ] notify

> **Explanation:** The `register` method is used to register a Colleague with the Mediator.

### How can Colleagues be decoupled from the Mediator?

- [x] By using events or messages
- [ ] By hardcoding dependencies
- [ ] By sharing references directly
- [ ] By using global variables

> **Explanation:** Colleagues can be decoupled from the Mediator by using events or messages, promoting an event-driven architecture.

### What is a common application of the Mediator Pattern?

- [ ] Singleton management
- [x] Chat rooms
- [ ] Data encapsulation
- [ ] Object serialization

> **Explanation:** A common application of the Mediator Pattern is managing communication in chat rooms.

### Why should the Mediator's interface be kept clear?

- [x] To avoid complexity and maintain simplicity
- [ ] To increase the number of methods
- [ ] To allow direct access to Colleagues
- [ ] To reduce the number of Colleagues

> **Explanation:** Keeping the Mediator's interface clear avoids complexity and maintains simplicity in communication management.

### How can asynchronous communication be handled in the Mediator Pattern?

- [x] By using Promises or async/await
- [ ] By blocking operations
- [ ] By using synchronous loops
- [ ] By ignoring asynchronous needs

> **Explanation:** Asynchronous communication can be handled using Promises or async/await to ensure non-blocking operations.

### What is a potential risk of the Mediator Pattern if not implemented carefully?

- [x] The Mediator becoming a monolithic component
- [ ] Colleagues becoming too independent
- [ ] The Mediator having too few responsibilities
- [ ] The system becoming too decentralized

> **Explanation:** If not implemented carefully, the Mediator can become a monolithic component, handling too much logic.

### How can the performance of a Mediator be optimized?

- [x] By managing large volumes of messages efficiently
- [ ] By increasing the number of Colleagues
- [ ] By reducing the number of Mediators
- [ ] By avoiding asynchronous operations

> **Explanation:** Performance can be optimized by managing large volumes of messages efficiently within the Mediator.

### What is a strategy for making the Mediator reusable?

- [x] Designing it to be configurable and context-independent
- [ ] Hardcoding specific Colleague interactions
- [ ] Limiting the number of Colleagues
- [ ] Using global variables for communication

> **Explanation:** Designing the Mediator to be configurable and context-independent makes it reusable across different scenarios.

### True or False: The Mediator Pattern reduces the dependencies between Colleague objects.

- [x] True
- [ ] False

> **Explanation:** The Mediator Pattern reduces dependencies between Colleague objects by centralizing their interactions through a Mediator.

{{< /quizdown >}}

