---
layout: post
title: "TS: Class와 Interface"
categories: [ts]
---

Understanding TypeScript : Section 5

---

```typescript
class Department {
  private name: string;

  constructor(n: string) {
    this.name = n;
  }

  describe(this: Department) {
    console.log(`Department: ${this.name}`);
  }
}

const accounting = new Department("Accounting");
accounting.describe();

const accountingCopy = { describe: accounting.describe };
accountingCopy.describe();
```

- `accountingCopy.describe();` throws a compiling error because as we assign this to `Department` class type, typescript compares the shape of `accountingCopy` and class `Department` and finds that the shape not fits.

  Thus now we can make sure that this of method sholud always look like a class.

### Typescript only features

- if I add modifier `private`, the propertycannot be accessed outside of the method. Only through public method the code can access to private property.

```typescript
class Department {
  private employees: string[] = [];

  constructor(private readonly id: string, public name: string) {}
}
```

- initialization can be shortened using constuctor method
  it means you make exact same properties in the class, and `public` / `private` keyword is necessary to inform that we are initializing properties. It can prevent double initialization and make code shorter.
- `readonly` keyword makes sure that the readonly property can be changed once when initialization.

## Inheritence

class that inherits the other class automatically has al the feature of the class it inherits including properties and constructors.

- when making constructor, `super()` is necessary and it must come in the first line of the constructor method. **shortened initialization is still possible**, but if you want to initialize property first and assign value by using `this.admins` for example, then that line should come **after** the `super` command.

```typescript
class ITDepartment extends Department {
  admins: string[];
  constructor(id: string, admins: string[]) {
    super(id, "IT");
    this.admins = admins;
  }
}

class AccountingDeparment extends Department {
  constructor(id: string, private reports: string[]) {
    super(id, "Accounting");
  }
}
```

- while `private` mark make the property accessable only inside the class, `protected` mark makes the property **accessable in the class that inherits the class that the property was initialized**, still not acceessable outside class.

```typescript
class Department {
//private employees: string[] = [];
  protected employees: string[] = [];
  ...
}

class AccountingDeparment extends Department {
  ...
  addEmployee(employee: string) {
    if (employee === 'Max') {
      return;
    }
    this.employees.push(employee);
  }
}
```

### Getter and Setter

By using `get` and `set`keyword, private property or method can be accessed outside of the class.

- getter and setter can have the same name. while using itself alone, it works as a getter, and when assign something with equal sign, it can work as a setter.

```typescript
class AccountingDeparment extends Department {
  private lastReport: string;

  get mostRecentReport() {
    if (this.lastReport) {
      return this.lastReport;
    }
    throw new Error("No report found.");
  }

  set mostRecentReport(value: string) {
    if (!value) {
      throw new Error("Please pass in a valid value");
    }
    this.addReport(value);
  }

  //...
}

const accounting = new AccountingDeparment("d2", []);
accounting.mostRecentReport = "second report";
console.log(accounting.mostRecentReport);
```

### static

can call it directly on a class without initializing class with `new` keyword. it can be methods and property. You don't call the static method on an object created based on a class.

but you can't access to static property or method inside the class.

```typescript
class Deparment {
  static fiscalYear = "2021";

  constructor() {
    console.log(this.fiscalYear); // ERROR
    console.log(Department.fiscalYear); // 2021
  }

  createEmployee(name: string) {
    return { name: name };
  }
}

const employee1 = Department.createEmployee("Max");
console.log(employee1); // {name: Max}
consoel.log(Department.fiscalYear); // 2021
```

### abstract

If it's clear that a class is always extended and a method varies with every inherited classes, you can make the class and method abstract.

```typescript
abstract class Department {
  //...
  abstract describe(this: Department): void;
}

class ITDepartment extends Department {
  //...
  describe() {
    console.log(`IT Department: ${this.id}`);
  }
}

it.describe(); // IT Department: d1
```

- if you make a method `abstract`, then you must override the method in every class that extending the abstract class.
- the abstract method does not have any content in it, thus it doesn't have `{}`, instead **it only has parameter and return type definitions**.
- when you make a `abstract class` , then **the class cannot be initialized by itself**, instead the other class extending the abstract class can be initialized with `new` keyword,

## singleton pattern

when you have to make sure that only one object exists, you say it singleton pattern. below is the example of make a class singleton.

- inside static method, static property is accessable with `this` keyword.
- `AccountingDepartment.instance` and `this.instance` is the same since `this` refers to the class itself.

```typescript
class AccountingDeparment extends Department {
  private static instance: AccountingDeparment;

  private constructor(id: string, private reports: string[]) {
    super(id, "Accounting");
    this.lastReport = reports[0];
  }

  static getInstance() {
    if (AccountingDeparment.instance) {
      return this.instance;
    }
    this.instance = new AccountingDeparment("d2", []);
    return this.instance;
  }
}

const accounting = new AccountingDepartment("d2", []);
// throws an error because constructor is private.

const accounting1 = AccountingDepartment.getInstance();
const accounting2 = AccountingDepartment.getInstance();
// accounting1 and accounting2 is exactly the same.
```

# Interface

Interface **defines the structure of the object**. It defines how the object look like. In Interface, we can only define the shape not assign default value, or make function. It's **PURE typescript feature,** thus it is not compiled to javascript and can't be instantiated. So it makes easier and clearer that the code is well structured, not making any traces during runtime.

_It's a powerful feature that forces a class or an object to have a certain structure!_

**difference with custom type**

- interfaces are used to describe object
- interfaces cannot store or describe arbitrary types like union types
- interfaces can be implemented in class

```typescript
interface Named {
  readonly name: string;
}

interface Aged {
  age: number;
}

interface Greetable extends Named, Aged {
  greet(phrase: string): void;
}

class Person implements Greetable {
  name: string;
  age: number;

  constructor(n: string, a: number) {
    this.name = n;
    this.age = a;
  }
  greet(phrase: string) {
    console.log(`${phrase} ${this.name}`);
  }
}
```

- interface can extend other interfaces so that it can merge multiple interfaces together
- a class can implement more than one interfaces, making sure that a specific type of property or method is exist in the class.

### function type

it can be used as an alternative of function type. So the first line of the code and the next three lines work exactly the same.

```typescript
// type AddFn = (a: number, b: number) => number;
interface AddFn {
  (a: number, b: number): number;
}

let add: AddFn;
add = (n1: number, n2: number) => {
  return n1 + n2;
};
```

### Optionals

optional property and parameter can be set with question mark, `?` so that you can make the structure of your code more flexible.

```typescript
interface Greetable extends Named {
  greet(phrase: string): void;
}

class Person implements Greetable {
  name?: string;
  age = 30;

  constructor(n?: string) {
    if (n) {
      this.name = n;
    }
  }
}

const user1 = new Person();
const user2 = new Person("Jeongmin"); // both possible
```
