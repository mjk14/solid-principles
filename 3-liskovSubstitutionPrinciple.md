# Liskov Substitution Principle (LSP)

## What is LSP?

> Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program.

In simpler terms, if a program is designed to work with a base class, it should also work correctly with any of its derived classes without requiring changes to the program's logic. This principle ensures that inheritance is used properly and that subclasses adhere to the contract defined by their superclass.

### Why is LSP Important?

- Maintainability: Code that adheres to LSP is easier to maintain because subclasses behave predictably.

- Reusability: Subclasses can be reused in place of their superclass without causing unexpected behavior.

- Robustness: Programs are less likely to break when new subclasses are introduced.

### Violation of LSP

```typescript
class Rectangle {
  protected width: number;
  protected height: number;

  constructor(width: number, height: number) {
    this.width = width;
    this.height = height;
  }

  setWidth(width: number): void {
    this.width = width;
  }

  setHeight(height: number): void {
    this.height = height;
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  constructor(size: number) {
    super(size, size);
  }

  setWidth(width: number): void {
    this.width = width;
    this.height = width; // Ensure width and height are always equal
  }

  setHeight(height: number): void {
    this.width = height;
    this.height = height; // Ensure width and height are always equal
  }
}

// Usage
function printArea(rectangle: Rectangle): void {
  rectangle.setWidth(4);
  rectangle.setHeight(5);
  console.log(`Area: ${rectangle.getArea()}`);
}

const rectangle = new Rectangle(2, 3);
const square = new Square(2);

printArea(rectangle); // Output: Area: 20
printArea(square); // Output: Area: 25
```

Explanation

Base Class (Rectangle):

- The Rectangle class allows independent modification of width and height.

Derived Class (Square):

- The Square class inherits from Rectangle but overrides setWidth and setHeight to enforce that width and height are always equal.

Usage:

- The printArea function modifies the width and height of a Rectangle object and calculates its area.

- When a Square object is passed to printArea, the behavior is unexpected because setting the width also changes the height.

Why This Violates LSP

- The Square class changes the behavior of the setWidth and setHeight methods, which violates the contract defined by the Rectangle class.

- A Square object cannot be used interchangeably with a Rectangle object without causing unexpected behavior.

### Compliance with LSP

```typescript
class Shape {
  getArea(): number {
    throw new Error('Method not implemented.');
  }
}

class Rectangle extends Shape {
  protected width: number;
  protected height: number;

  constructor(width: number, height: number) {
    super();
    this.width = width;
    this.height = height;
  }

  setWidth(width: number): void {
    this.width = width;
  }

  setHeight(height: number): void {
    this.height = height;
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Shape {
  private size: number;

  constructor(size: number) {
    super();
    this.size = size;
  }

  setSize(size: number): void {
    this.size = size;
  }

  getArea(): number {
    return this.size * this.size;
  }
}

// Usage
function printArea(shape: Shape): void {
  console.log(`Area: ${shape.getArea()}`);
}

const rectangle = new Rectangle(2, 3);
const square = new Square(2);

rectangle.setWidth(4);
rectangle.setHeight(5);
printArea(rectangle); // Output: Area: 20

square.setSize(5);
printArea(square); // Output: Area: 25
```

Explanation

Base Class (Shape):

- The Shape class defines a method getArea that throws an error if not implemented. This ensures that all derived classes must implement this method.

Derived Classes (Rectangle and Square):

- Both Rectangle and Square extend the Shape class and implement the getArea method.

- The Rectangle class allows independent modification of width and height.

- The Square class enforces that the size (width and height) must always be equal.

Usage:

- The printArea function accepts a Shape object and calls its getArea method.

- Both Rectangle and Square objects can be passed to printArea without causing unexpected behavior.

Why This Complies with LSP

- The Rectangle and Square classes can be used interchangeably wherever a Shape is expected.

- The behavior of the getArea method is consistent and predictable for both derived classes.

## Comparison

| **Aspect**              | **Compliance Example**                         | **Violation Example**                                 |
| ----------------------- | ---------------------------------------------- | ----------------------------------------------------- |
| **Inheritance**         | `Rectangle` and `Square` extend `Shape`.       | `Square` extends `Rectangle`.                         |
| **Behavior**            | Both classes implement `getArea` consistently. | `Square` modifies behavior of `setWidth`/`setHeight`. |
| **Interchangeability**  | `Rectangle` and `Square` can replace `Shape`.  | `Square` cannot replace `Rectangle` without issues.   |
| **Unexpected Behavior** | No unexpected behavior.                        | Unexpected behavior when `Square` is used.            |

## Conclusion

Avoid forcing subclasses to inherit from classes that don't align with their behavior.
Use composition over inheritance when subclasses cannot fully adhere to the superclass contract.
