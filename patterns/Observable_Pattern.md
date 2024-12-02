# Observer Pattern

The **Observer Pattern** is a behavioral design pattern used to define a one-to-many relationship between objects. In this pattern, an object (the **Observable** or **Subject**) maintains a list of its dependents (the **Observers**), and notifies them automatically of any changes to its state.

## Key Concepts

1. **Observable (Subject)**:
   - The object whose state changes and needs to notify others about it.
   - It manages a list of observers and notifies them when a change in state occurs.
   - Example: `Order` in an e-commerce system, where the status of an order changes (e.g., from "Processing" to "Shipped").

2. **Observer**:
   - The entity that listens to the observable for state changes and reacts to them.
   - It registers itself with the observable to receive notifications.
   - Example: `Customer` in an e-commerce system, where they are notified of order status changes.

## How It Works

- **Observable (Subject)** has a list of **Observers** that are interested in state changes.
- When the **Observable**'s state changes (like a change in order status), it triggers an update by calling the `update()` method of all registered **Observers**.
- The **Observers** react to the update, performing actions such as displaying new information, sending notifications, etc.

## UML Diagram

```plaintext
        +---------------------+                 +-------------------+
        |     Observable       |<>-------------->|     Observer      |
        +---------------------+                 +-------------------+
        | - state             |                 | + update(state)   |
        | - observers[]       |                 +-------------------+
        +---------------------+                 |                   |
        | + addObserver()     |                 |                   |
        | + removeObserver()  |                 |                   |
        | + notifyObservers() |                 |                   |
        +---------------------+                 +-------------------+
                   ^
                   |
                   |
        +---------------------+    
        |  ConcreteObserver   |    
        +---------------------+    
        | + update(state)     |    
        +---------------------+    

```
import java.util.ArrayList;
import java.util.List;

// Observable (Subject)
class Order {
    private List<Observer> observers = new ArrayList<>();
    private String status;

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    public void setStatus(String status) {
        this.status = status;
        notifyObservers();
    }

    public String getStatus() {
        return status;
    }

    private void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(this.status);
        }
    }
}

// Observer
interface Observer {
    void update(String status);
}

// Concrete Observer
class Customer implements Observer {
    private String name;

    public Customer(String name) {
        this.name = name;
    }

    @Override
    public void update(String status) {
        System.out.println(name + " notified of order status: " + status);
    }
}

// Client Code
public class Main {
    public static void main(String[] args) {
        Order order = new Order();
        
        Customer customer1 = new Customer("John");
        Customer customer2 = new Customer("Jane");

        order.addObserver(customer1);
        order.addObserver(customer2);

        order.setStatus("Shipped");
        order.setStatus("Delivered");
    }
}


## Advantages and Disadvantages of the Observer Pattern

### Advantages

1. **Loose Coupling**: 
   - The observable does not need to know about the concrete observers, only that they implement the `Observer` interface. This reduces direct dependencies between the objects, promoting flexibility and reusability.

2. **Scalability**:
   - New observers can be added or removed without changing the observable. This makes the system highly extensible and easy to scale as new functionalities can be added without affecting existing ones.

3. **Multiple Observers**:
   - Multiple observers can listen to a single observable, allowing many components of a system to respond to the same state change in different ways.

4. **Event-Driven**:
   - It provides an event-driven approach to communication. Observers are only notified when there is a state change, rather than having to poll the observable, which can improve performance and reduce unnecessary checks.

5. **Decoupling of Components**:
   - The observer pattern promotes a clean separation of concerns by decoupling the subject (observable) from the observers. This leads to cleaner, easier-to-maintain code.

---

### Disadvantages

1. **Memory Usage**:
   - The observable maintains a list of observers, which can increase memory usage, especially when the number of observers is large. In large-scale systems, managing this collection can become resource-intensive.

2. **Unwanted Notifications**:
   - If the observer is not careful about filtering which state changes it reacts to, it could be notified of changes that it’s not interested in, leading to unnecessary processing or updates.

3. **Difficulty in Managing Dependencies**:
   - If there are many interdependencies between observers and the observable, it may become difficult to manage the state flow or understand the impact of each state change, potentially causing bugs or unintended behavior.

4. **Performance Issues**:
   - In cases where an observable has many observers, each state change could lead to multiple updates, which might affect performance. Handling a large number of observers efficiently can become challenging.

5. **Complexity in Synchronization**:
   - In multi-threaded applications, ensuring that updates to observers are synchronized properly can be complex. If multiple observers are updated concurrently, race conditions or inconsistent states can occur.

