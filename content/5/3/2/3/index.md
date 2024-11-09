---
linkTitle: "3.2.3 Decorator Pattern in TypeScript"
title: "Decorator Pattern in TypeScript: Enhancing Code with Custom Decorators"
description: "Explore the Decorator Pattern in TypeScript, learn how to create custom decorators, enable experimental features, and apply best practices for effective code enhancement."
categories:
- Design Patterns
- TypeScript
- Software Architecture
tags:
- Decorator Pattern
- TypeScript
- Custom Decorators
- Software Design
- Code Enhancement
date: 2024-10-25
type: docs
nav_weight: 323000
---

## 3.2.3 Decorator Pattern in TypeScript

The Decorator Pattern is a structural design pattern that allows behavior to be added to individual objects, either statically or dynamically, without affecting the behavior of other objects from the same class. In TypeScript, decorators provide a powerful way to modify the behavior of classes, methods, properties, and parameters through metadata and reflection. This section delves into how TypeScript supports decorators, the syntax for creating custom decorators, and practical applications of this pattern.

### Understanding Decorators in TypeScript

Decorators in TypeScript are an experimental feature that allows you to attach metadata to classes, methods, accessors, properties, and parameters. This metadata can then be used to modify the behavior of the decorated element. The concept of decorators is inspired by the decorator pattern, but in TypeScript, they are implemented as functions that are prefixed with an `@` symbol.

#### Enabling Experimental Decorators

Before using decorators in TypeScript, you need to enable them in your project. This is done by setting the `experimentalDecorators` option to `true` in your `tsconfig.json` file:

```json
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```

This setting is necessary because decorators are not part of the ECMAScript standard yet and are considered an experimental feature in TypeScript.

### Types of Decorators

TypeScript supports several types of decorators:

- **Class Decorators**: Used to modify the behavior of a class.
- **Method Decorators**: Applied to methods within a class.
- **Accessor Decorators**: Used for getters and setters of properties.
- **Property Decorators**: Applied to properties within a class.
- **Parameter Decorators**: Used to access metadata about the parameters of a class method.

Each type of decorator serves a specific purpose and can be used to enhance different aspects of a class or its members.

#### Class Decorators

A class decorator is a function that takes a class constructor as its only argument and returns a new constructor or modifies the existing one. Here's a simple example:

```typescript
function sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

@sealed
class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }
  greet() {
    return `Hello, ${this.greeting}`;
  }
}
```

In this example, the `sealed` decorator seals the class and its prototype, preventing any further modifications.

#### Method Decorators

Method decorators are applied to the methods of a class. They receive three arguments: the target object, the name of the method, and the property descriptor of the method. Here's how you can create a method decorator:

```typescript
function log(target: Object, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${propertyKey} with arguments: ${JSON.stringify(args)}`);
    return originalMethod.apply(this, args);
  };
}

class Calculator {
  @log
  add(a: number, b: number): number {
    return a + b;
  }
}

const calculator = new Calculator();
calculator.add(2, 3);
```

The `log` decorator logs the method name and its arguments each time the method is called.

#### Property Decorators

Property decorators are applied to properties of a class. They receive two arguments: the target object and the name of the property. Here's an example:

```typescript
function readonly(target: Object, propertyKey: string) {
  Object.defineProperty(target, propertyKey, {
    writable: false
  });
}

class Person {
  @readonly
  name: string = "John Doe";
}

const person = new Person();
person.name = "Jane Doe"; // Error: Cannot assign to read-only property
```

The `readonly` decorator makes the `name` property immutable.

### Creating Custom Decorators

Creating custom decorators in TypeScript involves defining a function that follows the decorator signature for the element you wish to decorate. Here's how you can create a custom class decorator:

```typescript
function timestamp<T extends { new (...args: any[]): {} }>(constructor: T) {
  return class extends constructor {
    timestamp = new Date();
  };
}

@timestamp
class Document {
  title: string;
  constructor(title: string) {
    this.title = title;
  }
}

const doc = new Document("My Document");
console.log(doc.timestamp); // Outputs the timestamp when the instance was created
```

In this example, the `timestamp` decorator adds a `timestamp` property to the class, which is set to the current date and time when an instance is created.

### Metadata Reflection in TypeScript

TypeScript supports metadata reflection through the `reflect-metadata` library, which allows you to attach and retrieve metadata from objects. This is particularly useful for advanced decorator implementations, such as dependency injection frameworks.

To use metadata reflection, you need to install the `reflect-metadata` package and import it at the top of your TypeScript file:

```bash
npm install reflect-metadata --save
```

```typescript
import "reflect-metadata";

function logType(target: any, key: string) {
  const type = Reflect.getMetadata("design:type", target, key);
  console.log(`${key} type: ${type.name}`);
}

class Demo {
  @logType
  myProperty: string;
}
```

The `logType` decorator uses `Reflect.getMetadata` to log the type of the decorated property.

### Handling Decorator Order and Side Effects

The order in which decorators are applied is important and can lead to unexpected side effects if not managed properly. Decorators are applied from top to bottom but executed in reverse order (bottom to top). Consider the following example:

```typescript
function first() {
  console.log("first(): evaluated");
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log("first(): called");
  };
}

function second() {
  console.log("second(): evaluated");
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log("second(): called");
  };
}

class Example {
  @first()
  @second()
  method() {}
}

// Output:
// first(): evaluated
// second(): evaluated
// second(): called
// first(): called
```

In this example, `first` is evaluated before `second`, but `second` is called before `first`.

### Best Practices for Using Decorators

- **Keep Decorators Simple**: Decorators should focus on a single responsibility to maintain readability and ease of understanding.
- **Avoid Side Effects**: Ensure that decorators do not introduce unintended side effects that could affect other parts of the application.
- **Document Decorators**: Clearly document what each decorator does and any dependencies it may have.
- **Test Thoroughly**: Since decorators can alter the behavior of classes and methods, ensure they are covered by tests to prevent regressions.

### The Future of Decorators in ECMAScript

Decorators are currently an experimental feature in TypeScript and are not part of the ECMAScript standard. However, they are being considered for inclusion in future versions of ECMAScript. The TC39 committee, which is responsible for evolving the ECMAScript language, is actively working on a proposal for decorators. This proposal may introduce changes to how decorators are implemented and used in the future.

### Practical Applications of Decorators

Decorators have numerous practical applications, including:

- **Dependency Injection**: Decorators can be used to inject dependencies into classes, promoting loose coupling and enhancing testability.
- **Validation**: Decorators can validate inputs and ensure that methods receive the correct types and values.
- **Logging and Monitoring**: Decorators can log method calls and monitor performance metrics.
- **Access Control**: Decorators can enforce access control by checking user permissions before executing a method.

#### Example: Dependency Injection

Here's an example of how decorators can be used for dependency injection:

```typescript
import "reflect-metadata";

function Injectable() {
  return function (target: any) {
    Reflect.defineMetadata("injectable", true, target);
  };
}

function Inject(serviceIdentifier: string) {
  return function (target: any, key: string, index: number) {
    const existingInjectedParameters: any[] =
      Reflect.getOwnMetadata("inject", target, key) || [];
    existingInjectedParameters.push({ index, serviceIdentifier });
    Reflect.defineMetadata("inject", existingInjectedParameters, target, key);
  };
}

@Injectable()
class ServiceA {}

@Injectable()
class ServiceB {
  constructor(@Inject("ServiceA") private serviceA: ServiceA) {}
}
```

In this example, the `Injectable` decorator marks a class as injectable, and the `Inject` decorator specifies the dependencies to be injected.

### Conclusion

The Decorator Pattern in TypeScript provides a robust mechanism for enhancing and modifying the behavior of classes and their members. By leveraging decorators, developers can create clean, maintainable, and reusable code. However, it's important to adhere to best practices and be mindful of potential issues with decorator ordering and side effects. As decorators evolve and potentially become part of the ECMAScript standard, they will continue to play a crucial role in modern software development.

## Quiz Time!

{{< quizdown >}}

### What is a decorator in TypeScript?

- [x] A function that modifies the behavior of a class or its members.
- [ ] A TypeScript feature that enforces type safety.
- [ ] A syntax for defining classes and interfaces.
- [ ] A method for handling asynchronous operations.

> **Explanation:** Decorators are functions that modify the behavior of classes or their members by attaching metadata or altering functionality.

### How do you enable decorators in a TypeScript project?

- [x] By setting `experimentalDecorators` to `true` in `tsconfig.json`.
- [ ] By importing the `decorators` module.
- [ ] By using the `@enableDecorators` directive.
- [ ] By setting `allowDecorators` to `true` in `tsconfig.json`.

> **Explanation:** Decorators are an experimental feature in TypeScript and must be enabled by setting `experimentalDecorators` to `true` in the `tsconfig.json` file.

### Which of the following is NOT a type of decorator in TypeScript?

- [ ] Class Decorator
- [ ] Method Decorator
- [ ] Property Decorator
- [x] Interface Decorator

> **Explanation:** TypeScript supports class, method, property, accessor, and parameter decorators, but not interface decorators.

### What is a potential issue with decorator ordering?

- [x] Decorators are evaluated in the order they are applied but executed in reverse order.
- [ ] Decorators can only be applied to methods, not classes.
- [ ] Decorators cannot be used with asynchronous functions.
- [ ] Decorators always execute in the order they are applied.

> **Explanation:** Decorators are evaluated in the order they are applied but executed in reverse order, which can lead to unexpected behavior if not managed properly.

### What library is used for metadata reflection in TypeScript?

- [x] `reflect-metadata`
- [ ] `metadata-reflect`
- [ ] `type-metadata`
- [ ] `reflective-metadata`

> **Explanation:** The `reflect-metadata` library is used in TypeScript for metadata reflection, allowing decorators to attach and retrieve metadata.

### What is a practical application of decorators?

- [x] Dependency Injection
- [ ] File I/O Operations
- [ ] Network Requests
- [ ] Database Queries

> **Explanation:** Decorators can be used for dependency injection, among other applications like validation and logging.

### How can you make a class property read-only using a decorator?

- [x] By defining a property decorator that sets the `writable` attribute to `false`.
- [ ] By using the `@readonly` directive in TypeScript.
- [ ] By setting the `readonly` attribute in the class constructor.
- [ ] By using a method decorator on the property.

> **Explanation:** A property decorator can modify the property descriptor to set the `writable` attribute to `false`, making it read-only.

### What is the current status of decorators in ECMAScript?

- [x] They are an experimental feature and not yet part of the ECMAScript standard.
- [ ] They are fully integrated into the ECMAScript standard.
- [ ] They are deprecated and no longer supported.
- [ ] They are only available in older versions of ECMAScript.

> **Explanation:** Decorators are currently an experimental feature in TypeScript and are being considered for inclusion in future ECMAScript standards.

### Which decorator type is used to modify the behavior of a class method?

- [ ] Class Decorator
- [x] Method Decorator
- [ ] Property Decorator
- [ ] Parameter Decorator

> **Explanation:** Method decorators are used to modify the behavior of class methods by altering their property descriptors.

### True or False: Decorators can introduce side effects if not used carefully.

- [x] True
- [ ] False

> **Explanation:** Decorators can introduce side effects, especially if they modify shared state or affect execution order, so they should be used carefully to avoid unintended consequences.

{{< /quizdown >}}
