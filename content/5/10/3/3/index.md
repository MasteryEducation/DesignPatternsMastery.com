---
linkTitle: "10.3.3 Creating Custom Decorators"
title: "Creating Custom Decorators in TypeScript: A Comprehensive Guide"
description: "Explore the creation of custom decorators in TypeScript, including class, method, and parameter decorators. Learn to enhance functionality, maintain type safety, and adhere to best practices."
categories:
- TypeScript
- Metaprogramming
- Software Design
tags:
- TypeScript
- Decorators
- Metaprogramming
- Custom Decorators
- Design Patterns
date: 2024-10-25
type: docs
nav_weight: 1033000
---

## 10.3.3 Creating Custom Decorators

Decorators in TypeScript provide a powerful way to add annotations and a meta-programming syntax for class declarations and members. They allow developers to modify classes, methods, accessors, properties, or parameters. This section will guide you through creating custom decorators, explaining their types, and demonstrating practical examples.

### Understanding Decorators

Decorators are a stage 2 proposal for JavaScript that TypeScript has implemented. They are functions that provide a way to add annotations and meta-programming syntax to class declarations and members. Here's a quick overview of the different types of decorators:

- **Class Decorators**: Applied to class constructors.
- **Method Decorators**: Applied to methods.
- **Accessor Decorators**: Applied to accessors.
- **Property Decorators**: Applied to properties.
- **Parameter Decorators**: Applied to method parameters.

### Creating a Simple Class Decorator

A class decorator is a function that takes a class constructor as its only argument. It can be used to modify or augment the class.

```typescript
function SimpleLogger(constructor: Function) {
    console.log(`Class ${constructor.name} is being created.`);
}

@SimpleLogger
class ExampleClass {
    constructor() {
        console.log("ExampleClass instance created.");
    }
}

const instance = new ExampleClass();
```

**Explanation**: The `SimpleLogger` decorator logs a message when the `ExampleClass` is defined. When you instantiate `ExampleClass`, it will log both the decorator message and the constructor message.

### Creating Method Decorators

Method decorators allow you to intercept and modify method behavior. They receive three arguments: the target (class prototype), the method name, and the property descriptor.

```typescript
function LogExecutionTime(target: Object, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function (...args: any[]) {
        console.time(propertyKey);
        const result = originalMethod.apply(this, args);
        console.timeEnd(propertyKey);
        return result;
    };
}

class Calculator {
    @LogExecutionTime
    add(a: number, b: number): number {
        return a + b;
    }
}

const calculator = new Calculator();
calculator.add(5, 10);
```

**Explanation**: The `LogExecutionTime` decorator measures and logs the execution time of the `add` method. It wraps the original method, using `console.time` and `console.timeEnd` to measure the time taken.

### Parameterizing Decorators with Decorator Factories

Decorator factories allow you to pass parameters to decorators. They are functions that return a decorator function.

```typescript
function LogMethod(message: string) {
    return function (target: Object, propertyKey: string, descriptor: PropertyDescriptor) {
        const originalMethod = descriptor.value;
        descriptor.value = function (...args: any[]) {
            console.log(`${message} - Method ${propertyKey} called with args: ${JSON.stringify(args)}`);
            return originalMethod.apply(this, args);
        };
    };
}

class Greeter {
    @LogMethod("Greeting")
    greet(name: string): string {
        return `Hello, ${name}!`;
    }
}

const greeter = new Greeter();
greeter.greet("World");
```

**Explanation**: The `LogMethod` decorator factory takes a `message` parameter and logs it along with method calls. This demonstrates how to create configurable decorators using closures.

### Handling `this` Context in Decorators

Maintaining the correct `this` context within decorators is crucial. Using `Function.prototype.apply` or `Function.prototype.call` ensures the correct context is preserved.

```typescript
function Bind(target: Object, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    descriptor.value = function (...args: any[]) {
        return originalMethod.apply(this, args);
    };
}

class Person {
    constructor(public name: string) {}

    @Bind
    sayHello() {
        console.log(`Hello, my name is ${this.name}`);
    }
}

const person = new Person("Alice");
const greet = person.sayHello;
greet(); // Correctly logs "Hello, my name is Alice"
```

**Explanation**: The `Bind` decorator ensures that the method `sayHello` maintains the correct `this` context even when it's called as a standalone function.

### Composing Multiple Decorators

Decorators can be composed, and their order of execution is from bottom to top (or right to left).

```typescript
function First() {
    return function (target: Object, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("First decorator");
    };
}

function Second() {
    return function (target: Object, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("Second decorator");
    };
}

class Demo {
    @First()
    @Second()
    method() {}
}

const demo = new Demo();
demo.method();
```

**Explanation**: The `Second` decorator logs first because it is applied last. Understanding this order is crucial for composing decorators effectively.

### Best Practices for Creating Decorators

- **Single Responsibility**: Ensure each decorator does one thing well.
- **Error Handling**: Implement robust error handling within decorators.
- **Type Safety**: Use TypeScript's type system to ensure decorators are type-safe.
- **Documentation**: Clearly document what each decorator does and its intended use.
- **Testing**: Write unit tests for decorators to verify their behavior.

### Common Pitfalls and How to Avoid Them

- **Altering Method Signatures**: Be cautious when modifying method behavior to avoid unintended changes to method signatures.
- **Execution Context**: Always ensure the correct `this` context is maintained.
- **Order of Execution**: Remember that decorators are applied in reverse order.

### Real-World Examples and Use Cases

1. **Logging**: Automatically log method calls and parameters.
2. **Access Control**: Restrict access to certain methods based on user roles.
3. **Performance Monitoring**: Measure execution time and performance metrics.

### Testing and Configuring Decorators

Testing decorators involves ensuring they modify behavior as expected. Use a testing framework like Jest or Mocha to write tests. Configurable decorators should expose options that can be easily adjusted.

### Conclusion

Custom decorators in TypeScript offer a powerful way to enhance and modify class behavior. By following best practices and understanding their mechanics, you can create reusable, maintainable decorators that improve your codebase's functionality and readability. Always ensure decorators are well-documented, thoroughly tested, and used judiciously to maintain code clarity and performance.

## Quiz Time!

{{< quizdown >}}

### What is a decorator in TypeScript?

- [x] A function that adds annotations and meta-programming syntax to class declarations and members
- [ ] A built-in TypeScript function for handling asynchronous operations
- [ ] A method for defining class inheritance
- [ ] A way to compile TypeScript into JavaScript

> **Explanation:** Decorators are functions that add annotations and meta-programming syntax to class declarations and members in TypeScript.

### Which decorator type is used to modify class constructors?

- [x] Class Decorator
- [ ] Method Decorator
- [ ] Property Decorator
- [ ] Parameter Decorator

> **Explanation:** Class decorators are used to modify class constructors.

### What arguments does a method decorator receive?

- [x] Target, propertyKey, and descriptor
- [ ] Class name, method name, and return type
- [ ] Function name, arguments, and return value
- [ ] Parameter types and return type

> **Explanation:** A method decorator receives the target (class prototype), the property key (method name), and the descriptor.

### How can you pass parameters to a decorator?

- [x] Using a decorator factory
- [ ] By defining a global variable
- [ ] Through a constructor
- [ ] By using a configuration file

> **Explanation:** Decorator factories allow you to pass parameters to decorators by returning a decorator function.

### What is the order of execution for multiple decorators?

- [x] From bottom to top
- [ ] From top to bottom
- [ ] In alphabetical order
- [ ] In random order

> **Explanation:** Multiple decorators are executed from bottom to top (or right to left).

### How can you maintain the correct `this` context in a method decorator?

- [x] Using `Function.prototype.apply` or `Function.prototype.call`
- [ ] By using an arrow function
- [ ] By using a global variable
- [ ] By defining a new context object

> **Explanation:** Using `Function.prototype.apply` or `Function.prototype.call` ensures the correct `this` context is maintained.

### Why is it important to test decorators?

- [x] To ensure they modify behavior as expected
- [ ] To increase code complexity
- [ ] To make the code less readable
- [ ] To avoid using TypeScript features

> **Explanation:** Testing decorators ensures they modify behavior as expected and maintain code reliability.

### What should you be cautious of when modifying method behavior in decorators?

- [x] Altering method signatures unintentionally
- [ ] Increasing the method's return value
- [ ] Decreasing the method's execution time
- [ ] Changing the method's visibility

> **Explanation:** Be cautious of unintentionally altering method signatures when modifying method behavior in decorators.

### Which principle should be adhered to when designing decorators?

- [x] Single Responsibility Principle
- [ ] Open/Closed Principle
- [ ] Liskov Substitution Principle
- [ ] Interface Segregation Principle

> **Explanation:** Adhering to the Single Responsibility Principle ensures each decorator does one thing well.

### True or False: Decorators can only be used with classes in TypeScript.

- [ ] True
- [x] False

> **Explanation:** Decorators can be used with classes, methods, accessors, properties, and parameters in TypeScript.

{{< /quizdown >}}
