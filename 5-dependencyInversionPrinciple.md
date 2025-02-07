# Dependency Inversion Principle (DIP)

## What is the ?

> High-level modules should not depend on low-level modules. Both should depend on abstractions.

> Abstractions should not depend on details. Details should depend on abstractions.

In simpler terms:

- High-level modules (e.g., business logic) should not be tightly coupled to low-level modules (e.g., database access, external APIs).

- Instead, both should depend on abstractions (e.g., interfaces or abstract classes).

This principle encourages the use of dependency injection and promotes loose coupling, making the system more modular, maintainable, and testable.

## Why is DIP Important?

- Loose Coupling: Reduces dependencies between modules, making the system easier to modify and extend.

- Testability: Abstractions make it easier to mock dependencies during unit testing.

- Flexibility: Switching implementations (e.g., changing databases or payment processors) becomes straightforward.

- Maintainability: Changes in low-level modules do not affect high-level modules, reducing the risk of bugs.

### Violation of DIP

In this example, the high-level module (`UserService`) directly depends on the low-level module (`MySQLDatabase`). This creates tight coupling and

```typescript
// Low-level module
class MySQLDatabase {
  save(data: string): void {
    console.log(`Saving data to MySQL: ${data}`);
  }
}

// High-level module
class UserService {
  private database: MySQLDatabase;

  constructor() {
    this.database = new MySQLDatabase(); // Direct dependency on low-level module
  }

  createUser(name: string): void {
    this.database.save(name);
  }
}

// Usage
const userService = new UserService();
userService.createUser('John Doe');
```

Problem:

- UserService is tightly coupled to MySQLDatabase. If we want to switch to another database (e.g., MongoDB), we need to modify UserService.

### Compliance with DIP

To comply with DIP, we introduce an abstraction (Database interface) that both high-level and low-level modules depend on.

```typescript
// Abstraction (interface)
interface Database {
  save(data: string): void;
}

// Low-level module
class MySQLDatabase implements Database {
  save(data: string): void {
    console.log(`Saving data to MySQL: ${data}`);
  }
}

// Another low-level module
class MongoDBDatabase implements Database {
  save(data: string): void {
    console.log(`Saving data to MongoDB: ${data}`);
  }
}

// High-level module
class UserService {
  private database: Database;

  constructor(database: Database) {
    // Dependency injection
    this.database = database;
  }

  createUser(name: string): void {
    this.database.save(name);
  }
}

// Usage
const mySQLDatabase = new MySQLDatabase();
const userService1 = new UserService(mySQLDatabase);
userService1.createUser('John Doe');

const mongoDBDatabase = new MongoDBDatabase();
const userService2 = new UserService(mongoDBDatabase);
userService2.createUser('Jane Doe');
```

Benefits:

- UserService no longer depends on a specific database implementation. It depends on the Database abstraction.

- We can easily switch between different database implementations without modifying UserService.

## Comparison of Examples

| **Aspect**           | **Violation of DIP**                                                         | **Compliance with DIP**                                        |
| -------------------- | ---------------------------------------------------------------------------- | -------------------------------------------------------------- |
| **Coupling**         | Tight coupling between high-level and low-level modules.                     | Loose coupling through abstractions.                           |
| **Flexibility**      | Difficult to switch implementations (e.g., databases or payment processors). | Easy to switch implementations.                                |
| **Testability**      | Hard to mock dependencies for unit testing.                                  | Easy to mock dependencies using abstractions.                  |
| **Maintainability**  | Changes in low-level modules affect high-level modules.                      | Changes in low-level modules do not affect high-level modules. |
| **Code Reusability** | Low reusability due to tight coupling.                                       | High reusability due to modular design.                        |

## Conclusion

The Dependency Inversion Principle (DIP) is a powerful concept that promotes loose coupling, modularity, and maintainability in software design. By depending on abstractions rather than concrete implementations, we can create systems that are easier to extend, test, and maintain.
