# Open/Closed Principle (OCP)

## What is OCP?

> Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.\*\*

This means that you should design your code in such a way that you can add new functionality without changing the existing code. This principle promotes flexibility, maintainability, and scalability in your codebase.

---

## Key Concepts of OCP

1. **Open for Extension**: You should be able to extend the behavior of a class or module without modifying its source code.
2. **Closed for Modification**: Once a class or module is implemented, it should not be modified to add new behavior. Instead, you should extend it.

---

## Violation of OCP

The `violation-example.ts` file demonstrates a violation of the Open/Closed Principle. Here's how it fails:

### Code Explanation

1. Classes Rectangle, Circle, Triangle, and Square: Each class has its own area() method, but there is no common interface or contract.

2. Class AreaCalculator: Takes an array of shapes and uses instanceof checks to determine the type of each shape and calculate its area.

3. Modification Required: If you want to add a new shape (e.g., Square), you must modify the AreaCalculator class to handle the new type.

```typescript

class Rectangle {
constructor(private width: number, private height: number) {}

  area(): number {
    return this.width \* this.height;
  }
}

class Circle {
constructor(private radius: number) {}

  area(): number {
    return Math.PI \* this.radius \*\* 2;
  }
}

class Triangle {
constructor(private base: number, private height: number) {}

  area(): number {
    return (this.base \* this.height) / 2;
  }
}

class Square {
constructor(private side: number) {}

  area(): number {
    return this.side \*\* 2;
  }
}

class AreaCalculator {
constructor(private shapes: (Rectangle | Circle | Triangle | Square)[]) {}

  totalArea(): number {
    return this.shapes.reduce((sum, shape) => {
      if (shape instanceof Rectangle) {
        return sum + shape.area();
      } else if (shape instanceof Circle) {
        return sum + shape.area();
      } else if (shape instanceof Triangle) {
        return sum + shape.area();
      } else if (shape instanceof Square) {
        return sum + shape.area();
      }
        return sum;
    }, 0);
  }
}

const shapes = [
  new Rectangle(10, 20),
  new Circle(5),
  new Triangle(10, 15),
  new Square(4),
];

const areaCalculator = new AreaCalculator(shapes);
console.log('Total Area:', areaCalculator.totalArea());
```

## Drawbacks

1. **Not Extensible**: Adding a new shape requires modifying the AreaCalculator class.

2. **Brittle Code**: The instanceof checks make the code prone to errors and harder to maintain.

3. **Violates SRP**: The AreaCalculator class has to know about every shape type, violating the Single Responsibility Principle.

## Compliance with OCP

The `compliance-example.ts` file demonstrates how to adhere to the Open/Closed Principle. Here's how it works:

### Code Explanation

1. **Interface `Shape`**: Defines a contract (`area()`) that all shapes must implement.
2. **Classes `Rectangle`, `Circle`, and `Triangle`**: Implement the `Shape` interface and provide their own implementation of the `area()` method.
3. **Class `AreaCalculator`**: Takes an array of `Shape` objects and calculates the total area by calling the `area()` method on each shape.
4. **Extensibility**: If you want to add a new shape (e.g., `Square`), you only need to create a new class that implements the `Shape` interface. No changes are required in the `AreaCalculator` class.

```typescript
interface Shape {
  area(): number;
}

class Rectangle implements Shape {
  constructor(private width: number, private height: number) {}

  area(): number {
    return this.width * this.height;
  }
}

class Circle implements Shape {
  constructor(private radius: number) {}

  area(): number {
    return Math.PI * this.radius ** 2;
  }
}

class Triangle implements Shape {
  constructor(private base: number, private height: number) {}

  area(): number {
    return (this.base * this.height) / 2;
  }
}

class AreaCalculator {
  constructor(private shapes: Shape[]) {}

  totalArea(): number {
    return this.shapes.reduce((sum, shape) => sum + shape.area(), 0);
  }
}

const shapes: Shape[] = [
  new Rectangle(10, 20),
  new Circle(5),
  new Triangle(10, 15),
];

const areaCalculator = new AreaCalculator(shapes);
console.log('Total Area:', areaCalculator.totalArea());
```

### Benefits

1. **Extensible** : Adding a new shape (e.g., Square) requires no changes to the AreaCalculator class.

2. **Maintainable**: The code is clean and adheres to a single responsibility principle.

3. **Scalable**: The design supports future growth without introducing bugs or complexity.

## Guidelines for Applying OCP

1. Use Interfaces or Abstract Classes: Define contracts that classes must implement.

2. Avoid Type Checking: Do not use instanceof or similar checks to determine behavior.

3. Encapsulate Behavior: Move behavior into classes that implement the interface.

4. Favor Composition Over Inheritance: Use composition to extend functionality.

5. Design for Change: Anticipate future requirements and design your code to accommodate them.

## Comparison

| Feature                | Compliance Example (OCP)                    | Violation Example (Non-OCP)           |
| ---------------------- | ------------------------------------------- | ------------------------------------- |
| **Extensibility**      | Easy to add new shapes without modification | Requires modifying existing code      |
| **Maintainability**    | Clean and modular                           | Brittle and error-prone               |
| **Scalability**        | Supports future growth                      | Difficult to scale                    |
| **Code Duplication**   | Minimal                                     | High (repetitive `instanceof` checks) |
| **Adherence to SOLID** | Follows OCP and SRP                         | Violates OCP and SRP                  |

---

## Conclusion

The comparison clearly demonstrates the advantages of adhering to the Open/Closed Principle. By designing your code to be open for extension but closed for modification, you can create systems that are flexible, maintainable, and scalable. On the other hand, ignoring OCP leads to brittle code that is difficult to extend and maintain.
