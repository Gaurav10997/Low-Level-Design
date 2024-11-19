
# **Liskov Substitution Principle (LSP)**

The **Liskov Substitution Principle** is the "L" in the SOLID principles of object-oriented design. It states:

> **Objects of a superclass should be replaceable with objects of its subclasses without altering the correctness of the program.**

In simpler terms:
- If `S` is a subclass of `T`, then objects of type `T` in a program can be replaced with objects of type `S` without breaking the functionality.

---

## **Why Is LSP Important?**
Adhering to the LSP ensures that:
1. Subclasses remain **behaviorally consistent** with their superclasses.
2. Clients relying on the superclass behave the same when using its subclasses.
3. The system remains flexible, maintainable, and robust.

---

## **Common LSP Violations**
1. **Behavioral Mismatch:** Subclasses override methods in ways that break the expected behavior of the superclass.
2. **Stronger Preconditions:** Subclasses impose stricter conditions than the superclass.
3. **Weakened Postconditions:** Subclasses fail to deliver on the guarantees made by the superclass.

---

## **Example: LSP Violation**

### Problem Code
Let's consider a class hierarchy for shapes:

```java
class Rectangle {
    protected int width;
    protected int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getArea() {
        return width * height;
    }
}

class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width; // Ensuring it's a square
    }

    @Override
    public void setHeight(int height) {
        this.height = height;
        this.width = height; // Ensuring it's a square
    }
}
```

### Client Code
```java
public class Main {
    public void printArea(Rectangle rectangle) {
        rectangle.setWidth(10);
        rectangle.setHeight(5);
        System.out.println("Area: " + rectangle.getArea());
    }

    public static void main(String[] args) {
        Main app = new Main();

        // Using Rectangle
        Rectangle rect = new Rectangle();
        app.printArea(rect); // Output: Area: 50

        // Substituting Square
        Rectangle square = new Square();
        app.printArea(square); // Output: Area: 100 (Unexpected!)
    }
}
```

### What Went Wrong?
- The client expects the width and height to remain independent (`Rectangle` behavior).
- The `Square` class breaks this expectation by forcing width and height to be equal.

---

## **Solution: Adhering to LSP**

To fix this, we can introduce a common abstraction (e.g., `Shape`) for both `Rectangle` and `Square` instead of using inheritance.

### Correct Design
```java
interface Shape {
    int getArea();
}

class Rectangle implements Shape {
    private int width;
    private int height;

    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public int getArea() {
        return width * height;
    }
}

class Square implements Shape {
    private int side;

    public Square(int side) {
        this.side = side;
    }

    @Override
    public int getArea() {
        return side * side;
    }
}
```

### Updated Client Code
```java
public class Main {
    public void printArea(Shape shape) {
        System.out.println("Area: " + shape.getArea());
    }

    public static void main(String[] args) {
        Main app = new Main();

        // Using Rectangle
        Shape rectangle = new Rectangle(10, 5);
        app.printArea(rectangle); // Output: Area: 50

        // Using Square
        Shape square = new Square(5);
        app.printArea(square); // Output: Area: 25
    }
}
```

### Why This Works:
- Both `Rectangle` and `Square` implement the `Shape` interface.
- The client (`Main`) relies on the abstraction (`Shape`), ensuring smooth substitution of subclasses.

---

## **Key Takeaways**
- Subclasses should fulfill the expectations set by the superclass.
- Avoid inheritance if the subclass cannot maintain the behavior of the parent.
- Use composition or interfaces to provide flexibility and adhere to LSP.
