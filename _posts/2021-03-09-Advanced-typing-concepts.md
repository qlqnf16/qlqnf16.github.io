---
layout: post
title: "Advanced TS"
category: [ts]
---

Understanding TypeScript : Section 6

---

## Intersection Types

Intersection combines multiple types. It work same as when interface implements other interfaces but the intersection operators can work with **any type,** such as union type.

```typescript
type Admin = {
  name: string;
  privileges: string[];
};

type Employee = {
  name: string;
  hiredDate: Date;
};

type ElevatedEmployee = Admin & Employee;

const e1: ElevatedEmployee = {
  name: "Max",
  privileges: ["create-server"],
  hiredDate: new Date(),
};

type Combinable = string | number;
type Numeric = number | boolean;
type Universal = Combinable & Numeric; // type Univeral = string | number | boolean;
```

## Type Guards

A code pattern that helps you **avoid runtime errors** by checking types before you try to do something with the values. It works during runtime, thus it should be written in a way that works in javascript.

### 3 ways of Type Guards

- `typeof` - when checking type of a variable, should be **JS type**. (no interface)
- `'key' in object` - when checking the instance of object made with interface or class. possible to make a misstyping error.
- `instanceof {object}` - when checking if a variable is instance of class or object. Cannot use with interface like `if (example instanceof SampleInterface)` because interface is not compiled into javascript.

```typescript
// 1. Example of using typeof
function add(a: Combinable, b: Combinable) {
  if (typeof a === "string" || typeof b === "string") {
    return a.toString() + b.toString();
  }
  return a + b;
}

// 2. Example of using 'in'
type unknownEmployee = Employee | Admin;
function printEmployeeInformation(emp: unknownEmployee) {
  console.log(`Name: ${emp.name}`);
  if ("privileges" in emp) {
    console.log(`Privileges: ${emp.privileges}`);
  }
  if ("hiredDate" in emp) {
    console.log(`Hired Dae: ${emp.hiredDate}`);
  }
}

// 3. Example of using 'instanceof'
class Car {
  drive() {
    console.log("Driving a car...");
  }
}

class Truck {
  drive() {
    console.log("Driving a truck...");
  }

  loadCargo(amount: number) {
    console.log(`Loading cargo... ${amount}`);
  }
}

type Vehicle = Car | Truck;
const v1 = new Car();
const v2 = new Truck();

function useVehicle(vehicle: Vehicle) {
  vehicle.drive();
  if (vehicle instanceof Truck) {
    vehicle.loadCargo(1000);
  }
}
```

### Discriminated Unions

It's a pattern that can be implemented to **make type guards easier.** By **giving a same property to all interfaces** that would be used in union types, it can make type guards easier.

```typescript
interface Bird {
  type: "bird"; // literal type
  flyingSpeed: number;
}

interface Horse {
  type: "horse"; // literal type
  runningSpeed: number;
}

type Animal = Bird | Horse;

function moveAnimal(animal: Animal) {
  let speed;
  switch (animal.type) {
    case "bird":
      speed = animal.flyingSpeed;
      break;
    case "horse":
      speed = animal.runningSpeed;
      break;
  }
  console.log(`Moving at speed ${speed}`);
}
```

## Type Casting

Type casting is used when typescript compilier is not sure with type but we, developer know the type. **It informs TS that a certain value is of a specific type.** It's especially useful when dealing with DOM elements.

```html
<!-- input.html -->
<input type="text" id="user-input" />
```

```typescript
// app.ts
const userInputElement = <HTMLInputElement>(
  document.getElementById("user-input")
);
const userInputElement = document.getElementById(
  "user-input"
) as HTMLInputElement;

userInputElement.value = "Hello there!";
```

With the example above,

- typescript only knows that the element selected with `getElementById` is `HTMLElement`, but does not know if it's a input, paragraph or other elements.

  Thus, the last line of the code will make a compiling error because `HTMLElement` does not have `.value` property.

- If developer knows that it would be a `HTMLInputElement`, we can use type casting so that we can fully use the property of HTMLInputElement.

Type casting can be done in two ways, which is exactly the same.

- `<type>...;`
- `... as type`: this method is implemented to use with `react`. As react has different grammar that uses bracket <>, type casting using bracket is eliminated in react-typescript. Thus, `as type` can be used as an alternative in react.

## Index Type

Index type makes **the structure more flexible**. when I'm sure with the type of value, but not sure the amount or the name of property coming in.

i dont know property name and count, but I know that all the property in the container is string and value of the property is string.

```typescript
interface ErrorContainer {
  [prop: string]: string;
}

const errorBag: ErrorContainer = {
  email: "not a valid email!",
  username: "Must start with a capital character!",
};
```

## Function Overloads

When using union types as a parameter of function, typescript cannot inference the return type. We can use function overloads when we are sure what return type of the function would be.

```typescript
function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: Combinable, b: Combinable) {
  if (typeof a === "string" || typeof b === "string") {
    return a.toString() + b.toString();
  }
  return a + b;
}

const result = add("Max", "Schwarz");
result.split(" ");
```

- first two lines of the code above is **function overloads**.

  without them, the last line would throw an error because typescript thinks that the return type is `Combinable`, not `string`.

- when we know the return type, in this case we know that if parameter coming in is string, then returned value must be a string, then we can add function overloads. By defining the return type with specific case of parameter type, typescript now knows that the `result` is string type, and can use `.split` method without any error.

## Useful Features Regarding Nullish Value

### Optional Chaning

when working with some data that has nested structure, and not sure if the nested data exists or not. We can use optional chaining with question mark.

```typescript
console.log(fetchedUserData.job?.title);
```

We can write code like above when we are not sure if job property exists or not. And the code will be compiled into javascript as below.

```javascript
console.log(
  (_a = fetchedUserData.job) === null || _a === void 0 ? void 0 : a.title
);
```

### Nullish Coalescing

When dealing with the value that can be null or undefined, nullish coalescing is useful feature to use.

When handling nullish value with vanilla javascript the code is like `const data = userInput || 'DEFAULT'`. In this code, adding to `null` and `undefined`, `''` and `0` can be considered as null value.

Nullish Coalescing using double question mark `??`, only takes `null` and `undefined` as NULL value.

```typescript
const userInput = "";
const data1 = userInput || "DEFAULT";
console.log(data1); // DEFAULT

const data2 = userInput ?? "DEFAULT";
console.log(data2); //
```
