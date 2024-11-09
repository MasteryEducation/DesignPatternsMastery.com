---

linkTitle: "6.2.3 Mediator Pattern in TypeScript"
title: "Mediator Pattern in TypeScript: Streamlining Communication with Strong Typing"
description: "Explore the Mediator Pattern in TypeScript, leveraging interfaces and strong typing to manage complex interactions between objects. Learn how to implement, test, and integrate this pattern in modern frameworks."
categories:
- Software Design
- TypeScript
- Design Patterns
tags:
- Mediator Pattern
- TypeScript
- Design Patterns
- Object-Oriented Programming
- Software Architecture
date: 2024-10-25
type: docs
nav_weight: 623000
---

## 6.2.3 Mediator Pattern in TypeScript

In software development, managing communication between multiple objects can become complex and unwieldy. The Mediator Pattern offers a solution by centralizing communication, reducing dependencies, and promoting loose coupling. In this section, we will explore how to implement the Mediator Pattern in TypeScript, leveraging its strong typing and interface capabilities to enforce correct usage and facilitate maintenance.

### Understanding the Mediator Pattern

The Mediator Pattern is a behavioral design pattern that encapsulates how a set of objects interact. Instead of objects communicating directly with each other, they communicate through a mediator. This pattern promotes loose coupling by keeping objects from referring to each other explicitly and allows you to vary their interaction independently.

#### Key Components

- **Mediator**: The central hub through which all communication passes. It defines an interface for communication between Colleague objects.
- **Colleague**: Objects that communicate with each other through the Mediator. They hold a reference to the Mediator and use it to send and receive messages.

By using the Mediator Pattern, you can simplify object interactions, making your code more maintainable and scalable.

### Implementing the Mediator Pattern in TypeScript

TypeScript provides powerful features such as interfaces and strong typing, which make implementing the Mediator Pattern both efficient and robust. Let's explore how to define and implement the Mediator and Colleague components using TypeScript.

#### Defining the Mediator Interface

First, we define an interface for the Mediator. This interface will outline the methods that the Mediator must implement to facilitate communication between Colleagues.

```typescript
interface Mediator {
    notify(sender: Colleague, event: string): void;
}
```

#### Defining the Colleague Interface

Next, we define a Colleague interface. Colleagues will implement this interface and interact with the Mediator.

```typescript
interface Colleague {
    setMediator(mediator: Mediator): void;
}
```

#### Implementing Concrete Colleagues

Concrete Colleagues are specific implementations of the Colleague interface. They interact with each other through the Mediator.

```typescript
class ConcreteColleague1 implements Colleague {
    private mediator: Mediator;

    setMediator(mediator: Mediator): void {
        this.mediator = mediator;
    }

    doA(): void {
        console.log("Colleague1 does A.");
        this.mediator.notify(this, 'A');
    }

    doB(): void {
        console.log("Colleague1 does B.");
        this.mediator.notify(this, 'B');
    }
}

class ConcreteColleague2 implements Colleague {
    private mediator: Mediator;

    setMediator(mediator: Mediator): void {
        this.mediator = mediator;
    }

    doC(): void {
        console.log("Colleague2 does C.");
        this.mediator.notify(this, 'C');
    }

    doD(): void {
        console.log("Colleague2 does D.");
        this.mediator.notify(this, 'D');
    }
}
```

#### Implementing the Concrete Mediator

The Concrete Mediator implements the Mediator interface and coordinates communication between Colleagues.

```typescript
class ConcreteMediator implements Mediator {
    private colleague1: ConcreteColleague1;
    private colleague2: ConcreteColleague2;

    setColleague1(colleague: ConcreteColleague1): void {
        this.colleague1 = colleague;
    }

    setColleague2(colleague: ConcreteColleague2): void {
        this.colleague2 = colleague;
    }

    notify(sender: Colleague, event: string): void {
        if (event === 'A') {
            console.log("Mediator reacts on A and triggers following operations:");
            this.colleague2.doC();
        }

        if (event === 'D') {
            console.log("Mediator reacts on D and triggers following operations:");
            this.colleague1.doB();
        }
    }
}
```

### Benefits of Using TypeScript

TypeScript's strong typing and interface capabilities provide several benefits when implementing the Mediator Pattern:

- **Type Safety**: TypeScript enforces type safety, ensuring that Colleagues interact with the Mediator in a consistent and predictable manner.
- **Code Readability**: Interfaces clearly define the contract between the Mediator and Colleagues, improving code readability and maintainability.
- **Error Reduction**: Compile-time checks help reduce runtime errors by catching type mismatches and incorrect method calls.

### Handling Strongly Typed Messages

In complex systems, messages or events exchanged between Colleagues can be strongly typed to ensure consistency. TypeScript's union types and enums can be used to define a set of valid messages.

```typescript
type Event = 'A' | 'B' | 'C' | 'D';

class ConcreteMediatorWithTypes implements Mediator {
    private colleague1: ConcreteColleague1;
    private colleague2: ConcreteColleague2;

    setColleague1(colleague: ConcreteColleague1): void {
        this.colleague1 = colleague;
    }

    setColleague2(colleague: ConcreteColleague2): void {
        this.colleague2 = colleague;
    }

    notify(sender: Colleague, event: Event): void {
        if (event === 'A') {
            console.log("Mediator reacts on A and triggers following operations:");
            this.colleague2.doC();
        }

        if (event === 'D') {
            console.log("Mediator reacts on D and triggers following operations:");
            this.colleague1.doB();
        }
    }
}
```

### Generic Mediators for Different Colleagues

TypeScript's generics allow you to create flexible and reusable Mediators that can handle different types of Colleagues.

```typescript
interface GenericMediator<T extends Colleague> {
    notify(sender: T, event: string): void;
}

class GenericConcreteMediator<T extends Colleague> implements GenericMediator<T> {
    private colleagues: T[] = [];

    addColleague(colleague: T): void {
        this.colleagues.push(colleague);
    }

    notify(sender: T, event: string): void {
        // Handle notification logic
    }
}
```

### Using Type Guards

Type guards can be employed to handle different types of messages or events in a type-safe manner.

```typescript
function isColleague1(colleague: Colleague): colleague is ConcreteColleague1 {
    return (colleague as ConcreteColleague1).doA !== undefined;
}

class TypeGuardMediator implements Mediator {
    notify(sender: Colleague, event: string): void {
        if (isColleague1(sender)) {
            console.log("Handling event for Colleague1");
        } else {
            console.log("Handling event for another Colleague");
        }
    }
}
```

### Decoupling with Interfaces and Dependency Injection

Decoupling Colleagues from the Mediator can be achieved using interfaces and dependency injection. This approach enhances testability and flexibility.

```typescript
class DependencyInjectedMediator implements Mediator {
    private colleagues: Colleague[] = [];

    register(colleague: Colleague): void {
        this.colleagues.push(colleague);
        colleague.setMediator(this);
    }

    notify(sender: Colleague, event: string): void {
        // Handle notification logic
    }
}
```

### Documenting Interactions and Message Protocols

Documenting the interactions and message protocols between Colleagues and the Mediator is crucial for maintaining and extending the system. Clear documentation helps new developers understand the system's architecture and facilitates debugging.

### Testing Strategies

Testing the Mediator Pattern involves verifying the interactions between Colleagues and the Mediator. Unit tests can be written to test Colleagues and the Mediator in isolation, while integration tests ensure they work together correctly.

#### Testing Colleagues in Isolation

Colleagues can be tested independently by mocking the Mediator.

```typescript
class MockMediator implements Mediator {
    notify(sender: Colleague, event: string): void {
        // Mock implementation
    }
}

// Test ConcreteColleague1
const mockMediator = new MockMediator();
const colleague1 = new ConcreteColleague1();
colleague1.setMediator(mockMediator);
colleague1.doA();
```

#### Testing the Mediator

The Mediator can be tested by verifying that it correctly coordinates communication between Colleagues.

```typescript
const mediator = new ConcreteMediator();
const colleague1 = new ConcreteColleague1();
const colleague2 = new ConcreteColleague2();

mediator.setColleague1(colleague1);
mediator.setColleague2(colleague2);

colleague1.setMediator(mediator);
colleague2.setMediator(mediator);

colleague1.doA(); // Verify that colleague2.doC() is called
```

### Integrating the Mediator Pattern into Frontend Frameworks

The Mediator Pattern can be integrated into frontend frameworks like Angular or React to manage component interactions.

#### Angular Example

In Angular, the Mediator Pattern can be used to manage communication between components.

```typescript
@Injectable()
class AngularMediatorService implements Mediator {
    private components: Colleague[] = [];

    register(component: Colleague): void {
        this.components.push(component);
    }

    notify(sender: Colleague, event: string): void {
        // Handle component communication
    }
}

// Component A
@Component({
    selector: 'app-component-a',
    template: `<button (click)="doSomething()">Do Something</button>`
})
class ComponentA implements Colleague {
    constructor(private mediator: AngularMediatorService) {
        this.mediator.register(this);
    }

    doSomething(): void {
        this.mediator.notify(this, 'somethingHappened');
    }
}
```

#### React Example

In React, the Mediator Pattern can be implemented using context or state management libraries.

```typescript
const MediatorContext = React.createContext<Mediator | null>(null);

const ComponentA: React.FC = () => {
    const mediator = useContext(MediatorContext);

    const handleClick = () => {
        if (mediator) {
            mediator.notify(this, 'somethingHappened');
        }
    };

    return <button onClick={handleClick}>Do Something</button>;
};
```

### Best Practices for Error Handling

When implementing the Mediator Pattern, it is essential to handle errors and exceptions gracefully. This can be achieved by:

- Validating messages before processing them.
- Logging errors for debugging purposes.
- Providing fallback mechanisms to handle failures.

### Potential Pitfalls

While the Mediator Pattern offers many benefits, there are potential pitfalls to consider:

- **Complexity**: Overusing the Mediator Pattern can lead to a complex central mediator that is difficult to maintain.
- **Scalability**: As the number of Colleagues increases, the Mediator can become a bottleneck.
- **Modification**: Adding new Colleagues or modifying existing ones may require changes to the Mediator, affecting the system's flexibility.

### Conclusion

The Mediator Pattern is a powerful tool for managing complex interactions between objects, and TypeScript's features enhance its implementation. By centralizing communication through a Mediator, you can reduce dependencies, improve maintainability, and create more scalable systems. Whether integrating into frontend frameworks or using in backend services, the Mediator Pattern provides a robust solution for managing interactions in a type-safe and efficient manner.

## Quiz Time!

{{< quizdown >}}

### What is the primary purpose of the Mediator Pattern?

- [x] To centralize communication between objects
- [ ] To increase direct dependencies between objects
- [ ] To replace all interfaces in a system
- [ ] To enforce strict type checking

> **Explanation:** The Mediator Pattern centralizes communication between objects, reducing dependencies and promoting loose coupling.

### How does TypeScript enhance the implementation of the Mediator Pattern?

- [x] By providing strong typing and interfaces
- [ ] By eliminating the need for interfaces
- [ ] By enforcing runtime type checks
- [ ] By removing the need for a Mediator

> **Explanation:** TypeScript's strong typing and interfaces help enforce correct usage and improve code readability and maintainability.

### Which TypeScript feature allows for creating flexible and reusable Mediators?

- [x] Generics
- [ ] Enums
- [ ] Type Guards
- [ ] Decorators

> **Explanation:** Generics in TypeScript allow for creating flexible and reusable Mediators that can handle different types of Colleagues.

### What is a potential pitfall of overusing the Mediator Pattern?

- [x] It can lead to a complex central mediator that is difficult to maintain
- [ ] It simplifies all interactions in a system
- [ ] It eliminates the need for any other design patterns
- [ ] It increases the number of direct dependencies

> **Explanation:** Overusing the Mediator Pattern can lead to a complex central mediator that becomes difficult to maintain.

### How can the Mediator Pattern be integrated into Angular applications?

- [x] By using a service to manage component communication
- [ ] By replacing all components with a single mediator component
- [ ] By eliminating the use of services
- [ ] By using only direct component-to-component communication

> **Explanation:** In Angular, the Mediator Pattern can be integrated by using a service to manage communication between components.

### What is a benefit of using TypeScript interfaces in the Mediator Pattern?

- [x] They define clear contracts between the Mediator and Colleagues
- [ ] They remove the need for any implementation
- [ ] They enforce runtime type checks
- [ ] They eliminate the need for a Mediator

> **Explanation:** TypeScript interfaces define clear contracts between the Mediator and Colleagues, improving code readability and maintainability.

### Which strategy can be used to decouple Colleagues from the Mediator?

- [x] Using interfaces and dependency injection
- [ ] Using direct references
- [ ] Using global variables
- [ ] Using only static methods

> **Explanation:** Using interfaces and dependency injection helps decouple Colleagues from the Mediator, enhancing testability and flexibility.

### What is a common testing strategy for the Mediator Pattern?

- [x] Testing Colleagues and the Mediator in isolation and together
- [ ] Only testing the Mediator
- [ ] Only testing Colleagues
- [ ] Avoiding tests for the Mediator Pattern

> **Explanation:** A common testing strategy involves testing Colleagues and the Mediator both in isolation and together to ensure correct interactions.

### How can errors be handled in the Mediator Pattern?

- [x] By validating messages before processing them
- [ ] By ignoring all errors
- [ ] By using only try-catch blocks
- [ ] By logging errors without handling them

> **Explanation:** Errors can be handled by validating messages before processing them, logging errors, and providing fallback mechanisms.

### True or False: The Mediator Pattern eliminates all dependencies between objects.

- [ ] True
- [x] False

> **Explanation:** The Mediator Pattern reduces direct dependencies between objects but does not eliminate all dependencies. It centralizes communication through a mediator.

{{< /quizdown >}}


