# Factory Design Pattern

## Overview
The **Factory Pattern** is a **creational design pattern** that provides an interface for creating objects, but allows subclasses to alter the type of objects that will be created. The client does not need to know the exact class of the object it is using, just the interface.

### Key Concepts:
1. **Creator (Factory)**:
   - A class that defines a method for creating products. It doesn’t instantiate the product directly but delegates the responsibility to subclasses or other classes.
2. **Product**:
   - The objects that are created by the factory. These objects typically share a common interface.
3. **Factory Method**:
   - A method within the factory class responsible for creating the product objects. The client code does not need to know the concrete class of the product.

### Types of Factory Patterns:
1. **Simple Factory**: A class with a method that creates objects based on input conditions.
2. **Factory Method**: A method in a class or interface that is implemented by subclasses to create the product.
3. **Abstract Factory**: A higher-level factory that provides an interface for creating families of related or dependent products.

### Benefits:
- **Separation of Concerns**: The client code doesn’t need to worry about the instantiation process of the objects.
- **Flexibility**: Different classes can create different types of products dynamically.
- **Extensibility**: New products can be added without modifying the existing client code.

## Example Code in Java

### Product Interface:
```java
// Product Interface
public interface Animal {
    void speak();
}

// Concrete Product: Dog
public class Dog implements Animal {
    @Override
    public void speak() {
        System.out.println("Woof!");
    }
}

// Concrete Product: Cat
public class Cat implements Animal {
    @Override
    public void speak() {
        System.out.println("Meow!");
    }
}

// Factory Class
public class AnimalFactory {
    public Animal getAnimal(String type) {
        if (type == null) {
            return null;
        }
        if (type.equalsIgnoreCase("Dog")) {
            return new Dog();
        } else if (type.equalsIgnoreCase("Cat")) {
            return new Cat();
        }
        return null;
    }
}


// Client Code
public class FactoryPatternDemo {
    public static void main(String[] args) {
        AnimalFactory animalFactory = new AnimalFactory();

        // Create Dog
        Animal dog = animalFactory.getAnimal("Dog");
        dog.speak();  // Woof!

        // Create Cat
        Animal cat = animalFactory.getAnimal("Cat");
        cat.speak();  // Meow!
    }
}
