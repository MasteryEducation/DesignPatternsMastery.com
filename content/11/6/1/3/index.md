---
linkTitle: "6.1.3 Chain of Responsibility Pattern in TypeScript"
title: "Chain of Responsibility Pattern in TypeScript: A Comprehensive Guide"
description: "Explore the Chain of Responsibility Pattern in TypeScript with type-safe implementations, asynchronous handling, and best practices for server applications."
categories:
- Design Patterns
- TypeScript
- Software Development
tags:
- Chain of Responsibility
- Behavioral Patterns
- TypeScript
- Design Patterns
- Software Architecture
date: 2024-10-25
type: docs
nav_weight: 613000
---

## 6.1.3 Chain of Responsibility Pattern in TypeScript

The Chain of Responsibility pattern is a behavioral design pattern that allows an object to pass a request along a chain of potential handlers until the request is handled. This pattern decouples the sender of a request from its receiver, allowing multiple objects to handle the request without the sender needing to know which object will ultimately process it.

In this section, we will delve into the Chain of Responsibility pattern using TypeScript, focusing on type safety, asynchronous handling, and practical applications in server-side development. We will explore how TypeScript's features, such as interfaces, generics, and access modifiers, can enhance the implementation of this pattern.

### Understanding the Chain of Responsibility Pattern

Before diving into TypeScript-specific implementations, let's briefly recap the core components of the Chain of Responsibility pattern:

- **Handler**: An interface or abstract class that defines a method for handling requests. Each handler in the chain has a reference to the next handler.
- **Concrete Handler**: Implements the handler interface and processes requests it is responsible for. If it cannot handle a request, it forwards the request to the next handler.
- **Client**: Initiates the request and sets up the chain of handlers.

The pattern is particularly useful in scenarios where multiple handlers can process a request, such as in logging frameworks, event handling systems, or middleware chains in web applications.

### Defining the Handler Interface in TypeScript

In TypeScript, we can define a generic `Handler` interface to ensure type safety across different request and response types. This approach allows handlers to process specific types of requests, enhancing the flexibility and reusability of the pattern.

```typescript
interface Handler<TRequest, TResponse> {
  setNext(handler: Handler<TRequest, TResponse>): Handler<TRequest, TResponse>;
  handle(request: TRequest): TResponse | null;
}
```

- **Generics**: The `TRequest` and `TResponse` generics allow handlers to specify the types of requests they can handle and the types of responses they produce.
- **setNext**: This method sets the next handler in the chain and returns the handler itself for method chaining.
- **handle**: This method attempts to process the request and returns a response or `null` if it cannot handle the request.

### Implementing Concrete Handlers

Concrete handlers implement the `Handler` interface, providing specific logic for processing requests. Let's consider a simple example where we have handlers for different logging levels: `InfoHandler`, `WarningHandler`, and `ErrorHandler`.

```typescript
class InfoHandler implements Handler<string, void> {
  private nextHandler: Handler<string, void> | null = null;

  setNext(handler: Handler<string, void>): Handler<string, void> {
    this.nextHandler = handler;
    return handler;
  }

  handle(request: string): void | null {
    if (request === "info") {
      console.log("InfoHandler: Handling info request.");
      return;
    }
    return this.nextHandler ? this.nextHandler.handle(request) : null;
  }
}

class WarningHandler implements Handler<string, void> {
  private nextHandler: Handler<string, void> | null = null;

  setNext(handler: Handler<string, void>): Handler<string, void> {
    this.nextHandler = handler;
    return handler;
  }

  handle(request: string): void | null {
    if (request === "warning") {
      console.log("WarningHandler: Handling warning request.");
      return;
    }
    return this.nextHandler ? this.nextHandler.handle(request) : null;
  }
}

class ErrorHandler implements Handler<string, void> {
  private nextHandler: Handler<string, void> | null = null;

  setNext(handler: Handler<string, void>): Handler<string, void> {
    this.nextHandler = handler;
    return handler;
  }

  handle(request: string): void | null {
    if (request === "error") {
      console.log("ErrorHandler: Handling error request.");
      return;
    }
    return this.nextHandler ? this.nextHandler.handle(request) : null;
  }
}
```

- **Concrete Handlers**: Each handler checks if it can process the request. If it can, it handles the request; otherwise, it passes the request to the next handler in the chain.
- **Type Safety**: TypeScript ensures that each handler processes requests of the correct type, reducing runtime errors.

### Setting Up the Chain

To set up the chain, we create instances of the handlers and link them together using the `setNext` method.

```typescript
const infoHandler = new InfoHandler();
const warningHandler = new WarningHandler();
const errorHandler = new ErrorHandler();

infoHandler.setNext(warningHandler).setNext(errorHandler);
```

- **Method Chaining**: The `setNext` method returns the handler itself, allowing us to chain method calls and set up the chain in a concise manner.

### Handling Requests of Different Types

In scenarios where handlers need to process requests of different types, we can use TypeScript's union types or generics to accommodate this flexibility.

```typescript
type RequestType = "info" | "warning" | "error";

class FlexibleHandler implements Handler<RequestType, void> {
  private nextHandler: Handler<RequestType, void> | null = null;

  setNext(handler: Handler<RequestType, void>): Handler<RequestType, void> {
    this.nextHandler = handler;
    return handler;
  }

  handle(request: RequestType): void | null {
    if (request === "info") {
      console.log("FlexibleHandler: Handling info request.");
    } else if (request === "warning") {
      console.log("FlexibleHandler: Handling warning request.");
    } else if (request === "error") {
      console.log("FlexibleHandler: Handling error request.");
    } else {
      return this.nextHandler ? this.nextHandler.handle(request) : null;
    }
  }
}
```

- **Union Types**: The `RequestType` union type allows the handler to process multiple request types, enhancing flexibility.

### Asynchronous Chains with Promises and Async/Await

In modern applications, handling requests asynchronously is crucial, especially when dealing with I/O operations or network requests. We can adapt the Chain of Responsibility pattern to support asynchronous processing using Promises or async/await.

```typescript
interface AsyncHandler<TRequest, TResponse> {
  setNext(handler: AsyncHandler<TRequest, TResponse>): AsyncHandler<TRequest, TResponse>;
  handle(request: TRequest): Promise<TResponse | null>;
}

class AsyncInfoHandler implements AsyncHandler<string, void> {
  private nextHandler: AsyncHandler<string, void> | null = null;

  setNext(handler: AsyncHandler<string, void>): AsyncHandler<string, void> {
    this.nextHandler = handler;
    return handler;
  }

  async handle(request: string): Promise<void | null> {
    if (request === "info") {
      console.log("AsyncInfoHandler: Handling info request.");
      return;
    }
    return this.nextHandler ? this.nextHandler.handle(request) : Promise.resolve(null);
  }
}
```

- **AsyncHandler Interface**: The `handle` method returns a `Promise`, allowing handlers to perform asynchronous operations.
- **Async/Await**: We can use `async/await` syntax within the `handle` method to simplify asynchronous code.

### Managing Exceptions and Error Propagation

When implementing the Chain of Responsibility pattern, it's essential to handle exceptions gracefully and ensure they propagate correctly through the chain. TypeScript's try/catch blocks can be used within the `handle` method to manage exceptions.

```typescript
class ErrorHandlingHandler implements Handler<string, void> {
  private nextHandler: Handler<string, void> | null = null;

  setNext(handler: Handler<string, void>): Handler<string, void> {
    this.nextHandler = handler;
    return handler;
  }

  handle(request: string): void | null {
    try {
      if (request === "error") {
        throw new Error("ErrorHandlingHandler: Error occurred.");
      }
      return this.nextHandler ? this.nextHandler.handle(request) : null;
    } catch (error) {
      console.error("ErrorHandlingHandler: Caught exception:", error);
      return null;
    }
  }
}
```

- **Exception Handling**: Use try/catch blocks to manage exceptions and log errors. Ensure that exceptions do not disrupt the flow of the chain.

### Encapsulation with Access Modifiers

Access modifiers in TypeScript, such as `private` and `protected`, can be used to encapsulate handler internals and control access to handler methods and properties.

```typescript
class SecureHandler implements Handler<string, void> {
  private nextHandler: Handler<string, void> | null = null;

  setNext(handler: Handler<string, void>): Handler<string, void> {
    this.nextHandler = handler;
    return handler;
  }

  handle(request: string): void | null {
    if (this.canHandle(request)) {
      console.log("SecureHandler: Handling request.");
      return;
    }
    return this.nextHandler ? this.nextHandler.handle(request) : null;
  }

  private canHandle(request: string): boolean {
    return request === "secure";
  }
}
```

- **Private Methods**: Use private methods to encapsulate logic that should not be exposed to clients.

### Documenting Handler Roles and Interfaces

Clear documentation is crucial for maintaining and understanding the Chain of Responsibility pattern, especially in complex systems. Each handler should have well-documented roles, expected inputs, and outputs.

```typescript
/**
 * InfoHandler processes "info" requests.
 * @implements {Handler<string, void>}
 */
class InfoHandler implements Handler<string, void> {
  // Implementation details...
}
```

- **JSDoc Comments**: Use JSDoc comments to document handler classes and methods, providing clear descriptions of their roles and expected behavior.

### Integrating the Pattern into Server Applications

The Chain of Responsibility pattern is well-suited for server applications, such as middleware in Express.js or request processing pipelines in NestJS. Here's an example of integrating the pattern into an Express.js application.

```typescript
import express from 'express';

const app = express();

class MiddlewareHandler implements Handler<express.Request, express.Response> {
  private nextHandler: Handler<express.Request, express.Response> | null = null;

  setNext(handler: Handler<express.Request, express.Response>): Handler<express.Request, express.Response> {
    this.nextHandler = handler;
    return handler;
  }

  handle(req: express.Request, res: express.Response): void {
    console.log("MiddlewareHandler: Processing request.");
    if (this.nextHandler) {
      this.nextHandler.handle(req, res);
    } else {
      res.send("Request processed.");
    }
  }
}

const middlewareHandler = new MiddlewareHandler();
app.use((req, res) => middlewareHandler.handle(req, res));

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

- **Express.js Integration**: Use the pattern to create middleware handlers that process requests in a chain, enhancing modularity and code reuse.

### Best Practices for Testing Handlers

Testing handlers in TypeScript is facilitated by its strong type-checking capabilities. Here are some best practices for testing handlers:

- **Unit Testing**: Test each handler independently to ensure it processes requests correctly and forwards them when necessary.
- **Mocking**: Use mocking libraries to simulate dependencies and isolate handlers during testing.
- **Type Assertions**: Leverage TypeScript's type assertions to verify that handlers receive and return the correct types.

```typescript
import { expect } from 'chai';
import { spy } from 'sinon';

describe('InfoHandler', () => {
  it('should handle "info" requests', () => {
    const handler = new InfoHandler();
    const consoleSpy = spy(console, 'log');

    handler.handle('info');

    expect(consoleSpy.calledWith('InfoHandler: Handling info request.')).to.be.true;
    consoleSpy.restore();
  });
});
```

### Addressing Circular Dependencies

Circular dependencies can occur when handlers depend on each other in a way that creates a loop. To mitigate this issue:

- **Design Carefully**: Ensure that handlers are designed to process requests independently and do not rely on each other excessively.
- **Dependency Injection**: Use dependency injection to manage dependencies and avoid circular references.

### Conclusion

The Chain of Responsibility pattern is a powerful tool for building flexible and modular applications in TypeScript. By leveraging TypeScript's features, such as interfaces, generics, and access modifiers, we can create robust and type-safe implementations of this pattern. Whether handling synchronous or asynchronous requests, the pattern enhances code organization and reusability.

By following best practices for testing and documentation, developers can ensure that their implementations are maintainable and scalable. With careful design, the Chain of Responsibility pattern can be a valuable asset in any TypeScript-based application.

## Quiz Time!

{{< quizdown >}}

### What is the primary purpose of the Chain of Responsibility pattern?

- [x] To decouple the sender of a request from its receiver
- [ ] To ensure a single handler processes all requests
- [ ] To provide a direct connection between request sender and receiver
- [ ] To prioritize requests based on their type

> **Explanation:** The Chain of Responsibility pattern allows multiple objects to handle a request without the sender knowing which object will process it, thus decoupling the sender from the receiver.

### Which TypeScript feature enhances the flexibility of handlers in the Chain of Responsibility pattern?

- [x] Generics
- [ ] Enums
- [ ] Decorators
- [ ] Namespaces

> **Explanation:** Generics allow handlers to specify the types of requests they can handle and the types of responses they produce, enhancing flexibility.

### How can handlers in a chain be linked together in TypeScript?

- [ ] Using arrays
- [x] Using the `setNext` method
- [ ] Using promises
- [ ] Using async/await

> **Explanation:** The `setNext` method is used to link handlers together, forming a chain where each handler can pass the request to the next.

### What is a common method to handle asynchronous requests in a Chain of Responsibility implementation?

- [ ] Using callbacks
- [ ] Using synchronous code
- [x] Using Promises or async/await
- [ ] Using event listeners

> **Explanation:** Promises or async/await are commonly used to handle asynchronous requests in a Chain of Responsibility implementation.

### Which access modifier in TypeScript helps encapsulate handler internals?

- [ ] public
- [ ] readonly
- [x] private
- [ ] static

> **Explanation:** The `private` access modifier is used to encapsulate handler internals, controlling access to methods and properties.

### What is a potential issue when handlers in a chain depend on each other excessively?

- [ ] Increased performance
- [x] Circular dependencies
- [ ] Simplified code
- [ ] Enhanced security

> **Explanation:** Excessive dependencies between handlers can lead to circular dependencies, which can cause issues in the application.

### How can TypeScript's type-checking capabilities be used in testing handlers?

- [ ] By ignoring type errors
- [x] By using type assertions
- [ ] By disabling type-checking
- [ ] By using any type

> **Explanation:** Type assertions can be used to verify that handlers receive and return the correct types during testing.

### What is a common use case for the Chain of Responsibility pattern in server applications?

- [ ] Direct database access
- [ ] Rendering user interfaces
- [x] Middleware processing
- [ ] Static file serving

> **Explanation:** The Chain of Responsibility pattern is commonly used in middleware processing, where requests are handled by a chain of middleware functions.

### How can circular dependencies in handler implementations be mitigated?

- [ ] By using global variables
- [x] By using dependency injection
- [ ] By increasing the number of handlers
- [ ] By ignoring the issue

> **Explanation:** Dependency injection can help manage dependencies and avoid circular references in handler implementations.

### True or False: The Chain of Responsibility pattern always requires asynchronous handling.

- [ ] True
- [x] False

> **Explanation:** The Chain of Responsibility pattern can handle both synchronous and asynchronous requests, depending on the application's needs.

{{< /quizdown >}}
