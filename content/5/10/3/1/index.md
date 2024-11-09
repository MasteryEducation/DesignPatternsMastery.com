---
linkTitle: "10.3.1 Introduction to Decorators in TypeScript"
title: "TypeScript Decorators: A Comprehensive Introduction"
description: "Explore the world of TypeScript decorators: their purpose, usage, and best practices for enhancing code with annotations and metadata."
categories:
- TypeScript
- Metaprogramming
- Software Design
tags:
- TypeScript Decorators
- Metaprogramming
- Code Annotations
- Software Development
- JavaScript
date: 2024-10-25
type: docs
nav_weight: 1031000
---

## 10.3.1 Introduction to Decorators in TypeScript

Decorators in TypeScript are a powerful feature that allows developers to modify and enhance classes, methods, properties, and parameters with additional behavior and metadata. They provide a declarative way to apply cross-cutting concerns, such as logging, validation, or dependency injection, without cluttering the core logic of the application. In this section, we will delve into the intricacies of decorators, exploring their syntax, use cases, and best practices.

### Understanding Decorators

Decorators are special declarations prefixed with the `@` symbol, which can be applied to various elements of a class. They are essentially functions that receive a target and can modify its behavior or add metadata. This capability makes decorators an essential tool for metaprogramming in TypeScript, allowing developers to write cleaner, more maintainable code.

#### What Decorators Can Do

- **Modify Classes and Methods:** Decorators can alter the behavior of a class or method by wrapping or replacing its implementation.
- **Add Metadata:** They can attach metadata to a class or its members, which can be used for reflection or configuration purposes.
- **Facilitate Dependency Injection:** Decorators can inject dependencies into classes or methods, promoting loose coupling and testability.
- **Implement Cross-Cutting Concerns:** Common concerns like logging, caching, or access control can be implemented using decorators, keeping the core logic clean.

### Enabling Decorators in TypeScript

As of now, decorators are an experimental feature in TypeScript and must be explicitly enabled in the compiler options. To use decorators, you need to add the following configuration to your `tsconfig.json` file:

```json
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```

This setting allows TypeScript to recognize and process decorator syntax during compilation.

### Applying Decorators

Decorators can be applied to classes, methods, accessors, properties, and parameters. Let's explore each type with examples.

#### Class Decorators

A class decorator is a function that takes a class constructor as an argument and can modify or replace it. Here's a simple example:

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

In this example, the `sealed` decorator seals the class, preventing new properties from being added to it or its prototype.

#### Method Decorators

Method decorators are applied to class methods and can modify their behavior. They receive three arguments: the target object, the method name, and the property descriptor.

```typescript
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Method ${propertyKey} called with args: ${args}`);
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
calculator.add(2, 3); // Logs: "Method add called with args: 2,3"
```

Here, the `log` decorator wraps the `add` method to log its invocation details.

#### Accessor Decorators

Accessor decorators are similar to method decorators but are applied to getters and setters. They can modify the behavior of property accessors.

```typescript
function configurable(value: boolean) {
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    descriptor.configurable = value;
  };
}

class Point {
  private _x: number = 0;

  @configurable(false)
  get x() {
    return this._x;
  }

  set x(value: number) {
    this._x = value;
  }
}
```

In this case, the `configurable` decorator sets the configurability of the `x` accessor.

#### Property Decorators

Property decorators are applied to class properties and can be used to add metadata or modify property behavior.

```typescript
function readonly(target: any, propertyKey: string) {
  Object.defineProperty(target, propertyKey, {
    writable: false,
  });
}

class Book {
  @readonly
  title: string = "The Great Gatsby";
}

const book = new Book();
book.title = "New Title"; // Error: Cannot assign to read-only property
```

The `readonly` decorator makes the `title` property immutable.

#### Parameter Decorators

Parameter decorators are used to annotate parameters within a method. They receive three arguments: the target object, the method name, and the parameter index.

```typescript
function logParameter(target: any, propertyKey: string, parameterIndex: number) {
  const existingParameters: number[] = Reflect.getOwnMetadata("log_parameters", target, propertyKey) || [];
  existingParameters.push(parameterIndex);
  Reflect.defineMetadata("log_parameters", existingParameters, target, propertyKey);
}

class UserService {
  greet(@logParameter name: string) {
    console.log(`Hello, ${name}`);
  }
}
```

Here, the `logParameter` decorator adds metadata about the decorated parameter.

### Execution Order of Decorators

The execution order of decorators is crucial to understand how they affect the target they are applied to. Decorators are applied in the following order:

1. **Parameter Decorators**, for each parameter.
2. **Method, Accessor, or Property Decorators**, for each member.
3. **Class Decorators**, for the class itself.

Within each category, decorators are applied in reverse order of their declaration. This means the last decorator in the code is executed first.

### Use Cases for Decorators

Decorators are versatile and can be used in various scenarios:

- **Logging:** Automatically log method calls and parameters.
- **Authentication:** Check user permissions before executing a method.
- **Validation:** Validate method arguments or class properties.
- **Dependency Injection:** Inject dependencies into classes or methods.
- **Caching:** Cache method results based on input parameters.

### Writing Custom Decorators

Creating custom decorators involves defining a function that takes specific arguments based on the decorator type. Here's a guide to writing a simple method decorator:

1. **Define the Decorator Function:** The function should take the target, property key, and descriptor as arguments.

```typescript
function myDecorator(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  // Modify the method or add behavior
}
```

2. **Apply the Decorator:** Use the `@` syntax to apply the decorator to a method.

```typescript
class MyClass {
  @myDecorator
  myMethod() {
    // Method logic
  }
}
```

3. **Test the Decorator:** Ensure the decorator behaves as expected in different scenarios.

### Types of Decorators

- **Class Decorators:** Modify or replace the class constructor.
- **Method Decorators:** Wrap or modify method behavior.
- **Accessor Decorators:** Alter getter/setter functionality.
- **Property Decorators:** Add metadata or modify property behavior.
- **Parameter Decorators:** Annotate method parameters.

### Best Practices for Decorators

- **Naming Conventions:** Use descriptive names that reflect the decorator's purpose.
- **Organize Decorators:** Group related decorators in separate files or modules.
- **Document Behavior:** Clearly document what each decorator does and its impact.
- **Test Thoroughly:** Test decorators in isolation and within the application context.
- **Consider Performance:** Be mindful of potential performance overhead introduced by decorators.

### Limitations and Considerations

- **Performance Impact:** Decorators can introduce additional runtime overhead.
- **Experimental Feature:** As an experimental feature, decorators may change in future TypeScript versions.
- **Compatibility:** Ensure compatibility with JavaScript and future ECMAScript standards.
- **Minification Issues:** Decorators may not work well with certain minification or obfuscation tools.

### Handling Metadata and Parameters

Decorators can use the `Reflect` API to store and retrieve metadata. This is useful for passing parameters or configuration to decorators.

```typescript
import "reflect-metadata";

function myDecorator(options: any) {
  return function (target: any, propertyKey: string) {
    Reflect.defineMetadata("options", options, target, propertyKey);
  };
}

class MyService {
  @myDecorator({ role: "admin" })
  performAction() {
    // Action logic
  }
}
```

### Compatibility with JavaScript and ECMAScript

While decorators are a TypeScript feature, they are being considered for inclusion in future ECMAScript standards. This means they may eventually become a native feature of JavaScript.

### Experimenting with Built-In Decorators

Frameworks like Angular extensively use decorators for dependency injection and component configuration. Exploring these frameworks can provide practical insights into the power of decorators.

### Potential Issues with Minification

Decorators may rely on function names or property keys, which can be altered during minification. It's essential to test decorated code with minification tools to ensure compatibility.

### Testing Decorated Classes and Methods

Testing decorated classes involves verifying both the core logic and the behavior added by decorators. Use unit tests to isolate and test each aspect separately.

### Importance of Documentation

Given the abstract nature of decorators, thorough documentation is crucial. Comments and documentation should explain what each decorator does, its parameters, and its impact on the code.

### Conclusion

Decorators in TypeScript offer a powerful mechanism for enhancing and modifying code behavior in a declarative manner. By understanding their syntax, use cases, and best practices, developers can leverage decorators to write more maintainable and scalable applications. As decorators evolve and potentially become part of ECMAScript, they will continue to play a significant role in modern software development.

## Quiz Time!

{{< quizdown >}}

### What are decorators in TypeScript?

- [x] Special declarations that can modify classes, methods, properties, or parameters.
- [ ] Functions that only modify method behavior.
- [ ] A feature exclusive to JavaScript.
- [ ] A way to create new data types.

> **Explanation:** Decorators are special declarations in TypeScript that can modify classes, methods, properties, or parameters, providing additional behavior or metadata.

### What must be enabled in TypeScript to use decorators?

- [x] experimentalDecorators
- [ ] emitDecoratorMetadata
- [ ] allowJs
- [ ] strictNullChecks

> **Explanation:** The `experimentalDecorators` option must be enabled in TypeScript's `tsconfig.json` to use decorators.

### In what order are decorators applied?

- [x] Parameter, Method/Accessor/Property, Class
- [ ] Class, Method/Accessor/Property, Parameter
- [ ] Method/Accessor/Property, Parameter, Class
- [ ] Parameter, Class, Method/Accessor/Property

> **Explanation:** Decorators are applied in the order of Parameter, Method/Accessor/Property, and then Class.

### What is a common use case for decorators?

- [x] Logging
- [ ] Creating new data types
- [ ] Compiling TypeScript
- [ ] Styling components

> **Explanation:** Decorators are commonly used for logging, among other cross-cutting concerns.

### How can decorators affect performance?

- [x] They can introduce runtime overhead.
- [ ] They always optimize performance.
- [ ] They have no impact on performance.
- [ ] They only affect compile time.

> **Explanation:** Decorators can introduce runtime overhead, potentially affecting performance.

### What is a potential issue when using decorators with minification tools?

- [x] Alteration of function names or property keys
- [ ] Increased file size
- [ ] Loss of type information
- [ ] Compilation errors

> **Explanation:** Minification tools can alter function names or property keys, which may affect decorators.

### What is the role of the `Reflect` API in decorators?

- [x] To store and retrieve metadata
- [ ] To compile decorators
- [ ] To execute decorators
- [ ] To create new decorators

> **Explanation:** The `Reflect` API is used in decorators to store and retrieve metadata.

### Which TypeScript feature is necessary for decorators to work?

- [x] experimentalDecorators
- [ ] strictNullChecks
- [ ] allowSyntheticDefaultImports
- [ ] noImplicitAny

> **Explanation:** The `experimentalDecorators` compiler option is necessary for decorators to work in TypeScript.

### What should be documented when using decorators?

- [x] The behavior and impact of each decorator
- [ ] The TypeScript version used
- [ ] The JavaScript runtime
- [ ] The operating system

> **Explanation:** It's important to document the behavior and impact of each decorator to ensure clarity and maintainability.

### True or False: Decorators are a finalized feature in the ECMAScript standard.

- [ ] True
- [x] False

> **Explanation:** Decorators are currently an experimental feature in TypeScript and are not yet finalized in the ECMAScript standard.

{{< /quizdown >}}
