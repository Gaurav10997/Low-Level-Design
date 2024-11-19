
# Interface Segregation Principle (ISP)

The **Interface Segregation Principle (ISP)** is one of the five SOLID principles of object-oriented design. It states:

> **"No client should be forced to depend on methods it does not use."**

This principle emphasizes creating smaller, more specific interfaces rather than a large, monolithic one. This way, classes implementing the interfaces are only required to implement methods that are relevant to them, reducing unnecessary dependencies.

---

## Key Points:
1. **Avoid Fat Interfaces**: Large interfaces with many methods can force classes to implement methods they don't need.
2. **Split Interfaces**: Divide large interfaces into smaller, cohesive interfaces that group related functionalities.
3. **Encapsulation of Behavior**: Design interfaces around specific behaviors to ensure clients only depend on the behaviors they use.

---

## Example of Violation:
Suppose you have a `Printer` interface:

```typescript
interface Printer {
  print(): void;
  scan(): void;
  fax(): void;
}
```

Now, a simple printer that only supports printing will have to implement the unused `scan` and `fax` methods.

```typescript
class SimplePrinter implements Printer {
  print(): void {
    console.log("Printing...");
  }

  scan(): void {
    throw new Error("Scan not supported");
  }

  fax(): void {
    throw new Error("Fax not supported");
  }
}
```

---

## Following ISP:
To adhere to the Interface Segregation Principle, split the `Printer` interface into smaller, more specific interfaces:

```typescript
interface Printable {
  print(): void;
}

interface Scannable {
  scan(): void;
}

interface Faxable {
  fax(): void;
}
```

Now, the `SimplePrinter` class only implements what it actually uses:

```typescript
class SimplePrinter implements Printable {
  print(): void {
    console.log("Printing...");
  }
}
```

For a multifunction printer, you can implement multiple interfaces:

```typescript
class MultiFunctionPrinter implements Printable, Scannable, Faxable {
  print(): void {
    console.log("Printing...");
  }

  scan(): void {
    console.log("Scanning...");
  }

  fax(): void {
    console.log("Faxing...");
  }
}
```

---

## Benefits:
- **Flexibility**: Clients are not burdened with unnecessary dependencies.
- **Maintainability**: Changes in one interface don’t affect unrelated clients.
- **Reusability**: Smaller interfaces make it easier to combine behaviors as needed.
