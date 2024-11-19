
# Open-Closed Principle (OCP)

The **Open-Closed Principle (OCP)** is one of the SOLID principles in object-oriented programming. It states:
> **"Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification."**

---

## What Does This Mean?
- **Open for Extension**: You should be able to add new functionality to a module, class, or function without modifying its existing code.
- **Closed for Modification**: Once a class or module is written and tested, it should not be changed unless there's a bug fix or necessary refactor.

This ensures that new features can be added to the system without introducing errors in existing functionality.

---

## Why Use OCP?
1. **Promotes Reusability**: Adding new features without touching existing code reduces duplication.
2. **Improves Maintainability**: You don’t need to modify stable, tested code, reducing the chance of errors.
3. **Enhances Flexibility**: Makes the system more adaptable to changes.

---

## Example: Violating OCP

Consider a class for calculating discounts based on customer types.

```java
public class DiscountCalculator {
    public double calculateDiscount(String customerType, double amount) {
        if (customerType.equals("Regular")) {
            return amount * 0.1; // 10% discount
        } else if (customerType.equals("Premium")) {
            return amount * 0.2; // 20% discount
        }
        return 0;
    }
}
```

### Problems:
1. Adding a new customer type (e.g., VIP) requires modifying the `calculateDiscount` method.
2. Each modification increases the risk of introducing bugs to existing functionality.
3. Violates the OCP because the class is not **closed for modification**.

---

## Following OCP: Refactored Example

We can refactor the code using **polymorphism** to allow new discount types to be added without changing the existing code.

### Refactored Design:
```java
// Abstract class or interface for discounts
public interface DiscountPolicy {
    double applyDiscount(double amount);
}

// Regular customer discount
public class RegularCustomerDiscount implements DiscountPolicy {
    public double applyDiscount(double amount) {
        return amount * 0.1; // 10% discount
    }
}

// Premium customer discount
public class PremiumCustomerDiscount implements DiscountPolicy {
    public double applyDiscount(double amount) {
        return amount * 0.2; // 20% discount
    }
}

// Context class using the discount policy
public class DiscountCalculator {
    private DiscountPolicy discountPolicy;

    public DiscountCalculator(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }

    public double calculateDiscount(double amount) {
        return discountPolicy.applyDiscount(amount);
    }
}
```

### Adding New Discounts:
To add a new customer type, simply create a new class that implements `DiscountPolicy`:

```java
// New discount for VIP customers
public class VIPCustomerDiscount implements DiscountPolicy {
    public double applyDiscount(double amount) {
        return amount * 0.3; // 30% discount
    }
}
```

You can use the new discount policy without modifying the existing `DiscountCalculator` class:

```java
DiscountPolicy vipDiscount = new VIPCustomerDiscount();
DiscountCalculator calculator = new DiscountCalculator(vipDiscount);
double finalAmount = calculator.calculateDiscount(1000);
```

---

## Key Advantages of OCP:
1. **No Changes to Existing Code**: You don’t touch `DiscountCalculator` when adding a `VIPCustomerDiscount`.
2. **Extensibility**: The design easily accommodates new features.
3. **Reduced Risk**: Changes are localized to the new functionality.

---

## Rule of Thumb:
- Avoid hardcoding logic into classes.
- Use **interfaces, inheritance, or composition** to allow extensions.

By following OCP, you ensure your code is modular, scalable, and robust.
