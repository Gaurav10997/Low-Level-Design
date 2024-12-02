
# Decorator Design Pattern

## Overview
The **Decorator Pattern** is a structural design pattern that allows you to add new functionality to an object **dynamically** without altering its structure. This pattern is ideal for extending an object’s behavior in a flexible and reusable way, often used when subclassing is not practical.

### Key Concepts:
1. **Base Component**:
   - This is the object whose behavior you want to enhance. It typically defines the core functionality (e.g., a `Pizza` class).
2. **Decorator**:
   - A decorator class wraps the base component and adds new functionality to it, either extending or modifying its behavior. Each decorator typically implements the same interface as the base class.
3. **Flexibility**:
   - You can combine multiple decorators at runtime to extend the base functionality in different ways.

### Real-World Example (Pizza):
Think of a **pizza** as the base object. It can have multiple types of crusts (base functionality), but almost all pizzas have extra toppings. Using the **Decorator Pattern**, we can dynamically add toppings without hardcoding them into the pizza class.

### Benefits:
- **Avoids Hardcoding**: The decorator allows the flexibility to add features (like toppings) without modifying the base class (pizza).
- **Dynamic Behavior**: Multiple decorators can be combined to create customized behavior for an object at runtime.

## Example Code in Java

### Base Pizza Interface:
```java
public interface Pizza {
    String getDescription();
    double cost();
}
```

### Concrete Pizza (Base):
```java
public class PlainPizza implements Pizza {
    public String getDescription() {
        return "Plain Pizza";
    }
    
    public double cost() {
        return 5.00;
    }
}
```

### Cheese Decorator:
```java
public class CheeseDecorator implements Pizza {
    private Pizza pizza;

    public CheeseDecorator(Pizza pizza) {
        this.pizza = pizza;
    }

    public String getDescription() {
        return pizza.getDescription() + ", Cheese";
    }

    public double cost() {
        return pizza.cost() + 1.50;
    }
}
```

### Olive Decorator:
```java
public class OliveDecorator implements Pizza {
    private Pizza pizza;

    public OliveDecorator(Pizza pizza) {
        this.pizza = pizza;
    }

    public String getDescription() {
        return pizza.getDescription() + ", Olives";
    }

    public double cost() {
        return pizza.cost() + 2.00;
    }
}
```

### Client Code:
```java
public class DecoratorPatternDemo {
    public static void main(String[] args) {
        Pizza pizza = new PlainPizza();  // Base pizza
        pizza = new CheeseDecorator(pizza);  // Adding Cheese
        pizza = new OliveDecorator(pizza);  // Adding Olives
        
        System.out.println(pizza.getDescription());  // "Plain Pizza, Cheese, Olives"
        System.out.println("Cost: $" + pizza.cost());  // Cost: $8.50
    }
}
```

### Key Points:
- **Base Component** (like `PlainPizza`) defines the core functionality.
- **Decorators** (like `CheeseDecorator` and `OliveDecorator`) add new features dynamically without modifying the base class.
- **Multiple decorators** can be combined at runtime to create a customized object.
- The **Decorator Pattern** promotes **composition** over inheritance, allowing for more flexible and reusable code.

---

## Conclusion:
The **Decorator Pattern** allows you to extend the functionality of an object in a modular way, making it more flexible than using inheritance alone. It's especially useful when you want to add or remove features dynamically without changing the original class's structure.

