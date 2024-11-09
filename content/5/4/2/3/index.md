---
linkTitle: "4.2.3 Strategy Pattern in TypeScript"
title: "Strategy Pattern in TypeScript: Implementing Behavioral Design Patterns with Type Safety"
description: "Explore the Strategy Pattern in TypeScript, leveraging interfaces, generics, and advanced TypeScript features to create flexible and maintainable code."
categories:
- Design Patterns
- TypeScript
- Software Engineering
tags:
- Strategy Pattern
- TypeScript
- Behavioral Design Patterns
- Software Architecture
- Code Maintainability
date: 2024-10-25
type: docs
nav_weight: 423000
---

## 4.2.3 Strategy Pattern in TypeScript

The Strategy Pattern is a powerful behavioral design pattern that enables you to define a family of algorithms, encapsulate each one, and make them interchangeable. This pattern allows the algorithm to vary independently from the clients that use it. In TypeScript, the Strategy Pattern can be implemented using interfaces, generics, and other advanced TypeScript features to ensure type safety and enhance code maintainability.

### Understanding the Strategy Pattern

At its core, the Strategy Pattern consists of three main components:

- **Strategy Interface**: Defines a common interface for all strategies. This interface declares the method that all concrete strategies must implement.
- **Concrete Strategies**: These are classes that implement the Strategy interface, each providing a different implementation of the algorithm.
- **Context**: This is a class that maintains a reference to a Strategy object. The Context interacts with the Strategy interface to execute the algorithm defined by the current strategy.

### Defining the Strategy Interface with TypeScript

In TypeScript, interfaces are used to define the structure that classes must adhere to. This makes them ideal for defining the Strategy interface. Let's start by defining a simple Strategy interface:

```typescript
interface PaymentStrategy {
    pay(amount: number): void;
}
```

This interface declares a `pay` method, which all concrete strategies must implement. The method takes an `amount` as a parameter, representing the amount to be paid.

### Implementing Concrete Strategies

Concrete strategies implement the Strategy interface, providing specific algorithms. Here are two examples of concrete strategies:

```typescript
class CreditCardPayment implements PaymentStrategy {
    private cardNumber: string;
    private cardHolderName: string;

    constructor(cardNumber: string, cardHolderName: string) {
        this.cardNumber = cardNumber;
        this.cardHolderName = cardHolderName;
    }

    pay(amount: number): void {
        console.log(`Paid ${amount} using Credit Card: ${this.cardNumber}`);
    }
}

class PayPalPayment implements PaymentStrategy {
    private email: string;

    constructor(email: string) {
        this.email = email;
    }

    pay(amount: number): void {
        console.log(`Paid ${amount} using PayPal account: ${this.email}`);
    }
}
```

In these examples, `CreditCardPayment` and `PayPalPayment` are concrete strategies that implement the `PaymentStrategy` interface. Each class provides a specific implementation of the `pay` method.

### Using Generics for Flexible Strategies

Generics in TypeScript allow you to create reusable components that work with a variety of data types. This can be particularly useful in the Strategy Pattern when you want strategies to operate on different types of data.

Consider a scenario where you have a sorting strategy that can sort arrays of different data types:

```typescript
interface SortStrategy<T> {
    sort(data: T[]): T[];
}

class BubbleSort<T> implements SortStrategy<T> {
    sort(data: T[]): T[] {
        // Implementation of bubble sort
        return data;
    }
}

class QuickSort<T> implements SortStrategy<T> {
    sort(data: T[]): T[] {
        // Implementation of quick sort
        return data;
    }
}
```

In this example, `SortStrategy` is a generic interface, and `BubbleSort` and `QuickSort` are concrete strategies that implement this interface. The use of generics allows these strategies to sort arrays of any type.

### Benefits of TypeScript in Strategy Pattern

TypeScript offers several benefits when implementing the Strategy Pattern:

- **Type Safety**: TypeScript's static typing ensures that the strategies conform to the defined interface, catching errors at compile time rather than at runtime.
- **Code Readability**: Type annotations and interfaces make the code more readable and self-documenting, which is especially beneficial in larger applications with multiple strategies.
- **Refactoring Support**: TypeScript's type system makes it easier to refactor code safely, as the compiler will alert you to any type mismatches or missing implementations.

### Handling Optional Methods or Properties

In some cases, strategies may have optional methods or properties. TypeScript provides a way to handle this using optional properties in interfaces:

```typescript
interface AdvancedPaymentStrategy {
    pay(amount: number): void;
    refund?(amount: number): void; // Optional method
}
```

In this example, the `refund` method is optional, allowing concrete strategies to implement it if needed.

### Dependency Injection of Strategies

Dependency injection is a design pattern that allows you to inject dependencies into a class, rather than having the class instantiate them itself. This promotes loose coupling and enhances testability.

Here's how you can use dependency injection with the Strategy Pattern:

```typescript
class PaymentContext {
    private strategy: PaymentStrategy;

    constructor(strategy: PaymentStrategy) {
        this.strategy = strategy;
    }

    executePayment(amount: number): void {
        this.strategy.pay(amount);
    }
}
```

In this example, the `PaymentContext` class accepts a `PaymentStrategy` as a constructor parameter, allowing you to inject different strategies at runtime.

### Managing Multiple Strategies

In larger applications, you may have multiple strategies to manage. Here are some strategies for organizing them:

- **Use Enums or Constants**: Define enums or constants for strategy identification, making it easier to select strategies at runtime.

```typescript
enum PaymentMethod {
    CreditCard,
    PayPal,
    Bitcoin
}
```

- **Strategy Registry**: Maintain a registry of available strategies, allowing you to dynamically select and switch between strategies.

```typescript
class StrategyRegistry {
    private strategies: Map<PaymentMethod, PaymentStrategy> = new Map();

    register(method: PaymentMethod, strategy: PaymentStrategy): void {
        this.strategies.set(method, strategy);
    }

    getStrategy(method: PaymentMethod): PaymentStrategy {
        return this.strategies.get(method);
    }
}
```

### Real-World Applications

The Strategy Pattern is widely used in real-world applications. Here are a few examples:

- **Payment Processing**: As demonstrated earlier, different payment methods can be implemented as strategies.
- **Sorting Algorithms**: Different sorting algorithms can be encapsulated as strategies, allowing you to switch between them based on the data size or type.
- **Compression Algorithms**: Different file compression algorithms can be implemented as strategies, enabling you to choose the most efficient one based on file type or size.

### Impact on Maintainability

The Strategy Pattern, when implemented in TypeScript, enhances code maintainability by:

- **Encouraging Modularity**: Each strategy is encapsulated in its class, making it easier to modify or extend without affecting other parts of the code.
- **Facilitating Testing**: Strategies can be tested in isolation, ensuring that each algorithm works correctly.
- **Simplifying Code Changes**: New strategies can be added without modifying existing code, adhering to the Open/Closed Principle.

### Best Practices for Documenting Strategies

Documenting strategies is crucial for maintaining code clarity and understanding. Here are some best practices:

- **Use JSDoc Comments**: Provide detailed comments for each strategy, explaining its purpose and expected behavior.
- **Include Usage Examples**: Show examples of how to use each strategy, making it easier for other developers to understand its application.
- **Document Interfaces**: Clearly define the Strategy interface and any optional methods or properties.

### Refactoring to Use the Strategy Pattern

Refactoring existing code to use the Strategy Pattern can improve flexibility and maintainability. Here are some steps to consider:

1. **Identify the Algorithm**: Determine the algorithm or behavior that varies and encapsulate it in a strategy.
2. **Define the Strategy Interface**: Create an interface that defines the method(s) required by the strategy.
3. **Implement Concrete Strategies**: Develop concrete classes that implement the Strategy interface.
4. **Update the Context**: Modify the context to use the Strategy interface, allowing strategies to be injected dynamically.

### Leveraging Advanced TypeScript Features

TypeScript offers advanced features that can enhance the Strategy Pattern:

- **Discriminated Unions**: Use discriminated unions to handle different strategy types with a common interface, providing type-safe switching between strategies.
- **Type Guards**: Implement type guards to ensure that optional methods are handled correctly.

### Conclusion

The Strategy Pattern is a versatile design pattern that promotes flexibility and maintainability in software design. By leveraging TypeScript's type system and advanced features, you can implement the Strategy Pattern with type safety, ensuring robust and scalable applications. Whether you're developing a simple application or a complex system, the Strategy Pattern can help you manage different algorithms efficiently.

## Quiz Time!

{{< quizdown >}}

### What is the primary purpose of the Strategy Pattern?

- [x] To define a family of algorithms and make them interchangeable
- [ ] To encapsulate object creation
- [ ] To provide a way to access the elements of an aggregate object sequentially
- [ ] To define a one-to-many dependency between objects

> **Explanation:** The Strategy Pattern allows you to define a family of algorithms, encapsulate each one, and make them interchangeable, enabling the algorithm to vary independently from the clients that use it.

### How does TypeScript enhance the Strategy Pattern?

- [x] By providing type safety and catching errors at compile time
- [ ] By allowing dynamic typing
- [ ] By enforcing runtime checks
- [ ] By eliminating the need for interfaces

> **Explanation:** TypeScript enhances the Strategy Pattern by providing type safety, ensuring that strategies conform to the defined interface and catching errors at compile time.

### Which TypeScript feature is useful for strategies that operate on different data types?

- [x] Generics
- [ ] Enums
- [ ] Type Guards
- [ ] Decorators

> **Explanation:** Generics allow you to create reusable components that work with a variety of data types, making them useful for strategies that operate on different data types.

### What is a benefit of using dependency injection with the Strategy Pattern?

- [x] It promotes loose coupling and enhances testability
- [ ] It eliminates the need for interfaces
- [ ] It enforces strict coupling between classes
- [ ] It simplifies algorithm implementation

> **Explanation:** Dependency injection promotes loose coupling by allowing you to inject dependencies into a class, rather than having the class instantiate them itself, enhancing testability.

### How can you handle optional methods in a Strategy interface in TypeScript?

- [x] By using optional properties in interfaces
- [ ] By implementing all methods as required
- [ ] By using enums
- [ ] By creating separate interfaces for each optional method

> **Explanation:** TypeScript allows you to define optional methods in an interface by using the `?` symbol, making it possible for concrete strategies to implement them if needed.

### What is a common use case for the Strategy Pattern in real-world applications?

- [x] Payment processing with different payment methods
- [ ] Creating a singleton instance
- [ ] Iterating over a collection
- [ ] Managing object lifecycles

> **Explanation:** A common use case for the Strategy Pattern is payment processing, where different payment methods can be implemented as strategies.

### Which TypeScript feature can be used to handle different strategy types with a common interface?

- [x] Discriminated Unions
- [ ] Type Aliases
- [ ] Decorators
- [ ] Modules

> **Explanation:** Discriminated unions allow you to handle different strategy types with a common interface, providing type-safe switching between strategies.

### What should you consider when refactoring existing code to use the Strategy Pattern?

- [x] Identify the algorithm to encapsulate and define a Strategy interface
- [ ] Eliminate all interfaces
- [ ] Combine all algorithms into a single class
- [ ] Use dynamic typing for all strategies

> **Explanation:** When refactoring to use the Strategy Pattern, you should identify the algorithm to encapsulate, define a Strategy interface, and implement concrete strategies.

### How can you organize multiple strategies in a larger application?

- [x] Use enums or constants for strategy identification
- [ ] Combine all strategies into a single class
- [ ] Eliminate the use of interfaces
- [ ] Use dynamic typing for all strategies

> **Explanation:** In larger applications, you can organize multiple strategies by using enums or constants for strategy identification, making it easier to select strategies at runtime.

### True or False: The Strategy Pattern allows algorithms to vary independently from the clients that use them.

- [x] True
- [ ] False

> **Explanation:** True. The Strategy Pattern allows algorithms to vary independently from the clients that use them, promoting flexibility and maintainability.

{{< /quizdown >}}
