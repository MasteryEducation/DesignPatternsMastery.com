---
linkTitle: "A.2 Common Refactoring Techniques"
title: "A.2 Common Refactoring Techniques: Enhancing Java Code Quality"
description: "Explore essential refactoring techniques in Java, including Extract Method, Rename, Move Method, and more, to improve code readability, maintainability, and design."
categories:
- Software Development
- Java Programming
- Code Quality
tags:
- Refactoring
- Java
- Design Patterns
- Code Improvement
- Software Engineering
date: 2024-10-25
type: docs
nav_weight: 1012000
---

## A.2 Common Refactoring Techniques

Refactoring is a disciplined technique for restructuring an existing body of code, altering its internal structure without changing its external behavior. This process is crucial in maintaining a clean, efficient, and adaptable codebase. In this section, we will explore some fundamental refactoring techniques that can significantly enhance the quality of your Java applications.

### Extract Method

One of the most common refactoring techniques is **Extract Method**, which involves breaking down long methods into smaller, more focused methods. This improves readability and makes the code easier to understand and maintain.

#### When to Apply

- When a method is too long or complex.
- When you notice repeated code blocks that can be reused.
- When a method does multiple things that can be logically separated.

#### How to Apply

Identify a block of code that performs a single task and move it into a new method. Replace the original code block with a call to the new method.

**Before Refactoring:**

```java
public class OrderProcessor {
    public void processOrder(Order order) {
        // Validate order
        if (order.isValid()) {
            // Calculate total
            double total = 0;
            for (Item item : order.getItems()) {
                total += item.getPrice();
            }
            // Print receipt
            System.out.println("Order total: " + total);
            System.out.println("Thank you for your purchase!");
        }
    }
}
```

**After Refactoring:**

```java
public class OrderProcessor {
    public void processOrder(Order order) {
        if (order.isValid()) {
            double total = calculateTotal(order);
            printReceipt(total);
        }
    }

    private double calculateTotal(Order order) {
        double total = 0;
        for (Item item : order.getItems()) {
            total += item.getPrice();
        }
        return total;
    }

    private void printReceipt(double total) {
        System.out.println("Order total: " + total);
        System.out.println("Thank you for your purchase!");
    }
}
```

### Rename Variable/Method/Class

Meaningful names are essential for code clarity. **Renaming** involves changing the name of variables, methods, or classes to better reflect their purpose.

#### When to Apply

- When names are ambiguous or misleading.
- When the purpose of a variable, method, or class changes.

#### How to Apply

Choose a name that clearly describes the entity's role or behavior. Use automated refactoring tools to ensure all references are updated.

**Before Refactoring:**

```java
public class Calc {
    public int a(int x, int y) {
        return x + y;
    }
}
```

**After Refactoring:**

```java
public class Calculator {
    public int add(int firstNumber, int secondNumber) {
        return firstNumber + secondNumber;
    }
}
```

### Move Method/Field

**Move Method/Field** involves relocating methods or fields to the classes where they logically belong, enhancing cohesion.

#### When to Apply

- When a method or field is used more by another class than its own.
- When the current class has too many responsibilities.

#### How to Apply

Identify the class that uses the method or field most frequently and move it there.

**Before Refactoring:**

```java
public class Customer {
    private Address address;

    public String getFullAddress() {
        return address.getStreet() + ", " + address.getCity();
    }
}
```

**After Refactoring:**

```java
public class Address {
    private String street;
    private String city;

    public String getFullAddress() {
        return street + ", " + city;
    }
}

public class Customer {
    private Address address;
}
```

### Inline Method

**Inline Method** replaces a method call with the method's content, eliminating unnecessary indirection.

#### When to Apply

- When a method is trivial and used only once.
- When the method's name does not add clarity.

#### How to Apply

Replace the method call with the method's body and remove the method definition.

**Before Refactoring:**

```java
public class MathUtils {
    public int square(int number) {
        return number * number;
    }

    public int calculateSquare(int number) {
        return square(number);
    }
}
```

**After Refactoring:**

```java
public class MathUtils {
    public int calculateSquare(int number) {
        return number * number;
    }
}
```

### Encapsulate Field

**Encapsulate Field** involves using getters and setters to protect fields, promoting encapsulation.

#### When to Apply

- When fields are accessed directly.
- When you need to add validation or logic around field access.

#### How to Apply

Make the field private and provide public getter and setter methods.

**Before Refactoring:**

```java
public class Person {
    public String name;
}
```

**After Refactoring:**

```java
public class Person {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

### Replace Conditional with Polymorphism

This technique eliminates complex conditionals by using inheritance and method overriding.

#### When to Apply

- When you have a complex conditional that determines behavior.
- When different classes can represent the different cases.

#### How to Apply

Create a superclass with a method that is overridden by subclasses to provide specific behavior.

**Before Refactoring:**

```java
public class Bird {
    public void move(String type) {
        if (type.equals("Sparrow")) {
            fly();
        } else if (type.equals("Penguin")) {
            walk();
        }
    }

    private void fly() {
        System.out.println("Flying");
    }

    private void walk() {
        System.out.println("Walking");
    }
}
```

**After Refactoring:**

```java
public abstract class Bird {
    public abstract void move();
}

public class Sparrow extends Bird {
    @Override
    public void move() {
        System.out.println("Flying");
    }
}

public class Penguin extends Bird {
    @Override
    public void move() {
        System.out.println("Walking");
    }
}
```

### Extract Class

**Extract Class** divides a large class into smaller, more focused classes, enhancing the Single Responsibility Principle.

#### When to Apply

- When a class has too many responsibilities.
- When a class has multiple unrelated fields or methods.

#### How to Apply

Identify related fields and methods and move them to a new class.

**Before Refactoring:**

```java
public class Employee {
    private String name;
    private String email;
    private String phoneNumber;

    public void sendEmail(String message) {
        // send email logic
    }

    public void makeCall() {
        // call logic
    }
}
```

**After Refactoring:**

```java
public class Employee {
    private String name;
    private ContactInfo contactInfo;

    // other employee-related methods
}

public class ContactInfo {
    private String email;
    private String phoneNumber;

    public void sendEmail(String message) {
        // send email logic
    }

    public void makeCall() {
        // call logic
    }
}
```

### Introduce Parameter Object

**Introduce Parameter Object** groups related parameters into an object, simplifying method signatures.

#### When to Apply

- When a method has too many parameters.
- When parameters are frequently passed together.

#### How to Apply

Create a new class to encapsulate the parameters and modify the method to accept the new object.

**Before Refactoring:**

```java
public class OrderService {
    public void placeOrder(String customerName, String product, int quantity) {
        // order placement logic
    }
}
```

**After Refactoring:**

```java
public class OrderService {
    public void placeOrder(OrderDetails orderDetails) {
        // order placement logic
    }
}

public class OrderDetails {
    private String customerName;
    private String product;
    private int quantity;

    // getters and setters
}
```

### Replace Magic Numbers with Constants

Using named constants instead of magic numbers improves maintainability and clarity.

#### When to Apply

- When numbers appear without explanation.
- When numbers are used in multiple places.

#### How to Apply

Define a constant with a descriptive name and replace occurrences of the number with the constant.

**Before Refactoring:**

```java
public class Circle {
    public double calculateCircumference(double radius) {
        return 2 * 3.14159 * radius;
    }
}
```

**After Refactoring:**

```java
public class Circle {
    private static final double PI = 3.14159;

    public double calculateCircumference(double radius) {
        return 2 * PI * radius;
    }
}
```

### Decompose Conditional

Breaking down complex conditional statements into separate methods or variables enhances readability.

#### When to Apply

- When a conditional statement is too complex.
- When conditions are repeated.

#### How to Apply

Extract conditions into methods or variables with descriptive names.

**Before Refactoring:**

```java
public class DiscountCalculator {
    public double calculateDiscount(double price, int quantity) {
        if (price > 100 && quantity > 10) {
            return price * 0.1;
        }
        return 0;
    }
}
```

**After Refactoring:**

```java
public class DiscountCalculator {
    public double calculateDiscount(double price, int quantity) {
        if (isEligibleForDiscount(price, quantity)) {
            return price * 0.1;
        }
        return 0;
    }

    private boolean isEligibleForDiscount(double price, int quantity) {
        return price > 100 && quantity > 10;
    }
}
```

### Impact on Code Quality and Design

Each refactoring technique aims to improve code readability, maintainability, and design. By applying these techniques, you can achieve:

- **Improved Readability**: Code is easier to read and understand.
- **Enhanced Maintainability**: Changes and updates are easier to implement.
- **Better Design**: Code adheres to design principles like SOLID, promoting a clean architecture.

### Best Practices for Refactoring

- **Make Small Incremental Changes**: Refactor in small steps to minimize risk.
- **Use Automated Tools**: Leverage IDEs and tools like IntelliJ IDEA or Eclipse for safe refactoring.
- **Test After Each Change**: Ensure that behavior remains unchanged by running tests after each refactoring step.

### Conclusion

Refactoring is an essential skill for any Java developer. By mastering these techniques, you can significantly improve the quality of your codebase, making it more robust and adaptable to future changes. Remember, refactoring is not just about changing code; it's about improving design and ensuring that your software can evolve gracefully.

## Quiz Time!

{{< quizdown >}}

### What is the primary goal of refactoring?

- [x] To improve the internal structure of code without changing its external behavior
- [ ] To add new features to the code
- [ ] To increase the complexity of the code
- [ ] To make the code run faster

> **Explanation:** Refactoring focuses on improving the internal structure of the code while keeping its external behavior unchanged.

### When should you apply the Extract Method refactoring technique?

- [x] When a method is too long or complex
- [ ] When a method is too short
- [ ] When a method is not used
- [ ] When a method is deprecated

> **Explanation:** Extract Method is used to break down long or complex methods into smaller, more manageable ones.

### What is the benefit of renaming variables, methods, or classes?

- [x] It enhances code clarity and readability
- [ ] It makes the code run faster
- [ ] It increases the number of lines of code
- [ ] It hides the code's functionality

> **Explanation:** Renaming improves clarity and readability by using meaningful names that reflect the purpose of the code.

### How does the Move Method refactoring technique improve code cohesion?

- [x] By relocating methods to the classes where they logically belong
- [ ] By deleting unused methods
- [ ] By duplicating methods across classes
- [ ] By renaming methods

> **Explanation:** Moving methods to the appropriate classes enhances cohesion by ensuring that related functionalities are grouped together.

### What does the Inline Method refactoring technique involve?

- [x] Replacing a method call with the method's content
- [ ] Creating a new method
- [ ] Deleting a method
- [ ] Renaming a method

> **Explanation:** Inline Method involves replacing a method call with the actual content of the method, eliminating unnecessary indirection.

### Why is it important to encapsulate fields in a class?

- [x] To protect fields by using getters and setters
- [ ] To make fields public
- [ ] To increase the number of fields
- [ ] To decrease the number of methods

> **Explanation:** Encapsulation protects fields by controlling access through getters and setters, promoting data integrity and security.

### How does replacing conditionals with polymorphism improve code design?

- [x] By eliminating complex conditionals and using inheritance
- [ ] By adding more conditionals
- [ ] By making code less readable
- [ ] By increasing the number of classes

> **Explanation:** Replacing conditionals with polymorphism uses inheritance to handle different cases, improving code design and reducing complexity.

### What is the purpose of the Extract Class refactoring technique?

- [x] To divide a large class into smaller, more focused classes
- [ ] To combine multiple classes into one
- [ ] To delete unused classes
- [ ] To rename classes

> **Explanation:** Extract Class divides a large class into smaller, more focused classes, enhancing the Single Responsibility Principle.

### How does introducing a parameter object simplify method signatures?

- [x] By grouping related parameters into an object
- [ ] By removing parameters
- [ ] By adding more parameters
- [ ] By renaming parameters

> **Explanation:** Introducing a parameter object groups related parameters into a single object, simplifying method signatures and improving readability.

### True or False: Refactoring should only be done at the end of a project.

- [ ] True
- [x] False

> **Explanation:** Refactoring should be an ongoing process throughout the development lifecycle to continuously improve code quality.

{{< /quizdown >}}
