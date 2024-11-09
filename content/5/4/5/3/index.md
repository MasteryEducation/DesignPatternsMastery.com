---

linkTitle: "4.5.3 Example: Vending Machine States"
title: "State Pattern Example: Modeling Vending Machine States in Java"
description: "Explore the implementation of the State pattern in Java through a practical example of a vending machine, highlighting state transitions and handling user interactions."
categories:
- Java Design Patterns
- Behavioral Patterns
- State Management
tags:
- State Pattern
- Vending Machine
- Java
- Design Patterns
- Behavioral Design Patterns
date: 2024-10-25
type: docs
nav_weight: 4530

---

## 4.5.3 Example: Vending Machine States

In this section, we will explore the State pattern through a practical example: modeling a vending machine. The State pattern is a behavioral design pattern that allows an object to change its behavior when its internal state changes. This pattern is particularly useful for managing the complexity of state-dependent behavior in applications, such as a vending machine, where different actions are possible depending on the current state.

### Understanding the Vending Machine States

A vending machine can be in one of several states, each dictating the machine's behavior in response to user actions. For our example, we'll define the following states:

- **NoCoinState**: The initial state where the machine is waiting for a coin.
- **HasCoinState**: The state when a coin has been inserted, and the machine is ready to dispense a product.
- **DispensingState**: The state during which a product is being dispensed.
- **SoldOutState**: The state when the machine is out of products.

Each state will be represented by a class implementing a common interface, encapsulating the behavior specific to that state.

### Implementing the State Pattern

#### Step 1: Define the State Interface

The `State` interface will declare methods that all state classes must implement. These methods represent actions that can be performed on the vending machine.

```java
public interface State {
    void insertCoin();
    void ejectCoin();
    void selectProduct();
    void dispense();
}
```

#### Step 2: Implement State Classes

Each state class will implement the `State` interface and define the behavior for each action.

**NoCoinState**

```java
public class NoCoinState implements State {
    private VendingMachine vendingMachine;

    public NoCoinState(VendingMachine vendingMachine) {
        this.vendingMachine = vendingMachine;
    }

    @Override
    public void insertCoin() {
        System.out.println("Coin inserted.");
        vendingMachine.setState(vendingMachine.getHasCoinState());
    }

    @Override
    public void ejectCoin() {
        System.out.println("No coin to eject.");
    }

    @Override
    public void selectProduct() {
        System.out.println("Insert coin first.");
    }

    @Override
    public void dispense() {
        System.out.println("No coin inserted.");
    }
}
```

**HasCoinState**

```java
public class HasCoinState implements State {
    private VendingMachine vendingMachine;

    public HasCoinState(VendingMachine vendingMachine) {
        this.vendingMachine = vendingMachine;
    }

    @Override
    public void insertCoin() {
        System.out.println("Coin already inserted.");
    }

    @Override
    public void ejectCoin() {
        System.out.println("Coin ejected.");
        vendingMachine.setState(vendingMachine.getNoCoinState());
    }

    @Override
    public void selectProduct() {
        System.out.println("Product selected.");
        vendingMachine.setState(vendingMachine.getDispensingState());
    }

    @Override
    public void dispense() {
        System.out.println("Select product first.");
    }
}
```

**DispensingState**

```java
public class DispensingState implements State {
    private VendingMachine vendingMachine;

    public DispensingState(VendingMachine vendingMachine) {
        this.vendingMachine = vendingMachine;
    }

    @Override
    public void insertCoin() {
        System.out.println("Please wait, dispensing in progress.");
    }

    @Override
    public void ejectCoin() {
        System.out.println("Cannot eject coin, dispensing in progress.");
    }

    @Override
    public void selectProduct() {
        System.out.println("Dispensing in progress.");
    }

    @Override
    public void dispense() {
        System.out.println("Dispensing product...");
        if (vendingMachine.getProductCount() > 0) {
            vendingMachine.decrementProductCount();
            if (vendingMachine.getProductCount() > 0) {
                vendingMachine.setState(vendingMachine.getNoCoinState());
            } else {
                System.out.println("Out of products.");
                vendingMachine.setState(vendingMachine.getSoldOutState());
            }
        }
    }
}
```

**SoldOutState**

```java
public class SoldOutState implements State {
    private VendingMachine vendingMachine;

    public SoldOutState(VendingMachine vendingMachine) {
        this.vendingMachine = vendingMachine;
    }

    @Override
    public void insertCoin() {
        System.out.println("Machine is sold out.");
    }

    @Override
    public void ejectCoin() {
        System.out.println("No coin to eject.");
    }

    @Override
    public void selectProduct() {
        System.out.println("Machine is sold out.");
    }

    @Override
    public void dispense() {
        System.out.println("Machine is sold out.");
    }
}
```

#### Step 3: Implement the Vending Machine Context

The `VendingMachine` class will maintain a reference to the current state and delegate actions to the state object.

```java
public class VendingMachine {
    private State noCoinState;
    private State hasCoinState;
    private State dispensingState;
    private State soldOutState;

    private State currentState;
    private int productCount;

    public VendingMachine(int productCount) {
        noCoinState = new NoCoinState(this);
        hasCoinState = new HasCoinState(this);
        dispensingState = new DispensingState(this);
        soldOutState = new SoldOutState(this);

        this.productCount = productCount;
        if (productCount > 0) {
            currentState = noCoinState;
        } else {
            currentState = soldOutState;
        }
    }

    public void insertCoin() {
        currentState.insertCoin();
    }

    public void ejectCoin() {
        currentState.ejectCoin();
    }

    public void selectProduct() {
        currentState.selectProduct();
        currentState.dispense();
    }

    public void setState(State state) {
        currentState = state;
    }

    public State getNoCoinState() {
        return noCoinState;
    }

    public State getHasCoinState() {
        return hasCoinState;
    }

    public State getDispensingState() {
        return dispensingState;
    }

    public State getSoldOutState() {
        return soldOutState;
    }

    public int getProductCount() {
        return productCount;
    }

    public void decrementProductCount() {
        if (productCount > 0) {
            productCount--;
        }
    }
}
```

### Handling State Transitions

State transitions are triggered by user actions such as inserting a coin or selecting a product. The `VendingMachine` context delegates these actions to the current state, which determines the appropriate response and transitions to the next state if necessary.

### Benefits of Using the State Pattern

- **Encapsulation of State-Specific Behavior**: Each state class encapsulates the behavior specific to that state, making the code easier to manage and extend.
- **Simplified State Management**: The `VendingMachine` class delegates state-specific behavior to state objects, reducing complexity in the context class.
- **Ease of Extension**: Adding new states or modifying existing ones can be done with minimal impact on the rest of the system.

### Handling Errors and Invalid Operations

Each state class handles invalid operations gracefully, such as attempting to eject a coin when none has been inserted. This ensures that the vending machine behaves predictably and provides clear feedback to the user.

### Extending the Vending Machine

To extend the vending machine with new features or products, you can add new states or modify existing ones. For example, you might add a `MaintenanceState` for when the machine is being serviced.

### Testing State Transitions

Testing each state and the transitions between them is crucial. You can write unit tests for each state class, verifying that the correct actions and transitions occur in response to user inputs.

### Handling Concurrency

If multiple users interact with the vending machine simultaneously, you must ensure thread safety. Consider using synchronization mechanisms or atomic variables to manage shared state, such as the product count.

### Designing Reusable and Maintainable State Classes

Design state classes to be reusable and maintainable by adhering to principles such as the Single Responsibility Principle. Each state class should focus on handling behavior specific to that state.

### Performance Considerations

While the State pattern simplifies state management, consider the performance implications of frequent state transitions. Ensure that state changes are efficient and do not introduce unnecessary overhead.

### User Interface and Feedback

A clear user interface reflecting the vending machine's current state is essential. Provide feedback to users about the current state and available actions, enhancing the user experience.

### Integrating with User Input Handling

Integrate the State pattern with user input handling mechanisms to ensure that user actions are processed correctly and that the machine's state is updated accordingly.

### Conclusion

The State pattern provides a robust framework for managing state-dependent behavior in applications like a vending machine. By encapsulating state-specific logic in separate classes, you can simplify state management, improve code maintainability, and enhance the user experience.

## Quiz Time!

{{< quizdown >}}

### Which state represents the vending machine when it is waiting for a coin?

- [x] NoCoinState
- [ ] HasCoinState
- [ ] DispensingState
- [ ] SoldOutState

> **Explanation:** The `NoCoinState` represents the vending machine when it is waiting for a coin to be inserted.

### What does the `HasCoinState` allow the user to do?

- [ ] Insert another coin
- [x] Select a product
- [ ] Dispense a product
- [ ] Eject a coin

> **Explanation:** In the `HasCoinState`, the user can select a product, which transitions the machine to the `DispensingState`.

### Which method in the `State` interface is used to handle product selection?

- [ ] insertCoin()
- [ ] ejectCoin()
- [x] selectProduct()
- [ ] dispense()

> **Explanation:** The `selectProduct()` method is used to handle product selection in the `State` interface.

### What happens if a user tries to insert a coin in the `SoldOutState`?

- [ ] Coin is accepted
- [x] Machine informs that it is sold out
- [ ] Coin is ejected
- [ ] Product is dispensed

> **Explanation:** In the `SoldOutState`, the machine informs the user that it is sold out and does not accept the coin.

### How does the `VendingMachine` class delegate actions to the current state?

- [x] By calling methods on the current state object
- [ ] By directly handling actions
- [ ] By using a switch statement
- [ ] By ignoring actions

> **Explanation:** The `VendingMachine` class delegates actions by calling methods on the current state object.

### What is a benefit of using the State pattern in a vending machine?

- [x] Encapsulation of state-specific behavior
- [ ] Increased complexity
- [ ] Reduced flexibility
- [ ] Direct handling of all actions in the context class

> **Explanation:** The State pattern encapsulates state-specific behavior, making it easier to manage and extend.

### How can you ensure thread safety in a multi-user vending machine?

- [x] Use synchronization mechanisms
- [ ] Ignore concurrency issues
- [ ] Allow only one user at a time
- [ ] Use a single state object for all users

> **Explanation:** Synchronization mechanisms or atomic variables can ensure thread safety in a multi-user vending machine.

### What should each state class focus on according to the Single Responsibility Principle?

- [x] Handling behavior specific to that state
- [ ] Managing all machine operations
- [ ] Handling user inputs
- [ ] Managing product inventory

> **Explanation:** Each state class should focus on handling behavior specific to that state, adhering to the Single Responsibility Principle.

### How can you test state transitions effectively?

- [x] Write unit tests for each state class
- [ ] Manually test the machine
- [ ] Ignore testing
- [ ] Use a single test for all states

> **Explanation:** Writing unit tests for each state class ensures that the correct actions and transitions occur in response to user inputs.

### True or False: The State pattern can help improve the user experience by providing clear feedback.

- [x] True
- [ ] False

> **Explanation:** The State pattern can improve the user experience by providing clear feedback about the current state and available actions.

{{< /quizdown >}}
