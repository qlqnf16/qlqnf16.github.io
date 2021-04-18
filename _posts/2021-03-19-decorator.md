---
layout: post
title: "TS: Decorators"
tags: [ts]
---

Understanding TypeScript : Section 8

---

Decorator is a feature that is useful for meta-programming. Meta-programming is something that has little impact on end-users, instead has more impact on developers when exporting a code or making a tool.

Decorator is basically a **function**, so you can make decorator and use it like below.

```typescript
function Logger(constructor: Function) {
  console.log("LOGGING");
  console.log(constructor);
}

@Logger
class Person {
  name = "Max";

  constructor() {
    console.log("Creating person object...");
  }
}

// LOGGING
// Person {name: "Max"}
```

- Decorator is executed with `@` symbol.
- It runs when a class is defined, when javascript finds a class definition.
- Decorator must get one parameter typed Function

## Factory Function

If you want to hand in some parameter to the decorator, you can make it factory function, or factory decorator.

```typescript
function Logger(logString: string) {
  return function(_: Function) {
    console.log(logString);
  }
}

@Logger('Logging Person')
class Person {
...
}

// Logging Person
```

## Using Multiple Decorators

You can use multiple decorators together in one class. Decorator function executes in order from bottom to top.

```typescript
function Logger(_: Function) {
  console.log('LOGGER');
}
function WitheTemplate(_: Function) {
  console.log('TEMPLATE');
}

@Logger
@WitheTemplate
class Person {
...
}

// TEMPLATE
// LOGGER
```

## Usage

You can use decorators with not only classes but also other things inside the class, such as properties, accessors, or methods.

- Property Decorators

  gets two parametres, `target` and `propertyName`. The `target` can be either the prototype of the object or the constructor.

  ```typescript
  function Log(target: any, propertyName: string | Symbol) {}
  ```

- Accessor Decoratros

  gets three parameters, first two are the same with the one obove, and the third parameter is a property descriptor.

  ```typescript
  function Log2(target: any, name: string, descriptor: PropertyDescriptor) {}
  ```

- Method Decorators

  ```typescript
  function Log3(target: any, name: string, descriptor: PropertyDescriptor) {}
  ```

- Parameter Decorators

  third parameter is a position, which indicates the position of tha parameter.

  ```typescript
  function Log4(target: any, name: string | Symbol, position: number) {}
  ```

Each decorators can be located in the code like below.

All the decorators are executed when the class is defined. Decorators allow you to do additional behind the scene setup work (meta-programming).

```typescript
class Product {
  @Log
  title: string;
  private _price: number;

  @Log2
  set price(val: number) {
    if (val > 0) {
      this._price = val;
    } else {
      throw new Error("Invalid Price");
    }
  }

  constructor(t: string, p: number) {
    this.title = t;
    this._price = p;
  }

  @Log3
  getPriceWithTax(@Log4 tax: number) {
    return this._price * (1 + tax);
  }
}
```

## Returnign Value with Decorator

function decorator and accessor decorator can return a value.

PropertyDescriptor

```typescript
function Autobind(_: any, _2: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  const adjDescriptor: PropertyDescriptor = {
    configurable: true,
    enumerable: false,
    get() {
      const boundFn = originalMethod.bind(this);
      // this refers to the object that triggers the gette, the originally defined the method.
      return boundFn;
    },
  };
  return adjDescriptor;
}

class Printer {
  message = "This Works!";

  @Autobind
  showMessage() {
    console.log(this.message);
  }
}

const p = new Printer();

const button = document.querySelector("button")!;
button.addEventListener("click", p.showMessage);
```
