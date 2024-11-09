---
linkTitle: "3.1.3 Adapter Pattern in TypeScript"
title: "Adapter Pattern in TypeScript: Leveraging TypeScript's Type System for Robust Adapters"
description: "Explore the Adapter Pattern in TypeScript, focusing on type safety, interfaces, and practical implementations. Learn to create adaptable components using TypeScript's powerful features."
categories:
- Design Patterns
- TypeScript
- Software Architecture
tags:
- Adapter Pattern
- TypeScript
- Structural Patterns
- Software Design
- Interfaces
date: 2024-10-25
type: docs
nav_weight: 313000
---

## 3.1.3 Adapter Pattern in TypeScript

The Adapter Pattern is a structural design pattern that allows incompatible interfaces to work together. This pattern is particularly useful in TypeScript, where the robust type system can be leveraged to ensure type safety and prevent interface mismatches. In this section, we will explore how to implement the Adapter Pattern in TypeScript, utilizing interfaces, type-safe implementations, and TypeScript's advanced features to create flexible and reliable adapters.

### Understanding the Adapter Pattern

Before diving into TypeScript specifics, let's briefly revisit the Adapter Pattern. The main goal of the Adapter Pattern is to convert the interface of a class into another interface that a client expects. This allows classes with incompatible interfaces to work together seamlessly.

In TypeScript, the Adapter Pattern can be implemented using interfaces to define the expected interfaces (target and adaptee) and then creating an adapter class that bridges the gap between them.

### Defining Interfaces in TypeScript

TypeScript interfaces are a powerful way to define contracts within your code. They allow you to specify the structure of an object, ensuring that any object adhering to the interface will have the required properties and methods.

#### Example: Target and Adaptee Interfaces

Consider a scenario where we have a `MediaPlayer` interface that defines a method `play()`, and an existing class `AdvancedMediaPlayer` with a method `playAdvanced()`.

```typescript
// Target interface
interface MediaPlayer {
  play(fileName: string): void;
}

// Adaptee class
class AdvancedMediaPlayer {
  playAdvanced(fileName: string): void {
    console.log(`Playing advanced format: ${fileName}`);
  }
}
```

In this example, `MediaPlayer` is the target interface, and `AdvancedMediaPlayer` is the adaptee class. Our goal is to create an adapter that allows `AdvancedMediaPlayer` to be used where a `MediaPlayer` is expected.

### Implementing a Type-Safe Adapter

To implement the adapter, we create a class `MediaAdapter` that implements the `MediaPlayer` interface and internally uses an instance of `AdvancedMediaPlayer` to perform the actual work.

```typescript
// Adapter class
class MediaAdapter implements MediaPlayer {
  private advancedPlayer: AdvancedMediaPlayer;

  constructor(advancedPlayer: AdvancedMediaPlayer) {
    this.advancedPlayer = advancedPlayer;
  }

  play(fileName: string): void {
    this.advancedPlayer.playAdvanced(fileName);
  }
}
```

In this implementation, `MediaAdapter` adapts the `AdvancedMediaPlayer` to the `MediaPlayer` interface by translating the `play()` call into a `playAdvanced()` call. This approach ensures type safety, as `MediaAdapter` strictly adheres to the `MediaPlayer` interface.

### Class Inheritance vs. Composition

In TypeScript, you can implement adapters using either class inheritance or composition. Composition is generally preferred for the Adapter Pattern, as it provides greater flexibility and adheres to the principle of "favor composition over inheritance."

#### Composition Example

In the previous example, we used composition by including an instance of `AdvancedMediaPlayer` within `MediaAdapter`. This approach allows `MediaAdapter` to delegate the work to `AdvancedMediaPlayer` without being tightly coupled to its implementation.

#### Inheritance Example

While less common for adapters, inheritance can be used if the adapter needs to extend the functionality of the adaptee.

```typescript
// Inheritance-based adapter
class InheritedMediaAdapter extends AdvancedMediaPlayer implements MediaPlayer {
  play(fileName: string): void {
    this.playAdvanced(fileName);
  }
}
```

In this example, `InheritedMediaAdapter` extends `AdvancedMediaPlayer` and implements `MediaPlayer`. This approach might be useful if you want to add additional behavior to the `AdvancedMediaPlayer` while adapting it.

### Leveraging TypeScript's Type System

TypeScript's type system provides several features that can enhance the implementation of the Adapter Pattern, such as preventing interface mismatches and supporting generic types.

#### Preventing Interface Mismatches

By using TypeScript interfaces, you can ensure that your adapter correctly implements the target interface. TypeScript will enforce that all methods and properties defined in the interface are present in the adapter, preventing runtime errors due to missing functionality.

#### Using Generic Types

Generic types in TypeScript allow you to create adaptable components that can work with different data types. This can be particularly useful when implementing adapters that need to handle various types of data.

```typescript
// Generic adapter interface
interface GenericAdapter<T> {
  adapt(data: T): any;
}

// Example implementation
class JsonAdapter implements GenericAdapter<string> {
  adapt(data: string): object {
    return JSON.parse(data);
  }
}
```

In this example, `GenericAdapter` is a generic interface that defines an `adapt()` method. `JsonAdapter` implements this interface to convert a JSON string into an object.

### Handling Optional Properties and Methods

TypeScript interfaces can include optional properties and methods, which can be useful when creating adapters for APIs that may not always provide the same set of features.

```typescript
interface OptionalMediaPlayer {
  play(fileName: string): void;
  pause?(fileName: string): void; // Optional method
}

class OptionalMediaAdapter implements OptionalMediaPlayer {
  play(fileName: string): void {
    console.log(`Playing: ${fileName}`);
  }

  pause?(fileName: string): void {
    console.log(`Pausing: ${fileName}`);
  }
}
```

In this example, `pause()` is an optional method in the `OptionalMediaPlayer` interface. `OptionalMediaAdapter` can choose to implement it or not, providing flexibility in how the adapter is used.

### Impact of Access Modifiers

Access modifiers in TypeScript, such as `public`, `private`, and `protected`, can impact the implementation of adapters by controlling the visibility of properties and methods.

- **Public**: Accessible from anywhere.
- **Private**: Accessible only within the class.
- **Protected**: Accessible within the class and its subclasses.

```typescript
class MediaAdapterWithAccess implements MediaPlayer {
  private advancedPlayer: AdvancedMediaPlayer;

  constructor(advancedPlayer: AdvancedMediaPlayer) {
    this.advancedPlayer = advancedPlayer;
  }

  public play(fileName: string): void {
    this.advancedPlayer.playAdvanced(fileName);
  }
}
```

In this example, `advancedPlayer` is a private member, ensuring that it cannot be accessed directly from outside the `MediaAdapterWithAccess` class. This encapsulation is crucial for maintaining the integrity of the adapter's implementation.

### Testing Adapters with TypeScript's Strict Type Checks

TypeScript's strict type checks can be leveraged to test adapters thoroughly. By ensuring that your adapters adhere to the defined interfaces, you can catch potential issues at compile time rather than at runtime.

#### Example: Writing Unit Tests

```typescript
import { expect } from 'chai';

describe('MediaAdapter', () => {
  it('should play using AdvancedMediaPlayer', () => {
    const advancedPlayer = new AdvancedMediaPlayer();
    const adapter = new MediaAdapter(advancedPlayer);

    expect(() => adapter.play('file.mp3')).to.not.throw();
  });
});
```

In this test, we verify that the `MediaAdapter` correctly delegates the `play()` method to `AdvancedMediaPlayer`. By using TypeScript's type system, we can ensure that the adapter is implemented correctly and adheres to the expected interface.

### Documenting Type Relationships

Clear documentation of type relationships is essential when implementing the Adapter Pattern in TypeScript. This includes documenting the interfaces, classes, and their interactions, which helps maintain the codebase and facilitates collaboration among developers.

```typescript
/**
 * MediaPlayer interface defines the contract for media playback.
 */
interface MediaPlayer {
  play(fileName: string): void;
}

/**
 * AdvancedMediaPlayer provides advanced playback capabilities.
 */
class AdvancedMediaPlayer {
  playAdvanced(fileName: string): void {
    console.log(`Playing advanced format: ${fileName}`);
  }
}

/**
 * MediaAdapter adapts AdvancedMediaPlayer to MediaPlayer interface.
 */
class MediaAdapter implements MediaPlayer {
  private advancedPlayer: AdvancedMediaPlayer;

  constructor(advancedPlayer: AdvancedMediaPlayer) {
    this.advancedPlayer = advancedPlayer;
  }

  play(fileName: string): void {
    this.advancedPlayer.playAdvanced(fileName);
  }
}
```

### Handling Third-Party Libraries Lacking Type Definitions

When working with third-party libraries that lack type definitions, you can create custom type definitions to ensure type safety in your TypeScript projects.

#### Example: Creating a Type Definition

Suppose you have a third-party library without type definitions. You can create a `d.ts` file to define the types.

```typescript
// custom-library.d.ts
declare module 'custom-library' {
  export function customFunction(param: string): void;
}
```

By creating type definitions, you can integrate third-party libraries into your TypeScript projects while maintaining type safety and preventing interface mismatches.

### Conclusion

The Adapter Pattern is a powerful tool for integrating incompatible interfaces, and TypeScript's type system enhances its implementation by providing type safety and preventing interface mismatches. By leveraging interfaces, generic types, and access modifiers, you can create robust and flexible adapters that adhere to the principles of clean code and maintainability.

Incorporating TypeScript's features into your adapter implementations ensures that your code is reliable, easy to understand, and adaptable to future changes. By documenting type relationships and handling third-party libraries effectively, you can maintain a high standard of quality and consistency across your codebase.

### References and Further Reading

- [TypeScript Official Documentation](https://www.typescriptlang.org/docs/)
- [Design Patterns: Elements of Reusable Object-Oriented Software](https://en.wikipedia.org/wiki/Design_Patterns)
- [Refactoring Guru: Adapter Pattern](https://refactoring.guru/design-patterns/adapter)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)

## Quiz Time!

{{< quizdown >}}

### Which TypeScript feature helps prevent interface mismatches in adapter implementations?

- [x] TypeScript interfaces
- [ ] TypeScript decorators
- [ ] TypeScript enums
- [ ] TypeScript modules

> **Explanation:** TypeScript interfaces define contracts that ensure the correct implementation of methods and properties, helping prevent interface mismatches.

### What is the preferred approach for implementing adapters in TypeScript?

- [x] Composition
- [ ] Inheritance
- [ ] Singleton
- [ ] Factory

> **Explanation:** Composition is preferred as it provides greater flexibility and adheres to the principle of "favor composition over inheritance."

### How can TypeScript's type system enhance testing of adapters?

- [x] By ensuring adherence to defined interfaces
- [ ] By automatically generating test cases
- [ ] By providing runtime type checks
- [ ] By enforcing strict null checks

> **Explanation:** TypeScript's type system enforces adherence to defined interfaces, allowing potential issues to be caught at compile time.

### What is the role of access modifiers in adapter implementation?

- [x] They control visibility and encapsulation of properties and methods.
- [ ] They define the data type of properties.
- [ ] They automatically generate interfaces.
- [ ] They enforce runtime checks.

> **Explanation:** Access modifiers control the visibility of properties and methods, ensuring encapsulation and integrity of the adapter's implementation.

### How can you handle optional methods in TypeScript interfaces?

- [x] By using the `?` symbol to mark methods as optional
- [ ] By defining them in a separate interface
- [ ] By using TypeScript decorators
- [ ] By implementing them as abstract methods

> **Explanation:** The `?` symbol in TypeScript interfaces marks methods as optional, allowing flexibility in implementation.

### What should you do when a third-party library lacks type definitions?

- [x] Create custom type definitions
- [ ] Avoid using the library
- [ ] Use any type for all interactions
- [ ] Implement a wrapper class

> **Explanation:** Creating custom type definitions ensures type safety and integration of third-party libraries into TypeScript projects.

### Which TypeScript feature allows creating adaptable components for various data types?

- [x] Generic types
- [ ] Type unions
- [ ] Type aliases
- [ ] Type assertions

> **Explanation:** Generic types allow the creation of adaptable components that can work with different data types.

### What is the benefit of documenting type relationships in TypeScript?

- [x] It helps maintain the codebase and facilitates collaboration.
- [ ] It automatically generates code documentation.
- [ ] It improves runtime performance.
- [ ] It enforces stricter type checks.

> **Explanation:** Documenting type relationships helps maintain the codebase and facilitates collaboration among developers.

### Which of the following is an example of an optional property in TypeScript?

- [x] `propertyName?: string;`
- [ ] `propertyName: string;`
- [ ] `propertyName! : string;`
- [ ] `propertyName: string | undefined;`

> **Explanation:** The `?` symbol indicates an optional property in TypeScript.

### True or False: In TypeScript, inheritance is always preferred over composition for implementing adapters.

- [ ] True
- [x] False

> **Explanation:** Composition is generally preferred over inheritance for implementing adapters due to its flexibility and adherence to design principles.

{{< /quizdown >}}
