# Single Responsibility Principle (SRP)

## What is SRP?

> A class should have only one reason to change, meaning it should have only one job or responsibility.

By adhering to SRP, you create code that is:

- **Modular**: Each class has a single, well-defined purpose.
- **Reusable**: Classes can be reused in different contexts.
- **Testable**: Easier to write unit tests for individual responsibilities.
- **Maintainable**: Changes to one responsibility do not affect others.

## Violation of SRP

```typescript
class PayrollSystem {
  private employees: { id: number; name: string; role: string }[] = [];
  private hourlyRates: { [role: string]: number } = {
    developer: 50,
    manager: 70,
    intern: 20,
  };

  // Responsibility 1: Employee Management
  addEmployee(id: number, name: string, role: string): void {
    this.employees.push({ id, name, role });
  }

  removeEmployee(id: number): void {
    this.employees = this.employees.filter((emp) => emp.id !== id);
  }

  getEmployee(
    id: number
  ): { id: number; name: string; role: string } | undefined {
    return this.employees.find((emp) => emp.id === id);
  }

  getAllEmployees(): { id: number; name: string; role: string }[] {
    return this.employees;
  }

  // Responsibility 2: Salary Calculation
  calculateSalary(employeeId: number, hoursWorked: number): number {
    const employee = this.getEmployee(employeeId);
    if (!employee) throw new Error('Employee not found');
    const rate = this.hourlyRates[employee.role] || 0;
    return rate * hoursWorked;
  }

  // Responsibility 3: Report Generation
  generatePayrollReport(): string {
    let report = 'Payroll Report:\n';
    this.employees.forEach((emp) => {
      const salary = this.calculateSalary(emp.id, 160); // Assume 160 hours worked
      report += `- ${emp.name} (${emp.role}): $${salary}\n`;
    });
    return report;
  }

  // Responsibility 4: Payment Processing
  processPayments(): void {
    this.employees.forEach((emp) => {
      const salary = this.calculateSalary(emp.id, 160); // Assume 160 hours worked
      console.log(
        `Processing payment of $${salary} to ${emp.name} (${emp.role})...`
      );
      // Simulate payment processing logic (e.g., API call to a payment gateway)
    });
  }
}

// Usage
const payrollSystem = new PayrollSystem();

// Add employees
payrollSystem.addEmployee(1, 'John Doe', 'developer');
payrollSystem.addEmployee(2, 'Jane Smith', 'manager');
payrollSystem.addEmployee(3, 'Alice Johnson', 'intern');

// Generate payroll report
const report = payrollSystem.generatePayrollReport();
console.log(report);

// Process payments
payrollSystem.processPayments();
```

## Compliance with SRP

we build a payroll system with the following responsibilities:

- **Employee Management**: Adding, removing, and retrieving employees.
- **Salary Calculation**: Calculating salaries based on employee roles and hours worked.
- **Report Generation**: Generating payroll reports.
- **Payment Processing**: Handling payments to employees.

Each responsibility is handled by a separate class.

```typescript
class EmployeeManager {
  private employees: { id: number; name: string; role: string }[] = [];

  addEmployee(id: number, name: string, role: string): void {
    this.employees.push({ id, name, role });
  }

  removeEmployee(id: number): void {
    this.employees = this.employees.filter((emp) => emp.id !== id);
  }

  getEmployee(
    id: number
  ): { id: number; name: string; role: string } | undefined {
    return this.employees.find((emp) => emp.id === id);
  }

  getAllEmployees(): { id: number; name: string; role: string }[] {
    return this.employees;
  }
}

class SalaryCalculator {
  private hourlyRates: { [role: string]: number } = {
    developer: 50,
    manager: 70,
    intern: 20,
  };

  calculateSalary(
    employee: { id: number; name: string; role: string },
    hoursWorked: number
  ): number {
    const rate = this.hourlyRates[employee.role] || 0;
    return rate * hoursWorked;
  }
}
class PayrollReportGenerator {
  generateReport(
    employees: { id: number; name: string; role: string }[],
    salaries: { [id: number]: number }
  ): string {
    let report = 'Payroll Report:\n';
    employees.forEach((emp) => {
      report += `- ${emp.name} (${emp.role}): $${salaries[emp.id]}\n`;
    });
    return report;
  }
}
class PaymentProcessor {
  processPayment(
    employee: { id: number; name: string; role: string },
    amount: number
  ): void {
    console.log(
      `Processing payment of $${amount} to ${employee.name} (${employee.role})...`
    );
    // Simulate payment processing logic (e.g., API call to a payment gateway)
  }
}

// Initialize all components
const employeeManager = new EmployeeManager();
const salaryCalculator = new SalaryCalculator();
const reportGenerator = new PayrollReportGenerator();
const paymentProcessor = new PaymentProcessor();

// Add employees
employeeManager.addEmployee(1, 'John Doe', 'developer');
employeeManager.addEmployee(2, 'Jane Smith', 'manager');
employeeManager.addEmployee(3, 'Alice Johnson', 'intern');

// Calculate salaries
const employees = employeeManager.getAllEmployees();
const salaries: { [id: number]: number } = {};

employees.forEach((emp) => {
  const hoursWorked = 160; // Assume 160 hours worked in a month
  salaries[emp.id] = salaryCalculator.calculateSalary(emp, hoursWorked);
});

// Generate payroll report
const report1 = reportGenerator.generateReport(employees, salaries);
console.log(report1);

// Process payments
employees.forEach((emp) => {
  paymentProcessor.processPayment(emp, salaries[emp.id]);
});
```

## Comparison

| **Aspect**           | **Clean Code (SRP)**                          | **Dirty Code (No SRP)**                       |
| -------------------- | --------------------------------------------- | --------------------------------------------- |
| **Responsibilities** | Separated into multiple classes               | Combined into a single class                  |
| **Reusability**      | High (e.g., `SalaryCalculator` can be reused) | Low (logic is tightly coupled)                |
| **Testability**      | Easy to test individual classes               | Hard to test individual functionalities       |
| **Maintainability**  | High (changes are isolated)                   | Low (changes affect multiple functionalities) |
| **Readability**      | Clear and modular                             | Bloated and hard to understand                |

## Conclusion

The Single Responsibility Principle (SRP) is a cornerstone of clean, maintainable, and scalable code. By adhering to SRP:

- You create modular and reusable components.
- Your code becomes easier to test and maintain.
- Changes in one area do not affect other parts of the system.

In contrast, violating SRP leads to:

- Tightly coupled and bloated classes.
- Poor reusability and testability.
- Increased risk of bugs during maintenance.
