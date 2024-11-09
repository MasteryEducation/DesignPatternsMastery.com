---
linkTitle: "10.1.1 Defining an Algorithm's Skeleton"
title: "Template Method Pattern: Defining an Algorithm's Skeleton"
description: "Explore the Template Method Pattern, a powerful design pattern that defines the skeleton of an algorithm, allowing subclasses to customize specific steps while maintaining the overall structure."
categories:
- Software Design
- Behavioral Patterns
- Software Architecture
tags:
- Template Method Pattern
- Design Patterns
- Algorithm Structure
- Code Reuse
- Software Engineering
date: 2024-10-25
type: docs
nav_weight: 1011000
---

## 10.1.1 Defining an Algorithm's Skeleton

In the realm of software architecture, the Template Method pattern stands out as a quintessential example of how design patterns can elegantly solve common problems. This behavioral pattern is all about defining the skeleton of an algorithm within a method, while allowing subclasses the flexibility to redefine specific steps without altering the overall algorithm's structure. Let's delve into the intricacies of the Template Method pattern, exploring its components, benefits, and practical applications.

### Understanding the Template Method Pattern

At its core, the Template Method pattern is a strategy for defining the outline of an algorithm in a base class, known as the abstract class, while deferring the implementation of certain steps to subclasses. This approach ensures that the overarching structure of the algorithm remains consistent, while allowing for customization where needed.

#### Key Components

1. **Abstract Class**: This is where the template method resides. The template method outlines the algorithm's skeleton, calling various abstract methods that subclasses will implement.

2. **Template Method**: This method defines the sequence of steps in the algorithm. It is typically a `final` method, meaning it cannot be overridden by subclasses, ensuring the algorithm's structure remains intact.

3. **Concrete Subclasses**: These subclasses inherit from the abstract class and provide specific implementations for the abstract methods defined in the template method.

### How It Works: A Simple Example

Imagine a scenario where we need to process data from different sources, such as files, databases, or web services. The overall steps for processing might include opening the data source, reading the data, processing the data, and finally closing the data source. The Template Method pattern allows us to define this skeleton in an abstract class, while concrete subclasses handle the specifics of each step for different data sources.

```java
abstract class DataProcessor {
    // Template method
    public final void process() {
        openSource();
        readData();
        processData();
        closeSource();
    }

    abstract void openSource();
    abstract void readData();
    abstract void processData();
    abstract void closeSource();
}

class FileDataProcessor extends DataProcessor {
    void openSource() { /* Open file logic */ }
    void readData() { /* Read file data */ }
    void processData() { /* Process file data */ }
    void closeSource() { /* Close file */ }
}

class DatabaseDataProcessor extends DataProcessor {
    void openSource() { /* Connect to database */ }
    void readData() { /* Fetch data from database */ }
    void processData() { /* Process database data */ }
    void closeSource() { /* Disconnect database */ }
}
```

### Promoting Code Reuse

The Template Method pattern promotes code reuse by encapsulating the invariant parts of the algorithm in the abstract class. This means that any changes to the algorithm's structure only need to be made in one place, rather than across multiple subclasses. This encapsulation also adheres to the **Hollywood Principle**: "Don't call us; we'll call you." The abstract class controls the algorithm's flow, calling the necessary methods in subclasses as needed.

### Supporting the Open/Closed Principle

One of the significant advantages of the Template Method pattern is its alignment with the **Open/Closed Principle**. This principle states that software entities should be open for extension but closed for modification. By allowing subclasses to implement specific steps, the pattern enables extensions without requiring changes to the algorithm's structure.

### Practical Applications

The Template Method pattern finds applications in various domains:

- **Game AI**: Defining a sequence of moves or actions for a game character, where specific actions vary based on the character type.
- **Network Protocols**: Implementing protocols where the sequence of operations is fixed, but specific actions (like authentication) can vary.
- **Data Processing**: As demonstrated earlier, processing data from different sources while maintaining a consistent workflow.

### Diagram Illustration

Here's a simple UML diagram illustrating the Template Method pattern:

```
+------------------+
|  DataProcessor   |  <--- Abstract Class
+------------------+
| - process()      |  <--- Template Method
| - openSource()   |  <--- Abstract Method
| - readData()     |
| - processData()  |
| - closeSource()  |
+------------------+
         |
         | Inheritance
         |
+--------------------+        +----------------------+
| FileDataProcessor  |        | DatabaseDataProcessor |
+--------------------+        +----------------------+
| + openSource()     |        | + openSource()       |
| + readData()       |        | + readData()         |
| + processData()    |        | + processData()      |
| + closeSource()    |        | + closeSource()      |
+--------------------+        +----------------------+
```

### Method Naming and Hooks

Using clear and descriptive method names is crucial for conveying the algorithm's steps. Additionally, the pattern can include **hooks**—optional methods that subclasses can override to provide additional behavior or default implementations.

### Challenges and Considerations

While the Template Method pattern offers many benefits, it can also introduce rigidity. The fixed structure of the algorithm means that significant changes to the process may require altering the abstract class, potentially affecting all subclasses. Therefore, it's essential to carefully design the template method to balance flexibility and stability.

### When to Consider the Template Method Pattern

Consider using the Template Method pattern when you have a consistent process with steps that can be customized. It is particularly useful when you want to enforce a specific sequence of operations while allowing flexibility in certain parts.

### Conclusion

The Template Method pattern is a powerful tool in the software architect's toolkit, providing a balance between consistency and customization. By defining an algorithm's skeleton in an abstract class, it promotes code reuse, supports the Open/Closed Principle, and adheres to the Hollywood Principle. With its wide range of applications, the Template Method pattern is a go-to solution for scenarios requiring a structured yet flexible approach.

## Quiz Time!

{{< quizdown >}}

### What is the primary purpose of the Template Method pattern?

- [x] To define the skeleton of an algorithm and allow subclasses to customize specific steps
- [ ] To provide a way to create objects without specifying the exact class
- [ ] To ensure a class has only one instance
- [ ] To separate the construction of a complex object from its representation

> **Explanation:** The Template Method pattern defines the skeleton of an algorithm in a method, allowing subclasses to redefine certain steps without changing the algorithm's structure.


### Which principle does the Template Method pattern adhere to?

- [x] Hollywood Principle: "Don't call us; we'll call you"
- [ ] Single Responsibility Principle
- [ ] Liskov Substitution Principle
- [ ] Dependency Inversion Principle

> **Explanation:** The Template Method pattern adheres to the Hollywood Principle, where the base class controls the flow of the algorithm and calls the necessary methods in subclasses.


### What is a key component of the Template Method pattern?

- [x] Abstract Class
- [ ] Singleton Object
- [ ] Factory Method
- [ ] Proxy Interface

> **Explanation:** The Template Method pattern involves an abstract class that contains the template method and defines the skeleton of the algorithm.


### How does the Template Method pattern support the Open/Closed Principle?

- [x] By allowing extensions without modifications to the algorithm's structure
- [ ] By creating a single instance of a class
- [ ] By using interfaces to define contracts
- [ ] By separating object construction from its representation

> **Explanation:** The Template Method pattern supports the Open/Closed Principle by allowing subclasses to implement specific steps, enabling extensions without modifying the algorithm's structure.


### What is a practical application of the Template Method pattern?

- [x] Implementing game AI with customizable actions
- [ ] Creating a single instance of a database connection
- [ ] Separating the construction of a complex object from its representation
- [ ] Providing a unified interface to a set of interfaces

> **Explanation:** The Template Method pattern is useful in scenarios like game AI, where the sequence of actions is fixed, but specific actions can vary based on the character type.


### What is the role of hooks in the Template Method pattern?

- [x] To provide optional or default behaviors
- [ ] To enforce a single instance of a class
- [ ] To create objects without specifying the exact class
- [ ] To separate the construction of a complex object from its representation

> **Explanation:** Hooks in the Template Method pattern are optional methods that subclasses can override to provide additional behavior or default implementations.


### What challenge can arise with the Template Method pattern?

- [x] Rigidity of the algorithm's structure
- [ ] Lack of a single instance
- [ ] Complexity in separating construction from representation
- [ ] Difficulty in providing a unified interface

> **Explanation:** The Template Method pattern can introduce rigidity because the fixed structure of the algorithm means significant changes may require altering the abstract class.


### When should you consider using the Template Method pattern?

- [x] When you have a consistent process with steps that can be customized
- [ ] When you need a single instance of a class
- [ ] When you need to separate object construction from its representation
- [ ] When you need to provide a unified interface to a set of interfaces

> **Explanation:** The Template Method pattern is ideal for scenarios requiring a consistent process with customizable steps, such as data processing or game AI.


### What is the template method in the Template Method pattern?

- [x] A method that defines the sequence of steps in an algorithm
- [ ] A method that creates objects without specifying the exact class
- [ ] A method that ensures a class has only one instance
- [ ] A method that separates the construction of a complex object from its representation

> **Explanation:** The template method is a method in the abstract class that defines the sequence of steps in the algorithm, calling various abstract methods.


### True or False: The Template Method pattern promotes code reuse by encapsulating the invariant parts of an algorithm.

- [x] True
- [ ] False

> **Explanation:** True. The Template Method pattern promotes code reuse by encapsulating the invariant parts of an algorithm in the abstract class, allowing subclasses to implement specific steps.

{{< /quizdown >}}
