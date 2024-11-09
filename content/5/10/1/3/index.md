---
linkTitle: "A.3 Case Studies in Refactoring"
title: "Refactoring to Patterns: Case Studies in Java Design"
description: "Explore real-world case studies of refactoring legacy Java code to apply design patterns like Strategy, Observer, and State for improved flexibility, modularity, and maintainability."
categories:
- Software Development
- Java Programming
- Design Patterns
tags:
- Refactoring
- Strategy Pattern
- Observer Pattern
- State Pattern
- Java
date: 2024-10-25
type: docs
nav_weight: 1013000
---

## A.3 Case Studies in Refactoring

Refactoring is a critical practice in software development, allowing developers to improve the design and structure of existing code without altering its external behavior. By refactoring to design patterns, we can enhance code flexibility, maintainability, and scalability. This section presents three case studies that illustrate the transformation of legacy Java codebases through the application of design patterns: Strategy, Observer, and State.

### Case Study 1: Applying the Strategy Pattern for Flexible Algorithm Selection

#### Initial Problem

In many legacy systems, algorithms are often hard-coded within classes, leading to rigid and inflexible code. This was the case in a financial application where different interest calculation algorithms were embedded directly within the `LoanCalculator` class. This approach made it difficult to introduce new algorithms or modify existing ones without altering the core logic of the class.

#### Refactoring Steps

1. **Identify Hard-Coded Algorithms**: The first step was to identify all the hard-coded algorithms within the `LoanCalculator` class.

2. **Define Strategy Interface**: A `CalculationStrategy` interface was created to define a common method, `calculateInterest()`, which all strategies must implement.

3. **Extract Strategies**: Each algorithm was extracted into its own class implementing the `CalculationStrategy` interface.

4. **Integrate Strategy Pattern**: The `LoanCalculator` class was refactored to use a `CalculationStrategy` object, allowing the strategy to be set dynamically at runtime.

#### Before and After Code Snippets

**Before Refactoring:**

```java
public class LoanCalculator {
    public double calculateInterest(double principal, double rate, int time, String type) {
        if ("simple".equals(type)) {
            return (principal * rate * time) / 100;
        } else if ("compound".equals(type)) {
            return principal * Math.pow((1 + rate / 100), time) - principal;
        }
        throw new IllegalArgumentException("Unknown interest type");
    }
}
```

**After Refactoring:**

```java
public interface CalculationStrategy {
    double calculateInterest(double principal, double rate, int time);
}

public class SimpleInterestStrategy implements CalculationStrategy {
    public double calculateInterest(double principal, double rate, int time) {
        return (principal * rate * time) / 100;
    }
}

public class CompoundInterestStrategy implements CalculationStrategy {
    public double calculateInterest(double principal, double rate, int time) {
        return principal * Math.pow((1 + rate / 100), time) - principal;
    }
}

public class LoanCalculator {
    private CalculationStrategy strategy;

    public LoanCalculator(CalculationStrategy strategy) {
        this.strategy = strategy;
    }

    public double calculateInterest(double principal, double rate, int time) {
        return strategy.calculateInterest(principal, rate, time);
    }
}
```

#### Benefits Gained

- **Improved Flexibility**: New algorithms can be added without modifying existing code.
- **Easier Maintenance**: Each strategy is encapsulated in its own class, simplifying updates and debugging.

### Case Study 2: Using the Observer Pattern to Decouple Components

#### Initial Problem

In an event-driven system managing stock market data, components were tightly coupled, with direct method calls to update observers. This coupling made it challenging to add new observers or change notification logic without affecting the entire system.

#### Refactoring Process

1. **Identify Coupled Components**: The tightly coupled classes were identified, particularly those responsible for broadcasting stock price updates.

2. **Define Observer Interfaces**: An `Observer` interface was introduced to standardize the update mechanism.

3. **Implement Notification Mechanism**: A `Subject` interface was created to manage observers and notify them of changes.

4. **Refactor Components**: The system was refactored to use the `Observer` and `Subject` interfaces, decoupling the components.

#### Code Examples

**Before Refactoring:**

```java
public class StockMarket {
    private List<StockDisplay> displays = new ArrayList<>();

    public void addDisplay(StockDisplay display) {
        displays.add(display);
    }

    public void updateStockPrice(String stock, double price) {
        for (StockDisplay display : displays) {
            display.update(stock, price);
        }
    }
}
```

**After Refactoring:**

```java
public interface Observer {
    void update(String stock, double price);
}

public interface Subject {
    void registerObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

public class StockMarket implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private Map<String, Double> stockPrices = new HashMap<>();

    public void registerObserver(Observer observer) {
        observers.add(observer);
    }

    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    public void setStockPrice(String stock, double price) {
        stockPrices.put(stock, price);
        notifyObservers();
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            for (Map.Entry<String, Double> entry : stockPrices.entrySet()) {
                observer.update(entry.getKey(), entry.getValue());
            }
        }
    }
}
```

#### Improvements

- **Enhanced Modularity**: Observers can be added or removed without affecting the subject.
- **Extensible Functionality**: New types of observers can be introduced with minimal changes.

### Case Study 3: Simplifying Conditional Logic with the State Pattern

#### Challenges with Nested Conditionals

A vending machine system had complex nested conditionals to manage different states (e.g., waiting for selection, dispensing item). This made the code difficult to read and maintain, and adding new states required modifying existing logic.

#### Refactoring Steps

1. **Identify State-Dependent Logic**: The conditional logic was analyzed to identify distinct states and transitions.

2. **Create State Classes**: State-specific classes were created, each encapsulating behavior for a particular state.

3. **Implement State Pattern**: The vending machine was refactored to delegate state-specific behavior to these classes.

#### Code Excerpts

**Before Refactoring:**

```java
public class VendingMachine {
    private String state = "waiting";

    public void handleAction(String action) {
        if ("waiting".equals(state)) {
            if ("select".equals(action)) {
                state = "selected";
                System.out.println("Item selected");
            }
        } else if ("selected".equals(state)) {
            if ("pay".equals(action)) {
                state = "paid";
                System.out.println("Payment received");
            }
        } else if ("paid".equals(state)) {
            if ("dispense".equals(action)) {
                state = "waiting";
                System.out.println("Item dispensed");
            }
        }
    }
}
```

**After Refactoring:**

```java
public interface State {
    void handleAction(VendingMachine machine, String action);
}

public class WaitingState implements State {
    public void handleAction(VendingMachine machine, String action) {
        if ("select".equals(action)) {
            machine.setState(new SelectedState());
            System.out.println("Item selected");
        }
    }
}

public class SelectedState implements State {
    public void handleAction(VendingMachine machine, String action) {
        if ("pay".equals(action)) {
            machine.setState(new PaidState());
            System.out.println("Payment received");
        }
    }
}

public class PaidState implements State {
    public void handleAction(VendingMachine machine, String action) {
        if ("dispense".equals(action)) {
            machine.setState(new WaitingState());
            System.out.println("Item dispensed");
        }
    }
}

public class VendingMachine {
    private State state = new WaitingState();

    public void setState(State state) {
        this.state = state;
    }

    public void handleAction(String action) {
        state.handleAction(this, action);
    }
}
```

#### Resulting Benefits

- **Code Clarity**: Each state is encapsulated, making the code easier to understand.
- **Ease of Adding States**: New states can be added with minimal impact on existing code.

### Lessons Learned and Best Practices

- **Identify Code Smells**: Regularly review code for signs of rigidity, fragility, or immobility to identify refactoring opportunities.
- **Collaborate with the Team**: Engage team members in the refactoring process to ensure shared understanding and knowledge transfer.
- **Document Changes**: Maintain comprehensive documentation of refactoring efforts for future reference and onboarding.
- **Test Thoroughly**: Ensure that refactored code is thoroughly tested to verify that functionality remains unchanged.

### Encouragement for Continuous Improvement

Refactoring to design patterns is not a one-time task but an ongoing process. Developers should continuously analyze their codebases for opportunities to apply patterns that enhance design and performance. By embracing refactoring, teams can create robust, maintainable, and scalable software systems.

## Quiz Time!

{{< quizdown >}}

### What is the main benefit of applying the Strategy Pattern in the first case study?

- [x] Improved flexibility and easier maintenance
- [ ] Increased performance
- [ ] Reduced code size
- [ ] Enhanced security

> **Explanation:** The Strategy Pattern allows for flexible algorithm selection and easier maintenance by encapsulating algorithms in separate classes.

### How does the Observer Pattern improve modularity in the second case study?

- [x] By decoupling the subject from its observers
- [ ] By reducing the number of classes
- [ ] By increasing the complexity of the code
- [ ] By enforcing a single update mechanism

> **Explanation:** The Observer Pattern decouples the subject from its observers, allowing for more modular and extensible code.

### What was the primary issue with the original vending machine code in the third case study?

- [x] Complex nested conditionals
- [ ] Lack of error handling
- [ ] Inefficient algorithms
- [ ] Poor user interface

> **Explanation:** The original code used complex nested conditionals to manage states, making it difficult to maintain and extend.

### What pattern was used to refactor the vending machine code?

- [x] State Pattern
- [ ] Strategy Pattern
- [ ] Observer Pattern
- [ ] Singleton Pattern

> **Explanation:** The State Pattern was used to refactor the vending machine code, encapsulating state-specific behavior in separate classes.

### Which of the following is a benefit of using the State Pattern?

- [x] Code clarity and ease of adding new states
- [ ] Increased code size
- [ ] Reduced flexibility
- [ ] Decreased performance

> **Explanation:** The State Pattern improves code clarity and makes it easier to add new states by encapsulating state-specific behavior.

### What is a common challenge when refactoring to design patterns?

- [x] Ensuring that functionality remains unchanged
- [ ] Reducing the number of classes
- [ ] Increasing code complexity
- [ ] Decreasing code readability

> **Explanation:** A common challenge is ensuring that the refactored code maintains the same functionality as the original code.

### Why is team collaboration important during refactoring?

- [x] To ensure shared understanding and knowledge transfer
- [ ] To reduce the number of classes
- [ ] To increase code complexity
- [ ] To decrease code readability

> **Explanation:** Team collaboration is crucial to ensure shared understanding and knowledge transfer, making the refactoring process more effective.

### How does documenting refactoring changes benefit future development?

- [x] It provides a reference for future changes and onboarding
- [ ] It reduces the number of classes
- [ ] It increases code complexity
- [ ] It decreases code readability

> **Explanation:** Documenting refactoring changes provides a reference for future development and helps onboard new team members.

### What should be done after refactoring to ensure code quality?

- [x] Thorough testing
- [ ] Reducing the number of classes
- [ ] Increasing code complexity
- [ ] Decreasing code readability

> **Explanation:** Thorough testing should be conducted to ensure that the refactored code maintains the same functionality and quality as the original.

### True or False: Refactoring to patterns can lead to performance optimizations.

- [x] True
- [ ] False

> **Explanation:** Refactoring to patterns can lead to performance optimizations by improving code structure and design.

{{< /quizdown >}}
