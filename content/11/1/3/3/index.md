---
linkTitle: "1.3.3 Interfaces and Abstract Classes in TypeScript"
title: "Interfaces and Abstract Classes in TypeScript: Understanding Contracts and Abstractions"
description: "Explore the use of interfaces and abstract classes in TypeScript to define contracts and abstractions, enhancing code reusability and maintainability."
categories:
- TypeScript
- Object-Oriented Programming
- Software Design
tags:
- TypeScript Interfaces
- Abstract Classes
- OOP
- Code Reusability
- Design Patterns
date: 2024-10-25
type: docs
nav_weight: 133000
---

## 1.3.3 Interfaces and Abstract Classes in TypeScript

In the realm of TypeScript, interfaces and abstract classes are pivotal constructs that enable developers to define clear contracts and abstractions within their code. These constructs not only promote code reusability and maintainability but also enhance type safety and robustness. In this section, we will delve into the intricacies of interfaces and abstract classes, exploring their differences, use cases, and best practices.

### Understanding Interfaces in TypeScript

Interfaces in TypeScript serve as blueprints for objects, defining the structure that an object must adhere to. They are a way to enforce the shape of objects and class implementations, ensuring that the code adheres to a predefined contract.

#### Defining Interfaces

An interface in TypeScript is defined using the `interface` keyword. It can specify properties and methods that an object or class must implement.

```typescript
interface Vehicle {
  make: string;
  model: string;
  year: number;
  startEngine(): void;
}
```

In this example, the `Vehicle` interface defines a contract that any object or class implementing it must have `make`, `model`, `year` properties, and a `startEngine` method.

#### Implementing Interfaces in Classes

Classes can implement interfaces using the `implements` keyword. This ensures that the class adheres to the interface's structure.

```typescript
class Car implements Vehicle {
  constructor(
    public make: string,
    public model: string,
    public year: number
  ) {}

  startEngine(): void {
    console.log(`Starting the engine of the ${this.make} ${this.model}.`);
  }
}

const myCar = new Car('Toyota', 'Camry', 2020);
myCar.startEngine();
```

Here, the `Car` class implements the `Vehicle` interface, ensuring that it has all the properties and methods defined by the interface.

#### Optional and Read-Only Properties

Interfaces can also define optional and read-only properties. Optional properties are denoted by a question mark (`?`), while read-only properties use the `readonly` modifier.

```typescript
interface Book {
  title: string;
  author: string;
  readonly ISBN: string;
  publisher?: string;
}

const myBook: Book = {
  title: 'TypeScript Handbook',
  author: 'Microsoft',
  ISBN: '123-456-789',
};

// myBook.ISBN = '987-654-321'; // Error: Cannot assign to 'ISBN' because it is a read-only property.
```

In this example, `ISBN` is a read-only property, and `publisher` is optional.

### Exploring Abstract Classes

Abstract classes in TypeScript provide a way to define a base class with common functionality that other classes can inherit. Unlike interfaces, abstract classes can contain implementation details and state.

#### Defining Abstract Classes

An abstract class is defined using the `abstract` keyword. It can include abstract methods (without implementation) and concrete methods (with implementation).

```typescript
abstract class Animal {
  constructor(public name: string) {}

  abstract makeSound(): void;

  move(): void {
    console.log(`${this.name} is moving.`);
  }
}
```

The `Animal` class defines an abstract method `makeSound` and a concrete method `move`.

#### Extending Abstract Classes

Classes that extend an abstract class must implement all abstract methods.

```typescript
class Dog extends Animal {
  makeSound(): void {
    console.log('Woof! Woof!');
  }
}

const myDog = new Dog('Buddy');
myDog.makeSound();
myDog.move();
```

Here, the `Dog` class extends `Animal` and implements the `makeSound` method.

### Interfaces vs. Abstract Classes

While both interfaces and abstract classes are used to define contracts and abstractions, they serve different purposes and have distinct characteristics.

- **Interfaces**:
  - Define a contract without implementation.
  - Support multiple inheritance (a class can implement multiple interfaces).
  - Ideal for defining the shape of objects and ensuring type safety.

- **Abstract Classes**:
  - Can provide both abstract and concrete methods.
  - Support single inheritance (a class can extend only one abstract class).
  - Suitable for sharing common functionality and state across related classes.

### When to Use Interfaces and Abstract Classes

Choosing between interfaces and abstract classes depends on the specific use case and design requirements.

- **Use Interfaces**:
  - When you need to define a contract for unrelated classes.
  - When you want to enforce the shape of an object.
  - For type-checking and enhancing code robustness.

- **Use Abstract Classes**:
  - When you need to share common functionality and state.
  - When you want to provide a base implementation for related classes.
  - When you need to use protected members or constructors.

### Promoting Code Reusability and Maintainability

Both interfaces and abstract classes promote code reusability and maintainability by enforcing consistent contracts and abstractions.

- **Code Reusability**: By defining common contracts and abstractions, interfaces and abstract classes allow developers to reuse code across different parts of an application.
- **Maintainability**: They provide a clear structure and contract, making it easier to understand and modify the codebase.

### Generics with Interfaces and Abstract Classes

Generics add a layer of flexibility to interfaces and abstract classes, allowing them to work with various data types.

#### Generic Interfaces

```typescript
interface Repository<T> {
  getById(id: string): T;
  save(entity: T): void;
}

class UserRepository implements Repository<User> {
  getById(id: string): User {
    // Implementation
  }

  save(user: User): void {
    // Implementation
  }
}
```

In this example, the `Repository` interface is generic, allowing it to work with any data type.

#### Generic Abstract Classes

```typescript
abstract class Service<T> {
  abstract getAll(): T[];
}

class ProductService extends Service<Product> {
  getAll(): Product[] {
    // Implementation
  }
}
```

Here, the `Service` abstract class is generic, enabling it to be extended for different data types.

### Best Practices for Naming and Organizing

- **Naming**: Use clear and descriptive names for interfaces and abstract classes. Prefix interfaces with `I` (e.g., `IVehicle`) if it helps clarify their purpose.
- **Organizing**: Group related interfaces and abstract classes together in modules or folders to enhance readability and maintainability.

### Polymorphism with Interfaces and Abstract Classes

Polymorphism allows objects to be treated as instances of their parent type. Interfaces and abstract classes facilitate polymorphism by defining common contracts.

```typescript
interface Shape {
  draw(): void;
}

class Circle implements Shape {
  draw(): void {
    console.log('Drawing a circle.');
  }
}

class Square implements Shape {
  draw(): void {
    console.log('Drawing a square.');
  }
}

function renderShape(shape: Shape) {
  shape.draw();
}

const shapes: Shape[] = [new Circle(), new Square()];
shapes.forEach(renderShape);
```

In this example, both `Circle` and `Square` implement the `Shape` interface, allowing them to be used polymorphically.

### Exercises for Practice

1. **Define and Implement Interfaces**: Create an interface for a `Person` with properties like `name`, `age`, and a method `greet()`. Implement this interface in a class and create an instance.

2. **Create Abstract Classes**: Define an abstract class `Appliance` with a method `turnOn()`. Extend this class with concrete classes like `WashingMachine` and `Refrigerator`, implementing the `turnOn` method.

3. **Use Generics**: Create a generic interface `Storage<T>` with methods `add(item: T)` and `get(index: number): T`. Implement this interface in a class and test it with different data types.

4. **Polymorphism with Interfaces**: Define an interface `Instrument` with a method `play()`. Implement this interface in classes like `Guitar` and `Piano`. Write a function that accepts an `Instrument` and calls `play()`.

### Conclusion

Interfaces and abstract classes are powerful tools in TypeScript that provide a robust foundation for building scalable and maintainable applications. By defining clear contracts and abstractions, they promote code reusability and type safety, enabling developers to create flexible and reliable software solutions. Understanding when and how to use these constructs is essential for mastering object-oriented programming in TypeScript.

## Quiz Time!

{{< quizdown >}}

### What is the primary purpose of interfaces in TypeScript?

- [x] To define a contract for objects and classes
- [ ] To provide implementation details for classes
- [ ] To allow multiple inheritance in TypeScript
- [ ] To enforce private property access

> **Explanation:** Interfaces in TypeScript are used to define a contract that objects and classes must adhere to, specifying the structure they must follow.

### How do abstract classes differ from interfaces?

- [x] Abstract classes can have implementation details; interfaces cannot.
- [ ] Interfaces can have implementation details; abstract classes cannot.
- [ ] Abstract classes support multiple inheritance; interfaces do not.
- [ ] Interfaces can be instantiated; abstract classes cannot.

> **Explanation:** Abstract classes can contain both abstract methods (without implementation) and concrete methods (with implementation), whereas interfaces only define a contract without implementation.

### Which of the following is a valid use case for an abstract class?

- [x] Sharing common functionality and state across related classes
- [ ] Defining a contract for unrelated classes
- [ ] Enforcing the shape of an object
- [ ] Allowing multiple inheritance

> **Explanation:** Abstract classes are suitable for sharing common functionality and state across related classes, providing a base implementation that can be extended.

### What is the significance of using generics with interfaces?

- [x] They allow interfaces to work with various data types.
- [ ] They restrict interfaces to a single data type.
- [ ] They enable interfaces to have private properties.
- [ ] They provide implementation details for interfaces.

> **Explanation:** Generics allow interfaces to be flexible and work with multiple data types, enhancing their reusability and adaptability.

### Which keyword is used to define an abstract class in TypeScript?

- [x] `abstract`
- [ ] `interface`
- [ ] `class`
- [ ] `extends`

> **Explanation:** The `abstract` keyword is used to define an abstract class in TypeScript, indicating that it contains abstract methods that must be implemented by derived classes.

### How can optional properties be defined in a TypeScript interface?

- [x] By using a question mark (`?`) after the property name
- [ ] By using the `optional` keyword
- [ ] By using the `readonly` keyword
- [ ] By defining them in a separate interface

> **Explanation:** Optional properties in a TypeScript interface are denoted by a question mark (`?`) after the property name, indicating that they are not required.

### What is the purpose of the `readonly` modifier in an interface?

- [x] To make a property immutable after its initial assignment
- [ ] To allow a property to be modified only within the class
- [ ] To make a property optional
- [ ] To hide a property from external access

> **Explanation:** The `readonly` modifier in an interface makes a property immutable after its initial assignment, preventing further modifications.

### Which of the following is a best practice for naming interfaces?

- [x] Use clear and descriptive names
- [ ] Prefix all interface names with `I`
- [ ] Use single-letter names for brevity
- [ ] Avoid using names that describe the interface's purpose

> **Explanation:** Using clear and descriptive names for interfaces enhances readability and understanding, making it easier to identify their purpose.

### What is polymorphism in the context of interfaces?

- [x] Treating objects as instances of their parent type
- [ ] Allowing multiple inheritance
- [ ] Enforcing private property access
- [ ] Providing implementation details for methods

> **Explanation:** Polymorphism allows objects to be treated as instances of their parent type, enabling flexible and interchangeable use of objects that implement the same interface.

### True or False: An abstract class can have a constructor.

- [x] True
- [ ] False

> **Explanation:** An abstract class can have a constructor, allowing it to initialize properties and perform setup tasks for derived classes.

{{< /quizdown >}}
