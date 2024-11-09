---

linkTitle: "4.4.4 Example: Custom Collection Classes"
title: "Custom Collection Classes in Java: Implementing Iterators for Enhanced Usability"
description: "Explore the creation of custom collection classes in Java, such as SkipList, and learn how to implement iterators for efficient and flexible traversal."
categories:
- Java Programming
- Design Patterns
- Software Development
tags:
- Java
- Design Patterns
- Iterator Pattern
- Custom Collections
- SkipList
date: 2024-10-25
type: docs
nav_weight: 4440

---

## 4.4.4 Example: Custom Collection Classes

In this section, we delve into the practical application of the Iterator Pattern by creating a custom collection class in Java. We will focus on a `SkipList`, a probabilistic data structure that allows for fast search, insertion, and deletion operations. We'll implement an iterator to traverse this collection, providing an example of how to integrate custom collections with Java's iteration mechanisms.

### Creating a Custom Collection Class: SkipList

A SkipList is a layered data structure that allows for efficient search operations. It consists of multiple levels of linked lists, where each level skips over some elements, allowing for faster traversal.

#### Defining the SkipList Class

Let's start by defining the basic structure of our `SkipList` class. We'll include methods for adding, removing, and searching for elements.

```java
import java.util.Random;

public class SkipList<T extends Comparable<T>> {
    private static final int MAX_LEVEL = 16;
    private final Node<T> head = new Node<>(null, MAX_LEVEL);
    private final Random random = new Random();
    private int level = 0;

    private static class Node<T> {
        T value;
        Node<T>[] forward;

        @SuppressWarnings("unchecked")
        public Node(T value, int level) {
            this.value = value;
            this.forward = new Node[level + 1];
        }
    }

    public void insert(T value) {
        Node<T>[] update = new Node[MAX_LEVEL + 1];
        Node<T> current = head;

        for (int i = level; i >= 0; i--) {
            while (current.forward[i] != null && current.forward[i].value.compareTo(value) < 0) {
                current = current.forward[i];
            }
            update[i] = current;
        }

        current = current.forward[0];

        if (current == null || !current.value.equals(value)) {
            int newLevel = randomLevel();
            if (newLevel > level) {
                for (int i = level + 1; i <= newLevel; i++) {
                    update[i] = head;
                }
                level = newLevel;
            }

            Node<T> newNode = new Node<>(value, newLevel);
            for (int i = 0; i <= newLevel; i++) {
                newNode.forward[i] = update[i].forward[i];
                update[i].forward[i] = newNode;
            }
        }
    }

    private int randomLevel() {
        int lvl = 0;
        while (random.nextInt(2) == 0 && lvl < MAX_LEVEL) {
            lvl++;
        }
        return lvl;
    }

    // Additional methods for remove, search, etc.
}
```

### Implementing the Iterable Interface

To allow iteration over our `SkipList`, we need to implement the `Iterable` interface. This involves creating an internal `Iterator` class that can traverse the list.

#### Implementing the Iterator

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

public class SkipList<T extends Comparable<T>> implements Iterable<T> {
    // Existing SkipList code...

    @Override
    public Iterator<T> iterator() {
        return new SkipListIterator();
    }

    private class SkipListIterator implements Iterator<T> {
        private Node<T> current = head.forward[0];

        @Override
        public boolean hasNext() {
            return current != null;
        }

        @Override
        public T next() {
            if (!hasNext()) {
                throw new NoSuchElementException();
            }
            T value = current.value;
            current = current.forward[0];
            return value;
        }

        @Override
        public void remove() {
            throw new UnsupportedOperationException("Remove not supported.");
        }
    }
}
```

### Navigating the Collection's Structure

The `SkipListIterator` navigates through the elements of the `SkipList` by following the forward pointers at the lowest level. This ensures that all elements are visited in sorted order.

#### Handling Edge Cases

- **Empty Collections:** The `hasNext()` method will return `false` if the `SkipList` is empty, preventing any calls to `next()`.
- **End-of-Collection:** The `next()` method throws a `NoSuchElementException` when there are no more elements to iterate over, adhering to the standard iterator contract.

### Encapsulation and Performance Considerations

Encapsulation is crucial to ensure that the iterator does not expose the internal structure of the `SkipList`. By providing a controlled interface for iteration, we maintain the integrity of the data structure.

- **Efficiency:** The `next()` and `hasNext()` methods are efficient, operating in constant time, as they only involve pointer traversal.
- **Avoiding Exposure:** The iterator does not allow modification of the collection, preserving encapsulation and preventing unintended side effects.

### Using the Custom Collection and Iterator

Here's how a client might use the `SkipList` and its iterator:

```java
public class SkipListExample {
    public static void main(String[] args) {
        SkipList<Integer> skipList = new SkipList<>();
        skipList.insert(10);
        skipList.insert(20);
        skipList.insert(30);

        for (Integer value : skipList) {
            System.out.println(value);
        }
    }
}
```

### Implementing Additional Iterator Features

While the basic iterator implementation is sufficient for many use cases, additional features can enhance its functionality:

- **Removing Elements:** Implementing the `remove()` method can allow the iterator to modify the collection, though this requires careful consideration of concurrent modifications.
- **Filtering:** Custom iterators can support filtering by accepting predicates, allowing clients to iterate over a subset of elements.

### Testing Strategies

Testing the `SkipList` and its iterator involves:

- **Unit Tests:** Validate the correctness of insertion, deletion, and iteration.
- **Stress Testing:** Use large datasets to ensure performance and reliability.
- **Edge Cases:** Test empty lists, single-element lists, and boundary conditions.

### User Expectations and Standard Behaviors

When designing custom iterators, consider user expectations based on standard Java collections. Ensure consistency with behaviors such as throwing exceptions for invalid operations.

### Potential Extensions

- **Concurrent Iteration:** Implementing thread-safe iterators for concurrent environments.
- **Lambda Expressions:** Allowing custom traversal logic through lambda expressions, enhancing flexibility.

### Benefits of Iterator Support

Providing iterator support enhances the usability of custom collections, allowing them to integrate seamlessly with Java's collection framework and enabling idiomatic use in client code.

### Integrating with Java's Collections Framework

While `SkipList` is a standalone collection, it can be adapted to implement interfaces like `List` or `Set`, enabling broader compatibility with Java's collections framework.

## Conclusion

Creating a custom collection class like `SkipList` and implementing an iterator for it demonstrates the power and flexibility of the Iterator Pattern. By adhering to Java's iteration conventions, we enhance the usability and integration of our custom collections, providing developers with robust tools for data management.

## Quiz Time!

{{< quizdown >}}

### What is a SkipList?

- [x] A probabilistic data structure that allows fast search, insertion, and deletion operations.
- [ ] A type of binary tree used for sorting elements.
- [ ] A linked list with additional backward pointers.
- [ ] A stack-based data structure for LIFO operations.

> **Explanation:** A SkipList is a layered data structure that uses multiple levels of linked lists to allow efficient search, insertion, and deletion operations.

### What is the purpose of the `randomLevel()` method in the SkipList implementation?

- [x] To determine the level at which a new node should be inserted.
- [ ] To shuffle the elements in the SkipList.
- [ ] To remove nodes from the SkipList.
- [ ] To sort the elements in the SkipList.

> **Explanation:** The `randomLevel()` method determines the level at which a new node should be inserted, contributing to the probabilistic nature of the SkipList.

### Which interface must a custom collection implement to support iteration in Java?

- [x] Iterable
- [ ] Iterator
- [ ] Comparable
- [ ] Serializable

> **Explanation:** A custom collection must implement the `Iterable` interface to support iteration, allowing it to be used in enhanced for-loops and with other iteration mechanisms.

### What exception should be thrown by the `next()` method of an iterator when there are no more elements?

- [x] NoSuchElementException
- [ ] IllegalStateException
- [ ] UnsupportedOperationException
- [ ] NullPointerException

> **Explanation:** The `next()` method should throw a `NoSuchElementException` when there are no more elements to iterate over, as per the iterator contract.

### What is the main benefit of providing iterator support for custom collections?

- [x] Enhances usability and integration with Java's collection framework.
- [ ] Increases the memory usage of the collection.
- [ ] Allows direct access to the collection's internal structure.
- [ ] Makes the collection immutable.

> **Explanation:** Providing iterator support enhances the usability of custom collections, allowing them to integrate seamlessly with Java's collection framework and enabling idiomatic use in client code.

### How does the `SkipListIterator` navigate the SkipList?

- [x] By following the forward pointers at the lowest level.
- [ ] By traversing backward pointers.
- [ ] By randomly accessing elements.
- [ ] By using a stack to track visited nodes.

> **Explanation:** The `SkipListIterator` navigates the SkipList by following the forward pointers at the lowest level, ensuring all elements are visited in sorted order.

### What should be considered when implementing the `remove()` method in an iterator?

- [x] Concurrent modifications and ensuring the collection's integrity.
- [ ] Increasing the collection's size.
- [ ] Sorting the collection.
- [ ] Randomizing the order of elements.

> **Explanation:** Implementing the `remove()` method requires careful consideration of concurrent modifications to ensure the collection's integrity and prevent unintended side effects.

### What is a potential extension for custom iterators?

- [x] Supporting concurrent iteration.
- [ ] Allowing direct access to private fields.
- [ ] Disabling iteration for large datasets.
- [ ] Making the iterator static.

> **Explanation:** Supporting concurrent iteration is a potential extension for custom iterators, allowing them to be used safely in multi-threaded environments.

### What is a key performance consideration for iterator methods?

- [x] Ensuring `next()` and `hasNext()` operate in constant time.
- [ ] Ensuring `next()` and `hasNext()` operate in linear time.
- [ ] Minimizing the use of memory.
- [ ] Maximizing the number of elements returned.

> **Explanation:** Ensuring that `next()` and `hasNext()` operate in constant time is a key performance consideration, as it allows for efficient iteration over the collection.

### True or False: The `SkipList` can be adapted to implement interfaces like `List` or `Set`.

- [x] True
- [ ] False

> **Explanation:** True. The `SkipList` can be adapted to implement interfaces like `List` or `Set`, enabling broader compatibility with Java's collections framework.

{{< /quizdown >}}
