# Interface Segregation Principle (ISP)

## What is the ISP?

> Clients should not be forced to depend on interfaces they do not use.

In other words:

- Large, monolithic interfaces should be avoided because they force implementing classes to define methods they don't need.

- Instead, small, specific interfaces should be created, each serving a distinct purpose.

Why is ISP Important?

1.  Reduces coupling: Clients only depend on the methods they actually use.

2.  Improves maintainability: Changes to one interface don't affect unrelated clients.

3.  Enhances readability: Smaller interfaces are easier to understand and implement.

## Example

To demonstrate ISP, we'll use a User Management System with two types of users:

1.  Admin: Can read, write, and delete data.

2.  Guest: Can only read data.

We'll implement this system in two ways:

- Compliance with ISP: Using small, specific interfaces.

- Violation of ISP: Using a single, monolithic interface.

## Violation of ISP

In this implementation, we use a single, monolithic interface (UserActions) that includes all methods (read, write, delete). This forces the Guest class to implement methods it doesn't need.
Code

```typescript
export interface UserActions {
  read(): void;
  write(): void;
  delete(): void;
}

export class Admin implements UserActions {
  read(): void {
    console.log("Admin is reading data.");
  }

  write(): void {
    console.log("Admin is writing data.");
  }

  delete(): void {
    console.log("Admin is deleting data.");
  }
}


export class Guest implements UserActions {
  read(): void {
    console.log("Guest is reading data.");
  }

  // Guest is forced to implement methods it doesn't need
  write(): void {
    throw new Error("Guest cannot write data.");
  }

  delete(): void {
    throw new Error("Guest cannot delete data.");
  }
}

const admin = new Admin();
admin.read(); // Admin is reading data.
admin.write(); // Admin is writing data.
admin.delete(); // Admin is deleting data.

const guest = new Guest();
guest.read(); // Guest is reading data.
// guest.write(); // Throws an error: "Guest cannot write data."
// guest.delete(); // Throws an error: "Guest cannot delete data."

//Output
Admin is reading data.
Admin is writing data.
Admin is deleting data.
Guest is reading data.
```

Problems with Violation

- Unnecessary methods: Guest is forced to implement write() and delete(), even though it doesn't need them.

- Error-prone: Calling write() or delete() on a Guest object throws an error.

- Rigid design: Adding new user types requires implementing all methods, even if they're not needed.

### Compliance with ISP

In this implementation, we create small, specific interfaces for each capability:

    CanRead: For reading data.

    CanWrite: For writing data.

    CanDelete: For deleting data.

```typescript
export interface CanRead {
  read(): void;
}

export interface CanWrite {
  write(): void;
}

export interface CanDelete {
  delete(): void;
}

export class Admin implements CanRead, CanWrite, CanDelete {
  read(): void {
    console.log("Admin is reading data.");
  }

  write(): void {
    console.log("Admin is writing data.");
  }

  delete(): void {
    console.log("Admin is deleting data.");
  }
}

export class Guest implements CanRead {
  read(): void {
    console.log("Guest is reading data.");
  }
}

const admin = new Admin();
admin.read(); // Admin is reading data.
admin.write(); // Admin is writing data.
admin.delete(); // Admin is deleting data.

const guest = new Guest();
guest.read(); // Guest is reading data.

//Output
Admin is reading data.
Admin is writing data.
Admin is deleting data.
Guest is reading data.
```

Benefits of Compliance

- No unnecessary methods: Guest only implements read() and doesn't need to define write() or delete().

- Flexibility: New user types can implement only the interfaces they need.

- Maintainability: Changes to one interface (e.g., CanWrite) don't affect unrelated classes.

## Comparison

| **Aspect**              | **Compliance with ISP**                                          | **Violation of ISP**                                             |
| ----------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| **Interface Design**    | Small, specific interfaces (`CanRead`, `CanWrite`, `CanDelete`). | Single, monolithic interface (`UserActions`).                    |
| **Client Dependencies** | Clients depend only on the methods they use.                     | Clients depend on methods they don't use.                        |
| **Flexibility**         | Easy to add new user types with specific needs.                  | Adding new user types requires implementing unnecessary methods. |
| **Maintainability**     | Changes to one interface don't affect others.                    | Changes to the interface affect all clients.                     |
| **Error Handling**      | No unnecessary methods or errors.                                | Unnecessary methods throw errors.                                |

## Conclusion

The ISP encourages us to design small, specific interfaces that cater to the needs of individual clients. By adhering to ISP, we:

- Reduce coupling between classes.

- Improve code readability and maintainability.

- Avoid forcing clients to implement unnecessary methods.
