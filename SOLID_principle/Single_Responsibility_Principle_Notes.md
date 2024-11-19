
# Single Responsibility Principle (SRP)

The **Single Responsibility Principle (SRP)** is one of the SOLID principles in object-oriented programming. It states:
> "A class should have only one reason to change."

This means that a class should handle **only one responsibility** or concern, ensuring it has a focused purpose.

---

## Why Follow SRP?
1. **Easier Maintenance**: Changes in one area of functionality won't inadvertently affect other areas.
2. **Better Reusability**: Classes with single responsibilities can be reused in different contexts without modification.
3. **Improved Testability**: Testing smaller, focused classes is easier and more reliable.

---

## Violating SRP: Example
A class with multiple responsibilities violates SRP, making it harder to maintain and extend.

```java
public class Invoice {
    private double amount;

    public double calculateTotal() {
        // Business logic for calculating the total amount
        return amount * 1.18; // Includes tax, for example.
    }

    public void saveToDb() {
        // Logic for saving the invoice to the database
    }

    public void printInvoice() {
        // Logic for generating a printable invoice
    }
}
```
### Problems:
1. The `Invoice` class handles three unrelated responsibilities: business logic, database access, and printing.
2. Any change to one responsibility (e.g., printing format) can unintentionally affect others.

---

## Following SRP: Refactored Example
Separate the responsibilities into distinct classes to adhere to SRP.

```java
// Invoice class: Handles only business logic
public class Invoice {
    private double amount;
    private double taxRate;

    public Invoice(double amount, double taxRate) {
        this.amount = amount;
        this.taxRate = taxRate;
    }

    public double calculateTotal() {
        return amount + calculateTax();
    }

    private double calculateTax() {
        return amount * taxRate;
    }
}

// InvoiceDao class: Handles database operations
public class InvoiceDao {
    public void saveToDb(Invoice invoice) {
        // Logic to save the invoice to the database
    }
}

// InvoicePrinter class: Handles printing operations
public class InvoicePrinter {
    public void printInvoice(Invoice invoice) {
        // Logic to print the invoice
    }
}
```
### Benefits:
- Each class now has a single, well-defined responsibility.
- Changes to database logic, printing logic, or business logic are isolated.

---

## When Is It Okay to Have Multiple Methods in a Class?
If the methods are tightly related to the class's **single responsibility**, they can stay in the same class.

### Example:
```java
public class Invoice {
    private double amount;
    private double taxRate;

    public Invoice(double amount, double taxRate) {
        this.amount = amount;
        this.taxRate = taxRate;
    }

    public double calculateTotal() {
        return amount + calculateTax();
    }

    private double calculateTax() {
        return amount * taxRate;
    }
}
```
### Why It's Okay:
- Both `calculateTotal()` and `calculateTax()` are part of the invoice's business logic.
- The methods are cohesive and work together to fulfill the class's purpose.

---

## Rule of Thumb
- **Related methods** (e.g., calculations, validations) = Can stay in one class.
- **Unrelated responsibilities** (e.g., database access, printing) = Should be split into separate classes.

By following SRP, you ensure your code is modular, easier to maintain, and adheres to good design principles.
