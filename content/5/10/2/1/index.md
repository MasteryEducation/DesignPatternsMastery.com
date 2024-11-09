---
linkTitle: "B.1 Summary of Gang of Four Patterns"
title: "B.1 Summary of Gang of Four Patterns: Comprehensive Guide to GoF Design Patterns in Java"
description: "Explore concise summaries of the 23 Gang of Four design patterns, including intent, participants, collaborations, consequences, and sample code in Java."
categories:
- Design Patterns
- Java Programming
- Software Architecture
tags:
- Gang of Four
- Creational Patterns
- Structural Patterns
- Behavioral Patterns
- Java Design Patterns
date: 2024-10-25
type: docs
nav_weight: 1021000
---

## B.1 Summary of Gang of Four Patterns

The Gang of Four (GoF) design patterns are a set of 23 foundational patterns for object-oriented software design, introduced by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides in their seminal book, "Design Patterns: Elements of Reusable Object-Oriented Software." These patterns are divided into three categories: Creational, Structural, and Behavioral. This section provides a concise summary of each pattern, including its intent, participants, collaborations, consequences, and a sample Java code snippet to illustrate its application.

### Creational Patterns

Creational patterns deal with object creation mechanisms, trying to create objects in a manner suitable to the situation.

#### 1. Singleton

- **Category**: Creational
- **Intent**: Ensure a class has only one instance and provide a global point of access to it.
- **Participants**:
  - Singleton: The class that defines the `getInstance` method.
- **Collaborations**: The Singleton class itself manages its unique instance.
- **Consequences**: Controlled access to the sole instance, but can introduce global state and complicate testing.
- **Sample Code**:

  ```java
  public class Singleton {
      private static Singleton instance;
      
      private Singleton() {}
      
      public static Singleton getInstance() {
          if (instance == null) {
              instance = new Singleton();
          }
          return instance;
      }
  }
  ```

#### 2. Factory Method

- **Category**: Creational
- **Intent**: Define an interface for creating an object, but let subclasses alter the type of objects that will be created.
- **Participants**:
  - Creator: Declares the factory method.
  - ConcreteCreator: Implements the factory method.
  - Product: Defines the interface of objects the factory method creates.
- **Collaborations**: Creator relies on its subclasses to define the factory method.
- **Consequences**: Provides hooks for subclasses, but may require subclassing just to instantiate a particular class.
- **Sample Code**:

  ```java
  abstract class Product {}
  
  class ConcreteProduct extends Product {}
  
  abstract class Creator {
      public abstract Product factoryMethod();
  }
  
  class ConcreteCreator extends Creator {
      @Override
      public Product factoryMethod() {
          return new ConcreteProduct();
      }
  }
  ```

#### 3. Abstract Factory

- **Category**: Creational
- **Intent**: Provide an interface for creating families of related or dependent objects without specifying their concrete classes.
- **Participants**:
  - AbstractFactory: Declares an interface for operations that create abstract products.
  - ConcreteFactory: Implements the operations to create concrete product objects.
  - AbstractProduct: Declares an interface for a type of product object.
  - ConcreteProduct: Defines a product object to be created by the corresponding concrete factory.
- **Collaborations**: Concrete factories produce a family of products that are compatible.
- **Consequences**: Isolates concrete classes, but can make it difficult to support new kinds of products.
- **Sample Code**:

  ```java
  interface AbstractFactory {
      AbstractProduct createProduct();
  }
  
  class ConcreteFactory implements AbstractFactory {
      public AbstractProduct createProduct() {
          return new ConcreteProduct();
      }
  }
  
  interface AbstractProduct {}
  
  class ConcreteProduct implements AbstractProduct {}
  ```

#### 4. Builder

- **Category**: Creational
- **Intent**: Separate the construction of a complex object from its representation so that the same construction process can create different representations.
- **Participants**:
  - Builder: Specifies an abstract interface for creating parts of a Product object.
  - ConcreteBuilder: Constructs and assembles parts of the product by implementing the Builder interface.
  - Director: Constructs an object using the Builder interface.
  - Product: Represents the complex object under construction.
- **Collaborations**: The client creates a Director object and configures it with a Builder object.
- **Consequences**: Provides control over the construction process, but requires a separate ConcreteBuilder for each type of product.
- **Sample Code**:

  ```java
  class Product {
      private String partA;
      private String partB;
      
      public void setPartA(String partA) { this.partA = partA; }
      public void setPartB(String partB) { this.partB = partB; }
  }
  
  abstract class Builder {
      protected Product product = new Product();
      public abstract void buildPartA();
      public abstract void buildPartB();
      public Product getResult() { return product; }
  }
  
  class ConcreteBuilder extends Builder {
      public void buildPartA() { product.setPartA("Part A"); }
      public void buildPartB() { product.setPartB("Part B"); }
  }
  
  class Director {
      private Builder builder;
      public Director(Builder builder) { this.builder = builder; }
      public void construct() {
          builder.buildPartA();
          builder.buildPartB();
      }
  }
  ```

#### 5. Prototype

- **Category**: Creational
- **Intent**: Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype.
- **Participants**:
  - Prototype: Declares an interface for cloning itself.
  - ConcretePrototype: Implements the operation for cloning itself.
- **Collaborations**: A client asks a prototype to clone itself.
- **Consequences**: Adds flexibility in terms of object creation, but can be complex to implement with deep copies.
- **Sample Code**:

  ```java
  interface Prototype {
      Prototype clone();
  }
  
  class ConcretePrototype implements Prototype {
      public Prototype clone() {
          return new ConcretePrototype();
      }
  }
  ```

### Structural Patterns

Structural patterns concern class and object composition. They use inheritance to compose interfaces and define ways to compose objects to obtain new functionality.

#### 6. Adapter

- **Category**: Structural
- **Intent**: Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
- **Participants**:
  - Target: Defines the domain-specific interface that Client uses.
  - Adapter: Adapts the interface of Adaptee to the Target interface.
  - Adaptee: Defines an existing interface that needs adapting.
- **Collaborations**: The client calls operations on an Adapter instance. In turn, the adapter calls Adaptee operations that carry out the request.
- **Consequences**: Allows classes with incompatible interfaces to work together, but can introduce additional complexity.
- **Sample Code**:

  ```java
  interface Target {
      void request();
  }
  
  class Adaptee {
      public void specificRequest() {
          System.out.println("Specific request");
      }
  }
  
  class Adapter implements Target {
      private Adaptee adaptee;
      
      public Adapter(Adaptee adaptee) {
          this.adaptee = adaptee;
      }
      
      public void request() {
          adaptee.specificRequest();
      }
  }
  ```

#### 7. Bridge

- **Category**: Structural
- **Intent**: Decouple an abstraction from its implementation so that the two can vary independently.
- **Participants**:
  - Abstraction: Defines the abstraction's interface.
  - RefinedAbstraction: Extends the interface defined by Abstraction.
  - Implementor: Defines the interface for implementation classes.
  - ConcreteImplementor: Implements the Implementor interface.
- **Collaborations**: Abstraction forwards client requests to its Implementor object.
- **Consequences**: Decouples interface and implementation, but increases complexity.
- **Sample Code**:

  ```java
  interface Implementor {
      void operationImpl();
  }
  
  class ConcreteImplementorA implements Implementor {
      public void operationImpl() {
          System.out.println("ConcreteImplementorA operation");
      }
  }
  
  abstract class Abstraction {
      protected Implementor implementor;
      
      protected Abstraction(Implementor implementor) {
          this.implementor = implementor;
      }
      
      public abstract void operation();
  }
  
  class RefinedAbstraction extends Abstraction {
      public RefinedAbstraction(Implementor implementor) {
          super(implementor);
      }
      
      public void operation() {
          implementor.operationImpl();
      }
  }
  ```

#### 8. Composite

- **Category**: Structural
- **Intent**: Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.
- **Participants**:
  - Component: Declares the interface for objects in the composition.
  - Leaf: Represents leaf objects in the composition.
  - Composite: Defines behavior for components having children.
- **Collaborations**: Clients use the Component interface to interact with objects in the composition.
- **Consequences**: Simplifies client code, but can make the design overly general.
- **Sample Code**:

  ```java
  interface Component {
      void operation();
  }
  
  class Leaf implements Component {
      public void operation() {
          System.out.println("Leaf operation");
      }
  }
  
  class Composite implements Component {
      private List<Component> children = new ArrayList<>();
      
      public void add(Component component) {
          children.add(component);
      }
      
      public void operation() {
          for (Component child : children) {
              child.operation();
          }
      }
  }
  ```

#### 9. Decorator

- **Category**: Structural
- **Intent**: Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.
- **Participants**:
  - Component: Defines the interface for objects that can have responsibilities added to them.
  - ConcreteComponent: Defines an object to which additional responsibilities can be attached.
  - Decorator: Maintains a reference to a Component object and defines an interface that conforms to Component's interface.
  - ConcreteDecorator: Adds responsibilities to the component.
- **Collaborations**: Decorators forward requests to their Component and can perform additional actions before or after forwarding.
- **Consequences**: More flexible than inheritance, but can result in many small objects.
- **Sample Code**:

  ```java
  interface Component {
      void operation();
  }
  
  class ConcreteComponent implements Component {
      public void operation() {
          System.out.println("ConcreteComponent operation");
      }
  }
  
  abstract class Decorator implements Component {
      protected Component component;
      
      public Decorator(Component component) {
          this.component = component;
      }
      
      public void operation() {
          component.operation();
      }
  }
  
  class ConcreteDecorator extends Decorator {
      public ConcreteDecorator(Component component) {
          super(component);
      }
      
      public void operation() {
          super.operation();
          addedBehavior();
      }
      
      private void addedBehavior() {
          System.out.println("Added behavior");
      }
  }
  ```

#### 10. Facade

- **Category**: Structural
- **Intent**: Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.
- **Participants**:
  - Facade: Knows which subsystem classes are responsible for a request.
  - Subsystem classes: Implement subsystem functionality.
- **Collaborations**: Clients communicate with the subsystem through the Facade.
- **Consequences**: Simplifies use of the subsystem, but can hide important functionality.
- **Sample Code**:

  ```java
  class SubsystemA {
      public void operationA() {
          System.out.println("SubsystemA operation");
      }
  }
  
  class SubsystemB {
      public void operationB() {
          System.out.println("SubsystemB operation");
      }
  }
  
  class Facade {
      private SubsystemA subsystemA = new SubsystemA();
      private SubsystemB subsystemB = new SubsystemB();
      
      public void operation() {
          subsystemA.operationA();
          subsystemB.operationB();
      }
  }
  ```

#### 11. Flyweight

- **Category**: Structural
- **Intent**: Use sharing to support large numbers of fine-grained objects efficiently.
- **Participants**:
  - Flyweight: Declares an interface through which flyweights can receive and act on extrinsic state.
  - ConcreteFlyweight: Implements the Flyweight interface and adds storage for intrinsic state.
  - FlyweightFactory: Creates and manages flyweight objects.
- **Collaborations**: Clients should not instantiate ConcreteFlyweights directly.
- **Consequences**: Reduces the number of objects, but increases complexity.
- **Sample Code**:

  ```java
  interface Flyweight {
      void operation(String extrinsicState);
  }
  
  class ConcreteFlyweight implements Flyweight {
      private String intrinsicState;
      
      public ConcreteFlyweight(String intrinsicState) {
          this.intrinsicState = intrinsicState;
      }
      
      public void operation(String extrinsicState) {
          System.out.println("Intrinsic: " + intrinsicState + ", Extrinsic: " + extrinsicState);
      }
  }
  
  class FlyweightFactory {
      private Map<String, Flyweight> flyweights = new HashMap<>();
      
      public Flyweight getFlyweight(String key) {
          if (!flyweights.containsKey(key)) {
              flyweights.put(key, new ConcreteFlyweight(key));
          }
          return flyweights.get(key);
      }
  }
  ```

#### 12. Proxy

- **Category**: Structural
- **Intent**: Provide a surrogate or placeholder for another object to control access to it.
- **Participants**:
  - Proxy: Maintains a reference to the real subject and provides an interface identical to the Subject's.
  - RealSubject: Defines the real object that the proxy represents.
- **Collaborations**: Proxy forwards requests to RealSubject.
- **Consequences**: Adds a level of indirection, but can introduce additional complexity.
- **Sample Code**:

  ```java
  interface Subject {
      void request();
  }
  
  class RealSubject implements Subject {
      public void request() {
          System.out.println("RealSubject request");
      }
  }
  
  class Proxy implements Subject {
      private RealSubject realSubject;
      
      public void request() {
          if (realSubject == null) {
              realSubject = new RealSubject();
          }
          realSubject.request();
      }
  }
  ```

### Behavioral Patterns

Behavioral patterns are concerned with algorithms and the assignment of responsibilities between objects.

#### 13. Chain of Responsibility

- **Category**: Behavioral
- **Intent**: Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it.
- **Participants**:
  - Handler: Defines an interface for handling requests.
  - ConcreteHandler: Handles requests it is responsible for.
- **Collaborations**: Each handler in the chain either handles the request or passes it to the next handler.
- **Consequences**: Reduces coupling, but can result in unhandled requests.
- **Sample Code**:

  ```java
  abstract class Handler {
      protected Handler successor;
      
      public void setSuccessor(Handler successor) {
          this.successor = successor;
      }
      
      public abstract void handleRequest(String request);
  }
  
  class ConcreteHandlerA extends Handler {
      public void handleRequest(String request) {
          if (request.equals("A")) {
              System.out.println("Handled by ConcreteHandlerA");
          } else if (successor != null) {
              successor.handleRequest(request);
          }
      }
  }
  
  class ConcreteHandlerB extends Handler {
      public void handleRequest(String request) {
          if (request.equals("B")) {
              System.out.println("Handled by ConcreteHandlerB");
          } else if (successor != null) {
              successor.handleRequest(request);
          }
      }
  }
  ```

#### 14. Command

- **Category**: Behavioral
- **Intent**: Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.
- **Participants**:
  - Command: Declares an interface for executing an operation.
  - ConcreteCommand: Defines a binding between a Receiver object and an action.
  - Invoker: Asks the command to carry out the request.
  - Receiver: Knows how to perform the operations associated with carrying out the request.
- **Collaborations**: The invoker holds a command and invokes its execute method.
- **Consequences**: Decouples the object that invokes the operation from the one that knows how to perform it, but can result in many command classes.
- **Sample Code**:

  ```java
  interface Command {
      void execute();
  }
  
  class ConcreteCommand implements Command {
      private Receiver receiver;
      
      public ConcreteCommand(Receiver receiver) {
          this.receiver = receiver;
      }
      
      public void execute() {
          receiver.action();
      }
  }
  
  class Receiver {
      public void action() {
          System.out.println("Receiver action");
      }
  }
  
  class Invoker {
      private Command command;
      
      public void setCommand(Command command) {
          this.command = command;
      }
      
      public void invoke() {
          command.execute();
      }
  }
  ```

#### 15. Interpreter

- **Category**: Behavioral
- **Intent**: Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language.
- **Participants**:
  - AbstractExpression: Declares an interface for interpreting operations.
  - TerminalExpression: Implements an interpret operation associated with terminal symbols.
  - NonTerminalExpression: Represents non-terminal expressions.
- **Collaborations**: The context is passed to each expression to interpret.
- **Consequences**: Easy to change and extend the grammar, but complex grammars are hard to manage.
- **Sample Code**:

  ```java
  interface Expression {
      boolean interpret(String context);
  }
  
  class TerminalExpression implements Expression {
      private String data;
      
      public TerminalExpression(String data) {
          this.data = data;
      }
      
      public boolean interpret(String context) {
          return context.contains(data);
      }
  }
  
  class OrExpression implements Expression {
      private Expression expr1;
      private Expression expr2;
      
      public OrExpression(Expression expr1, Expression expr2) {
          this.expr1 = expr1;
          this.expr2 = expr2;
      }
      
      public boolean interpret(String context) {
          return expr1.interpret(context) || expr2.interpret(context);
      }
  }
  ```

#### 16. Iterator

- **Category**: Behavioral
- **Intent**: Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
- **Participants**:
  - Iterator: Defines an interface for accessing and traversing elements.
  - ConcreteIterator: Implements the Iterator interface.
  - Aggregate: Defines an interface for creating an Iterator object.
  - ConcreteAggregate: Implements the Aggregate interface.
- **Collaborations**: The client uses the iterator to access and traverse elements.
- **Consequences**: Supports variations in traversal, but can be cumbersome for complex collections.
- **Sample Code**:

  ```java
  interface Iterator {
      boolean hasNext();
      Object next();
  }
  
  class ConcreteIterator implements Iterator {
      private List<Object> list;
      private int position = 0;
      
      public ConcreteIterator(List<Object> list) {
          this.list = list;
      }
      
      public boolean hasNext() {
          return position < list.size();
      }
      
      public Object next() {
          return list.get(position++);
      }
  }
  
  interface Aggregate {
      Iterator createIterator();
  }
  
  class ConcreteAggregate implements Aggregate {
      private List<Object> items = new ArrayList<>();
      
      public void addItem(Object item) {
          items.add(item);
      }
      
      public Iterator createIterator() {
          return new ConcreteIterator(items);
      }
  }
  ```

#### 17. Mediator

- **Category**: Behavioral
- **Intent**: Define an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly.
- **Participants**:
  - Mediator: Defines an interface for communicating with Colleague objects.
  - ConcreteMediator: Implements cooperative behavior by coordinating Colleague objects.
  - Colleague: Each Colleague communicates with its Mediator.
- **Collaborations**: Colleagues send and receive requests from a Mediator.
- **Consequences**: Reduces the number of communication paths, but can centralize control.
- **Sample Code**:

  ```java
  interface Mediator {
      void send(String message, Colleague colleague);
  }
  
  abstract class Colleague {
      protected Mediator mediator;
      
      public Colleague(Mediator mediator) {
          this.mediator = mediator;
      }
  }
  
  class ConcreteColleagueA extends Colleague {
      public ConcreteColleagueA(Mediator mediator) {
          super(mediator);
      }
      
      public void send(String message) {
          mediator.send(message, this);
      }
      
      public void receive(String message) {
          System.out.println("ColleagueA received: " + message);
      }
  }
  
  class ConcreteMediator implements Mediator {
      private ConcreteColleagueA colleagueA;
      private ConcreteColleagueB colleagueB;
      
      public void setColleagueA(ConcreteColleagueA colleagueA) {
          this.colleagueA = colleagueA;
      }
      
      public void setColleagueB(ConcreteColleagueB colleagueB) {
          this.colleagueB = colleagueB;
      }
      
      public void send(String message, Colleague colleague) {
          if (colleague == colleagueA) {
              colleagueB.receive(message);
          } else {
              colleagueA.receive(message);
          }
      }
  }
  
  class ConcreteColleagueB extends Colleague {
      public ConcreteColleagueB(Mediator mediator) {
          super(mediator);
      }
      
      public void send(String message) {
          mediator.send(message, this);
      }
      
      public void receive(String message) {
          System.out.println("ColleagueB received: " + message);
      }
  }
  ```

#### 18. Memento

- **Category**: Behavioral
- **Intent**: Without violating encapsulation, capture and externalize an object's internal state so that the object can be restored to this state later.
- **Participants**:
  - Memento: Stores internal state of the Originator object.
  - Originator: Creates a memento containing a snapshot of its current internal state.
  - Caretaker: Responsible for the memento's safekeeping.
- **Collaborations**: The caretaker requests a memento from the originator, holds it, and passes it back to the originator to restore its state.
- **Consequences**: Preserves encapsulation boundaries, but can be costly in terms of memory.
- **Sample Code**:

  ```java
  class Memento {
      private String state;
      
      public Memento(String state) {
          this.state = state;
      }
      
      public String getState() {
          return state;
      }
  }
  
  class Originator {
      private String state;
      
      public void setState(String state) {
          this.state = state;
      }
      
      public Memento saveStateToMemento() {
          return new Memento(state);
      }
      
      public void getStateFromMemento(Memento memento) {
          state = memento.getState();
      }
  }
  
  class Caretaker {
      private List<Memento> mementoList = new ArrayList<>();
      
      public void add(Memento state) {
          mementoList.add(state);
      }
      
      public Memento get(int index) {
          return mementoList.get(index);
      }
  }
  ```

#### 19. Observer

- **Category**: Behavioral
- **Intent**: Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
- **Participants**:
  - Subject: Knows its observers and provides an interface for attaching and detaching Observer objects.
  - Observer: Defines an updating interface for objects that should be notified of changes in a Subject.
  - ConcreteSubject: Stores state of interest to ConcreteObserver objects.
  - ConcreteObserver: Implements the Observer updating interface to keep its state consistent with the subject's.
- **Collaborations**: ConcreteSubject notifies its observers whenever a change occurs.
- **Consequences**: Promotes loose coupling, but can lead to unexpected updates.
- **Sample Code**:

  ```java
  interface Observer {
      void update(String state);
  }
  
  class ConcreteObserver implements Observer {
      private String observerState;
      
      public void update(String state) {
          observerState = state;
          System.out.println("Observer state updated to: " + observerState);
      }
  }
  
  class Subject {
      private List<Observer> observers = new ArrayList<>();
      private String state;
      
      public void attach(Observer observer) {
          observers.add(observer);
      }
      
      public void detach(Observer observer) {
          observers.remove(observer);
      }
      
      public void setState(String state) {
          this.state = state;
          notifyObservers();
      }
      
      private void notifyObservers() {
          for (Observer observer : observers) {
              observer.update(state);
          }
      }
  }
  ```

#### 20. State

- **Category**: Behavioral
- **Intent**: Allow an object to alter its behavior when its internal state changes. The object will appear to change its class.
- **Participants**:
  - Context: Maintains an instance of a ConcreteState subclass that defines the current state.
  - State: Defines an interface for encapsulating the behavior associated with a particular state.
  - ConcreteState: Each subclass implements a behavior associated with a state of the Context.
- **Collaborations**: Context delegates state-specific behavior to its current State object.
- **Consequences**: Localizes state-specific behavior, but can result in many subclasses.
- **Sample Code**:

  ```java
  interface State {
      void handle(Context context);
  }
  
  class ConcreteStateA implements State {
      public void handle(Context context) {
          System.out.println("State A handling request.");
          context.setState(new ConcreteStateB());
      }
  }
  
  class ConcreteStateB implements State {
      public void handle(Context context) {
          System.out.println("State B handling request.");
          context.setState(new ConcreteStateA());
      }
  }
  
  class Context {
      private State state;
      
      public Context(State state) {
          this.state = state;
      }
      
      public void setState(State state) {
          this.state = state;
      }
      
      public void request() {
          state.handle(this);
      }
  }
  ```

#### 21. Strategy

- **Category**: Behavioral
- **Intent**: Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.
- **Participants**:
  - Strategy: Declares an interface common to all supported algorithms.
  - ConcreteStrategy: Implements the algorithm using the Strategy interface.
  - Context: Maintains a reference to a Strategy object.
- **Collaborations**: Context forwards requests to its Strategy object.
- **Consequences**: Provides flexibility in choosing algorithms, but can increase the number of objects.
- **Sample Code**:

  ```java
  interface Strategy {
      int execute(int a, int b);
  }
  
  class ConcreteStrategyAdd implements Strategy {
      public int execute(int a, int b) {
          return a + b;
      }
  }
  
  class ConcreteStrategySubtract implements Strategy {
      public int execute(int a, int b) {
          return a - b;
      }
  }
  
  class Context {
      private Strategy strategy;
      
      public void setStrategy(Strategy strategy) {
          this.strategy = strategy;
      }
      
      public int executeStrategy(int a, int b) {
          return strategy.execute(a, b);
      }
  }
  ```

#### 22. Template Method

- **Category**: Behavioral
- **Intent**: Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.
- **Participants**:
  - AbstractClass: Defines abstract primitive operations that concrete subclasses define to implement steps of an algorithm.
  - ConcreteClass: Implements the primitive operations to carry out subclass-specific steps of the algorithm.
- **Collaborations**: ConcreteClass relies on AbstractClass to implement the invariant steps of the algorithm.
- **Consequences**: Promotes code reuse, but can lead to a rigid class hierarchy.
- **Sample Code**:

  ```java
  abstract class AbstractClass {
      public final void templateMethod() {
          primitiveOperation1();
          primitiveOperation2();
      }
      
      protected abstract void primitiveOperation1();
      protected abstract void primitiveOperation2();
  }
  
  class ConcreteClass extends AbstractClass {
      protected void primitiveOperation1() {
          System.out.println("ConcreteClass operation1");
      }
      
      protected void primitiveOperation2() {
          System.out.println("ConcreteClass operation2");
      }
  }
  ```

#### 23. Visitor

- **Category**: Behavioral
- **Intent**: Represent an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.
- **Participants**:
  - Visitor: Declares a Visit operation for each class of ConcreteElement in the object structure.
  - ConcreteVisitor: Implements each operation declared by Visitor.
  - Element: Defines an Accept operation that takes a visitor as an argument.
  - ConcreteElement: Implements an Accept operation that takes a visitor as an argument.
- **Collaborations**: ConcreteElement calls the Visitor operation that corresponds to its class.
- **Consequences**: Makes adding new operations easy, but can make adding new ConcreteElement classes difficult.
- **Sample Code**:

  ```java
  interface Visitor {
      void visit(ConcreteElementA element);
      void visit(ConcreteElementB element);
  }
  
  class ConcreteVisitor implements Visitor {
      public void visit(ConcreteElementA element) {
          System.out.println("Visited ConcreteElementA");
      }
      
      public void visit(ConcreteElementB element) {
          System.out.println("Visited ConcreteElementB");
      }
  }
  
  interface Element {
      void accept(Visitor visitor);
  }
  
  class ConcreteElementA implements Element {
      public void accept(Visitor visitor) {
          visitor.visit(this);
      }
  }
  
  class ConcreteElementB implements Element {
      public void accept(Visitor visitor) {
          visitor.visit(this);
      }
  }
  ```

### Conclusion

This summary of the Gang of Four design patterns provides a quick reference to the essential characteristics of each pattern. For a deeper understanding, readers are encouraged to refer to the main chapters of this book, where each pattern is explored in detail with comprehensive examples and real-world applications.

## Quiz Time!

{{< quizdown >}}

### Which pattern ensures a class has only one instance?

- [x] Singleton
- [ ] Factory Method
- [ ] Prototype
- [ ] Builder

> **Explanation:** The Singleton pattern ensures a class has only one instance and provides a global point of access to it.

### What is the main intent of the Adapter pattern?

- [x] Convert the interface of a class into another interface clients expect
- [ ] Compose objects into tree structures
- [ ] Attach additional responsibilities to an object dynamically
- [ ] Provide a way to access elements of an aggregate object sequentially

> **Explanation:** The Adapter pattern converts the interface of a class into another interface clients expect, allowing incompatible interfaces to work together.

### Which pattern is used to define a family of algorithms and make them interchangeable?

- [ ] Observer
- [ ] Template Method
- [x] Strategy
- [ ] Visitor

> **Explanation:** The Strategy pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable.

### In which pattern do participants include Subject and Observer?

- [x] Observer
- [ ] Mediator
- [ ] Memento
- [ ] Command

> **Explanation:** The Observer pattern involves a Subject that maintains a list of Observers and notifies them of any state changes.

### What is the primary benefit of the Composite pattern?

- [x] Treats individual objects and compositions uniformly
- [ ] Provides a surrogate or placeholder for another object
- [ ] Decouples an abstraction from its implementation
- [ ] Encapsulates a request as an object

> **Explanation:** The Composite pattern allows clients to treat individual objects and compositions of objects uniformly.

### Which pattern involves a Caretaker, Originator, and Memento?

- [x] Memento
- [ ] Mediator
- [ ] Command
- [ ] Chain of Responsibility

> **Explanation:** The Memento pattern involves a Caretaker that manages the Memento objects created by the Originator.

### What is the main purpose of the Proxy pattern?

- [x] Control access to another object
- [ ] Simplify a complex subsystem
- [ ] Provide a way to traverse elements
- [ ] Define a one-to-many dependency

> **Explanation:** The Proxy pattern provides a surrogate or placeholder for another object to control access to it.

### Which pattern is characterized by the use of a ConcreteVisitor and ConcreteElement?

- [ ] Strategy
- [ ] State
- [x] Visitor
- [ ] Template Method

> **Explanation:** The Visitor pattern involves ConcreteVisitor and ConcreteElement classes to perform operations on object structures.

### What is the primary consequence of the Decorator pattern?

- [x] Allows additional responsibilities to be added to an object dynamically
- [ ] Simplifies the use of a subsystem
- [ ] Supports variations in traversal
- [ ] Provides a way to access elements of an aggregate object sequentially

> **Explanation:** The Decorator pattern allows additional responsibilities to be added to an object dynamically.

### True or False: The Factory Method pattern requires subclassing to instantiate a particular class.

- [x] True
- [ ] False

> **Explanation:** The Factory Method pattern allows subclasses to alter the type of objects that will be created, often requiring subclassing.

{{< /quizdown >}}
