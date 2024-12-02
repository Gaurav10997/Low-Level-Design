
# Strategy Design Pattern

The **Strategy Design Pattern** is a behavioral design pattern that enables selecting an algorithm's behavior at runtime. It defines a family of algorithms, encapsulates each one, and makes them interchangeable. This promotes flexibility, code reusability, and adherence to the **Open/Closed Principle**.

---

## Key Benefits

### 1. **Open/Closed Principle**
- Easily add new strategies without modifying existing code.
- Example: Add a new payment method like "Google Pay" or a new travel mode like "Airplane" without altering the context class.

### 2. **Code Reusability**
- Avoid duplicating similar logic across multiple classes by encapsulating shared functionality in strategies.
- Example: Travel time calculations for Car, Bike, and Train can be reused independently.

### 3. **Dynamic Behavior**
- Change the behavior of an object dynamically at runtime by switching strategies.
- Example: A shopping cart can switch from "Credit Card" to "PayPal" payments seamlessly.

### 4. **Simplifies Complex Code**
- Extract specific algorithms into separate strategy classes, keeping the context class focused and clean.
- Example: The `TravelContext` class delegates travel time calculations without implementing them.

### 5. **Easier Maintenance**
- Each strategy resides in its own class, making it simpler to debug, update, or replace behaviors without affecting other parts of the system.
- Example: Updating the train speed calculation in `TrainStrategy` won’t affect `CarStrategy` or `BikeStrategy`.

### 6. **Promotes Loose Coupling**
- Decouples the context class from the strategy implementations, enabling modular and flexible design.
- Example: The `TravelContext` only needs the `TravelStrategy` interface and not the specifics of each strategy.

---

## Example: Travel Modes

Imagine an application where travel time needs to be calculated differently for Car, Bike, and Train.

### Implementation

#### Strategy Interface
```java
public interface TravelStrategy {
    int calculateTime(int distance);
}
```

#### Concrete Strategies
```java
public class CarStrategy implements TravelStrategy {
    public int calculateTime(int distance) {
        return distance / 60; // Assuming speed is 60 km/h
    }
}

public class BikeStrategy implements TravelStrategy {
    public int calculateTime(int distance) {
        return distance / 40; // Assuming speed is 40 km/h
    }
}

public class TrainStrategy implements TravelStrategy {
    public int calculateTime(int distance) {
        return distance / 100; // Assuming speed is 100 km/h
    }
}
```

#### Context Class
```java
public class TravelContext {
    private TravelStrategy strategy;

    public void setStrategy(TravelStrategy strategy) {
        this.strategy = strategy;
    }

    public int getTravelTime(int distance) {
        return strategy.calculateTime(distance);
    }
}
```

#### Usage
```java
public class Main {
    public static void main(String[] args) {
        TravelContext context = new TravelContext();

        context.setStrategy(new CarStrategy());
        System.out.println("Car Travel Time: " + context.getTravelTime(120)); // 2 hours

        context.setStrategy(new TrainStrategy());
        System.out.println("Train Travel Time: " + context.getTravelTime(120)); // 1.2 hours
    }
}
```

---

## Why Use the Strategy Pattern?

- **Flexibility**: Easily switch or add new strategies.
- **Maintainability**: Isolates changes to specific strategies.
- **Scalability**: Encourages adding new behaviors without modifying the core logic.

---

## When to Use

Use the Strategy Design Pattern when:
- Multiple classes share similar functionality with slight variations.
- You need to dynamically switch between different algorithms or behaviors.
- You want to avoid embedding multiple algorithms in a single class.

---

## Conclusion

The Strategy Design Pattern simplifies complex code, promotes flexibility, and ensures adherence to design principles, making it a powerful tool for solving behavioral problems in object-oriented programming.
