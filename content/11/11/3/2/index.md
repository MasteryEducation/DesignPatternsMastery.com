---
linkTitle: "11.3.2 Implementing Mocks and Stubs in JavaScript and TypeScript"
title: "Implementing Mocks and Stubs in JavaScript and TypeScript"
description: "Explore comprehensive techniques for implementing mocks and stubs in JavaScript and TypeScript using libraries like Sinon.js and Jest. Learn how to create robust, type-safe tests with examples and best practices."
categories:
- Software Development
- Testing
- JavaScript
- TypeScript
tags:
- Mocks
- Stubs
- Jest
- Sinon.js
- Test Doubles
date: 2024-10-25
type: docs
nav_weight: 1132000
---

## 11.3.2 Implementing Mocks and Stubs in JavaScript and TypeScript

In the realm of software testing, ensuring that your code behaves as expected across various scenarios is crucial. Mocks and stubs are essential tools in a developer's toolkit for creating isolated, reliable, and efficient tests. This section delves into the implementation of mocks and stubs in JavaScript and TypeScript, leveraging popular libraries like Sinon.js and Jest. We will explore how these tools can help simulate and control the behavior of complex systems, allowing for comprehensive testing without the need for live dependencies.

### Introduction to Mocks and Stubs

Before diving into implementation details, it's important to understand the fundamental concepts of mocks and stubs. Both are types of test doubles used to replace parts of the system under test:

- **Stubs**: These are used to provide predefined responses to method calls, helping to simulate specific conditions or behaviors. They are often used to replace functions or methods that return data.
  
- **Mocks**: Mocks are more complex than stubs and are used to verify interactions between objects. They can assert that certain methods were called with expected arguments, making them useful for testing side effects and interactions.

### Libraries for Creating Mocks and Stubs

Two popular libraries for creating mocks and stubs in JavaScript and TypeScript are Sinon.js and Jest. Each offers unique features and capabilities:

- **Sinon.js**: A standalone library that provides powerful features for creating spies, stubs, and mocks. It's highly flexible and can be integrated with any testing framework.
  
- **Jest**: A comprehensive testing framework that includes built-in mocking capabilities. Jest's `jest.fn()` and `jest.mock()` functions simplify the process of creating mocks and spies.

### Creating Stubs with Sinon.js

Let's start by creating a simple stub using Sinon.js. Suppose we have a function that fetches user data:

```javascript
// userService.js
export function fetchUserData(userId) {
  // Simulate an HTTP request to fetch user data
  return fetch(`https://api.example.com/users/${userId}`)
    .then(response => response.json());
}
```

To test this function without making an actual HTTP request, we can use a stub:

```javascript
import sinon from 'sinon';
import { fetchUserData } from './userService';

describe('fetchUserData', () => {
  it('should return predefined user data', async () => {
    const stub = sinon.stub(global, 'fetch');
    const mockResponse = { id: 1, name: 'John Doe' };
    stub.resolves(new Response(JSON.stringify(mockResponse)));

    const data = await fetchUserData(1);
    expect(data).toEqual(mockResponse);

    stub.restore();
  });
});
```

In this example, we use Sinon.js to stub the global `fetch` function, ensuring that it returns a predefined response. This allows us to test the `fetchUserData` function in isolation.

### Verifying Method Calls with Mocks

Mocks are useful for verifying that certain methods are called with expected arguments. Here's an example using Sinon.js:

```javascript
import sinon from 'sinon';

class Logger {
  log(message) {
    console.log(message);
  }
}

function processUser(user, logger) {
  if (user.isActive) {
    logger.log('User is active');
  }
}

describe('processUser', () => {
  it('should log a message for active users', () => {
    const user = { isActive: true };
    const logger = new Logger();
    const mock = sinon.mock(logger);

    mock.expects('log').once().withArgs('User is active');

    processUser(user, logger);

    mock.verify();
    mock.restore();
  });
});
```

In this test, we use a mock to verify that the `log` method is called once with the argument 'User is active'. This ensures that the `processUser` function behaves correctly.

### Manual vs. Automated Mocking

Manual mocking involves explicitly creating mocks and stubs for each test case, as shown in the examples above. While this approach offers fine-grained control, it can be time-consuming and error-prone for larger codebases.

Automated mocking, on the other hand, leverages tools like Jest's `jest.mock()` to automatically replace modules or functions with mocks. This approach simplifies the testing process and reduces boilerplate code.

### Mocking Modules and Functions in TypeScript

TypeScript's type system adds an extra layer of complexity when mocking modules and functions. Let's explore how to handle this:

```typescript
// mathUtils.ts
export function add(a: number, b: number): number {
  return a + b;
}
```

To mock this function in a test, we can use Jest:

```typescript
import { add } from './mathUtils';

jest.mock('./mathUtils', () => ({
  add: jest.fn(() => 3),
}));

describe('add function', () => {
  it('should return mocked value', () => {
    expect(add(1, 2)).toBe(3);
  });
});
```

Here, we use `jest.mock()` to replace the `add` function with a mock that always returns 3. This allows us to test code that depends on `add` without relying on its actual implementation.

### Handling Asynchronous Functions and Promises

Mocking asynchronous functions and promises requires careful handling to ensure tests run reliably. Here's an example using Jest:

```javascript
// asyncService.js
export function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('data');
    }, 1000);
  });
}
```

To mock this function, we can use Jest's `jest.fn()`:

```javascript
import { fetchData } from './asyncService';

jest.mock('./asyncService', () => ({
  fetchData: jest.fn(() => Promise.resolve('mocked data')),
}));

describe('fetchData', () => {
  it('should return mocked data', async () => {
    const data = await fetchData();
    expect(data).toBe('mocked data');
  });
});
```

By returning a promise that resolves immediately, we can test asynchronous code without waiting for real timeouts.

### Best Practices for Cleaning Up Mocks

To ensure tests remain isolated and independent, it's crucial to clean up and reset mocks between tests. Sinon.js and Jest provide utilities for this purpose:

- **Sinon.js**: Use `restore()` to reset stubs and mocks after each test.
- **Jest**: Use `jest.clearAllMocks()` or `jest.resetAllMocks()` to reset mocks.

### Mocking External Dependencies

Mocking external dependencies like HTTP requests or database calls is essential for testing in isolation. Libraries like `nock` can be used to intercept HTTP requests:

```javascript
import nock from 'nock';
import { fetchUserData } from './userService';

describe('fetchUserData', () => {
  it('should fetch user data', async () => {
    nock('https://api.example.com')
      .get('/users/1')
      .reply(200, { id: 1, name: 'John Doe' });

    const data = await fetchUserData(1);
    expect(data).toEqual({ id: 1, name: 'John Doe' });
  });
});
```

In this example, `nock` intercepts HTTP requests to the specified URL and returns a predefined response.

### Challenges with TypeScript's Type System

TypeScript's type system can pose challenges when creating mocks, particularly with interfaces and classes. To maintain type safety, consider using libraries like `ts-mockito`:

```typescript
import { mock, instance, when, verify } from 'ts-mockito';

interface UserService {
  getUser(id: number): Promise<{ id: number, name: string }>;
}

const mockedUserService: UserService = mock<UserService>();

when(mockedUserService.getUser(1)).thenResolve({ id: 1, name: 'John Doe' });

describe('UserService', () => {
  it('should return mocked user', async () => {
    const userService = instance(mockedUserService);
    const user = await userService.getUser(1);
    expect(user).toEqual({ id: 1, name: 'John Doe' });

    verify(mockedUserService.getUser(1)).once();
  });
});
```

This approach allows you to create type-safe mocks while leveraging TypeScript's interface capabilities.

### Maintaining Type Safety

When using mocks and stubs in TypeScript, it's important to maintain type safety to prevent runtime errors. Consider the following tips:

- Use TypeScript's type inference to automatically infer types for mocks.
- Leverage TypeScript's `Partial` and `Required` utility types to create flexible mocks.
- Use libraries like `ts-mockito` or `typemoq` for type-safe mocking.

### Writing Robust Tests

To ensure your tests remain robust as implementation details change, consider the following strategies:

- Focus on testing behavior and outcomes rather than implementation details.
- Avoid hardcoding values that may change frequently.
- Use dependency injection to facilitate easier mocking and stubbing.

### Dependency Injection for Easier Mocking

Dependency injection is a design pattern that promotes loose coupling and easier testing. By injecting dependencies into your code, you can easily replace them with mocks during testing:

```typescript
class UserService {
  constructor(private httpClient: HttpClient) {}

  getUser(id: number): Promise<{ id: number, name: string }> {
    return this.httpClient.get(`/users/${id}`);
  }
}

const mockHttpClient = {
  get: jest.fn(() => Promise.resolve({ id: 1, name: 'John Doe' })),
};

const userService = new UserService(mockHttpClient);
```

This approach allows you to test the `UserService` class without relying on a real HTTP client.

### Avoiding Tight Coupling

To avoid tight coupling between tests and code implementations, consider the following:

- Use interfaces and abstractions to decouple dependencies.
- Avoid testing private methods or internal logic directly.
- Focus on testing public APIs and expected behaviors.

### Documenting Mocks and Stubs

Documenting the behavior and expectations of mocks and stubs is crucial for maintaining clarity and understanding. Consider adding comments or documentation to describe:

- The purpose of each mock or stub.
- The expected behavior and interactions.
- Any assumptions or limitations.

### Continuous Review of Mocks and Stubs

Regularly review your mocks and stubs to ensure they reflect current dependencies and behaviors. This helps prevent outdated or incorrect tests from causing issues.

### Conclusion

Mocks and stubs are invaluable tools for creating isolated, reliable, and efficient tests in JavaScript and TypeScript. By leveraging libraries like Sinon.js and Jest, you can simulate and control the behavior of complex systems, allowing for comprehensive testing without the need for live dependencies. By following best practices and maintaining type safety, you can ensure your tests remain robust and effective as your code evolves.

## Quiz Time!

{{< quizdown >}}

### What is the primary purpose of a stub in testing?

- [x] To provide predefined responses to method calls
- [ ] To verify interactions between objects
- [ ] To replace entire modules with mock implementations
- [ ] To simulate user interactions

> **Explanation:** Stubs are used to provide predefined responses to method calls, helping to simulate specific conditions or behaviors in tests.

### Which library provides built-in mocking capabilities for JavaScript testing?

- [ ] Sinon.js
- [x] Jest
- [ ] Mocha
- [ ] Jasmine

> **Explanation:** Jest is a comprehensive testing framework that includes built-in mocking capabilities, such as `jest.fn()` and `jest.mock()`.

### How can you reset mocks between tests in Jest?

- [x] Use `jest.clearAllMocks()` or `jest.resetAllMocks()`
- [ ] Use `jest.restoreAllMocks()`
- [ ] Use `jest.removeAllMocks()`
- [ ] Use `jest.resetMocks()`

> **Explanation:** Jest provides `jest.clearAllMocks()` and `jest.resetAllMocks()` to reset mocks between tests, ensuring isolation and independence.

### What is a common challenge when creating mocks in TypeScript?

- [ ] Lack of mocking libraries
- [x] Maintaining type safety
- [ ] Limited support for asynchronous functions
- [ ] Inability to mock classes

> **Explanation:** Maintaining type safety is a common challenge when creating mocks in TypeScript, especially when dealing with interfaces and classes.

### Which utility type in TypeScript can help create flexible mocks?

- [ ] `Readonly`
- [x] `Partial`
- [ ] `Nullable`
- [ ] `Record`

> **Explanation:** The `Partial` utility type in TypeScript allows you to create flexible mocks by making all properties of a type optional.

### What is the benefit of using dependency injection for testing?

- [ ] It increases the complexity of the code
- [x] It promotes loose coupling and easier testing
- [ ] It eliminates the need for mocks and stubs
- [ ] It allows for testing private methods directly

> **Explanation:** Dependency injection promotes loose coupling and easier testing by allowing dependencies to be easily replaced with mocks during testing.

### How can you mock an HTTP request in a test?

- [ ] Use `jest.fn()` to replace the HTTP module
- [x] Use a library like `nock` to intercept requests
- [ ] Use `sinon.spy()` to observe requests
- [ ] Use `jest.mock()` to replace the HTTP function

> **Explanation:** Libraries like `nock` can be used to intercept HTTP requests and provide predefined responses, allowing for isolated testing.

### What should you focus on when writing robust tests?

- [x] Testing behavior and outcomes
- [ ] Testing private methods
- [ ] Hardcoding values
- [ ] Testing internal logic directly

> **Explanation:** When writing robust tests, focus on testing behavior and outcomes rather than implementation details, to ensure tests remain reliable as code changes.

### Which Jest function is used to create a mock function?

- [ ] `jest.spy()`
- [ ] `jest.createMock()`
- [ ] `jest.stub()`
- [x] `jest.fn()`

> **Explanation:** `jest.fn()` is used to create a mock function in Jest, allowing you to simulate and control function behavior in tests.

### True or False: Mocks can be used to verify method calls and their arguments.

- [x] True
- [ ] False

> **Explanation:** True. Mocks can be used to verify that certain methods are called with expected arguments, making them useful for testing interactions and side effects.

{{< /quizdown >}}
