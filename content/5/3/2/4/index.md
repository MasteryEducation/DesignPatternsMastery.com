---
linkTitle: "3.2.4 Example: Adding Responsibilities to Coffee Objects"
title: "Decorator Pattern: Adding Responsibilities to Coffee Objects"
description: "Explore the Decorator Pattern in Java by modeling a coffee shop menu, demonstrating how to dynamically add responsibilities to coffee objects with flexibility and maintainability."
categories:
- Design Patterns
- Java Programming
- Software Architecture
tags:
- Decorator Pattern
- Java
- Object-Oriented Design
- Code Reusability
- Software Development
date: 2024-10-25
type: docs
nav_weight: 324000
---

## 3.2.4 Example: Adding Responsibilities to Coffee Objects

In this section, we will explore the Decorator Pattern by modeling a coffee shop menu. This pattern is particularly useful for adding responsibilities to objects dynamically and is a perfect fit for scenarios where objects need to be extended with new functionality without altering their structure.

### Understanding the Decorator Pattern

The Decorator Pattern allows behavior to be added to individual objects, dynamically, without affecting the behavior of other objects from the same class. This pattern is often used to adhere to the Open/Closed Principle, which states that software entities should be open for extension but closed for modification.

### Modeling a Coffee Shop Menu

Let's consider a coffee shop where customers can order different types of coffee with various add-ons. We'll use the Decorator Pattern to model this system, enabling us to add responsibilities like milk, sugar, and whipped cream to coffee objects.

#### Defining the `Coffee` Interface

We'll start by defining a `Coffee` interface with methods to get the cost and description of the coffee.

```java
public interface Coffee {
    double getCost();
    String getDescription();
}
```

#### Implementing Basic Coffee Classes

Next, we implement basic coffee classes such as `Espresso` and `Decaf`, which will serve as the concrete components.

```java
public class Espresso implements Coffee {
    @Override
    public double getCost() {
        return 1.99;
    }

    @Override
    public String getDescription() {
        return "Espresso";
    }
}

public class Decaf implements Coffee {
    @Override
    public double getCost() {
        return 1.49;
    }

    @Override
    public String getDescription() {
        return "Decaf";
    }
}
```

#### Creating Decorator Classes for Add-ons

We'll create decorator classes for add-ons like `Milk`, `Sugar`, and `WhippedCream`. Each decorator will implement the `Coffee` interface and hold a reference to a `Coffee` object.

```java
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
}

public class Milk extends CoffeeDecorator {
    public Milk(Coffee coffee) {
        super(coffee);
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.50;
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Milk";
    }
}

public class Sugar extends CoffeeDecorator {
    public Sugar(Coffee coffee) {
        super(coffee);
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.20;
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Sugar";
    }
}

public class WhippedCream extends CoffeeDecorator {
    public WhippedCream(Coffee coffee) {
        super(coffee);
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.70;
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Whipped Cream";
    }
}
```

#### Wrapping Coffee Objects with Decorators

Let's see how we can wrap coffee objects with different decorators to create various combinations.

```java
public class CoffeeShop {
    public static void main(String[] args) {
        Coffee espresso = new Espresso();
        System.out.println(espresso.getDescription() + " $" + espresso.getCost());

        Coffee espressoWithMilk = new Milk(espresso);
        System.out.println(espressoWithMilk.getDescription() + " $" + espressoWithMilk.getCost());

        Coffee decafWithSugarAndCream = new WhippedCream(new Sugar(new Decaf()));
        System.out.println(decafWithSugarAndCream.getDescription() + " $" + decafWithSugarAndCream.getCost());
    }
}
```

#### Flexibility and Extensibility

The Decorator Pattern provides flexibility in creating various coffee combinations at runtime. New add-ons can be introduced without modifying existing code, adhering to the Open/Closed Principle. For example, adding a new `Caramel` decorator would not require changes to the existing classes.

#### Testing Strategies

Testing different decorator combinations involves ensuring that each combination correctly computes the cost and description. Unit tests can be written for each decorator to verify that they add the correct cost and description.

```java
@Test
public void testEspressoWithMilkAndSugar() {
    Coffee coffee = new Sugar(new Milk(new Espresso()));
    assertEquals(2.69, coffee.getCost(), 0.01);
    assertEquals("Espresso, Milk, Sugar", coffee.getDescription());
}
```

#### Managing Decorator Order

While the order of decorators generally does not affect the behavior in this example, it can be crucial in other contexts. For instance, if a `Discount` decorator is introduced, its position in the chain could affect the final cost.

#### Promoting Code Reusability and Maintainability

By using the Decorator Pattern, we promote code reusability and maintainability. Each decorator is responsible for a single responsibility, making the codebase easier to manage and extend.

#### Extending the Example

This example can be extended by introducing size options or promotions. For instance, a `SizeDecorator` could adjust the cost based on the size of the coffee, while a `PromotionDecorator` might apply discounts.

### Conclusion

The Decorator Pattern is a powerful tool for dynamically adding responsibilities to objects. It allows for flexible and maintainable code, making it an excellent choice for scenarios like our coffee shop example. By understanding and applying this pattern, developers can create systems that are both extensible and easy to maintain.

## Quiz Time!

{{< quizdown >}}

### What is the primary purpose of the Decorator Pattern?

- [x] To add responsibilities to individual objects dynamically
- [ ] To create a new class hierarchy
- [ ] To simplify complex subsystems
- [ ] To manage object states

> **Explanation:** The Decorator Pattern is used to add responsibilities to objects dynamically without affecting other objects from the same class.

### Which method should a decorator class implement to add functionality?

- [x] The method defined in the interface it decorates
- [ ] A new method unique to the decorator
- [ ] A static method
- [ ] A method with no parameters

> **Explanation:** A decorator class should implement the methods defined in the interface it decorates to add or modify functionality.

### How does the Decorator Pattern adhere to the Open/Closed Principle?

- [x] By allowing new functionalities to be added without modifying existing code
- [ ] By using inheritance to extend classes
- [ ] By using static methods
- [ ] By encapsulating all behaviors in a single class

> **Explanation:** The Decorator Pattern allows new functionalities to be added by creating new decorators, thus keeping existing code closed for modification but open for extension.

### In the coffee shop example, what is the role of the `CoffeeDecorator` class?

- [x] It serves as a base class for all coffee add-ons
- [ ] It represents a concrete coffee type
- [ ] It calculates the total cost of coffee
- [ ] It manages the coffee shop inventory

> **Explanation:** The `CoffeeDecorator` class serves as a base class for all coffee add-ons, allowing them to wrap a `Coffee` object.

### What would be a potential issue when using the Decorator Pattern?

- [x] Managing the order of decorators if it affects behavior
- [ ] Increasing the number of classes
- [ ] Reducing code readability
- [ ] Decreasing performance

> **Explanation:** In some cases, the order of decorators can affect the behavior, such as when applying discounts or other conditional logic.

### How can you test different decorator combinations effectively?

- [x] By writing unit tests for each combination
- [ ] By manually testing each combination in the application
- [ ] By using a single test case for all combinations
- [ ] By relying on user feedback

> **Explanation:** Writing unit tests for each combination ensures that all possible configurations are tested for correctness.

### What is a benefit of using the Decorator Pattern in software design?

- [x] It promotes code reusability and maintainability
- [ ] It simplifies class hierarchies
- [ ] It reduces the need for interfaces
- [ ] It eliminates the need for testing

> **Explanation:** The Decorator Pattern promotes code reusability and maintainability by allowing functionalities to be added dynamically.

### How does the Decorator Pattern differ from inheritance?

- [x] It adds functionality at runtime rather than compile-time
- [ ] It requires more classes
- [ ] It is less flexible
- [ ] It is more difficult to implement

> **Explanation:** The Decorator Pattern adds functionality at runtime, allowing for greater flexibility compared to compile-time inheritance.

### Can new add-ons be added to the coffee shop example without modifying existing code?

- [x] True
- [ ] False

> **Explanation:** New add-ons can be added by creating new decorator classes, without modifying existing code, adhering to the Open/Closed Principle.

### What is an example of extending the coffee shop example?

- [x] Introducing size options or promotions
- [ ] Adding more coffee types to the base class
- [ ] Removing existing decorators
- [ ] Simplifying the `Coffee` interface

> **Explanation:** Extending the example can involve introducing size options or promotions, which can be implemented as additional decorators.

{{< /quizdown >}}
