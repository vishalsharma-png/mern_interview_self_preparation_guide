# Section 3: Objects and Prototypal Inheritance

## Object Creation Methods

| Method | Example | Notes |
|--------|---------|-------|
| Object Literal | `const obj = {};` | Most common |
| Constructor Function | `function Car(){}` + `new Car()` | Before ES6 |
| ES6 Class | `class Car {}` | Cleaner syntax |
| Object.create() | `Object.create(proto)` | Create object with chosen prototype |

```js
const person = {
  name: "Alice",
  greet() {
    console.log(\`Hello, my name is ${this.name}.\`);
  }
};
person.greet();
```

---

## Prototype Chain

```js
function Vehicle(type) {
  this.type = type;
}

Vehicle.prototype.honk = function() {
  console.log(\`The ${this.type} is honking!\`);
};

const car = new Vehicle("Sedan");
car.honk();
```

| Term | Meaning | Used On | Purpose |
|------|---------|--------|---------|
| `prototype` | Shared methods storage | Constructor functions | Enables inheritance |
| `__proto__` | Internal prototype reference | Objects | Links to parent's prototype |

---

## ES6 Classes

```js
class Animal {
  constructor(name) { this.name = name; }
  speak() { console.log(\`${this.name} makes a sound.\`); }
}

const dog = new Animal("Dog");
dog.speak();
```




# Section 3: Objects and Prototypal Inheritance

## What is an Object?
In JavaScript, objects are collections of key-value pairs used to store data and behavior. Each key is a **string** or **symbol**, and each value can be **any data type**, including another object or function.

```js
const user = {
  name: "Rahul",
  age: 22,
  greet() {
    console.log("Hello!");
  }
};
```

Objects are dynamic — properties can be added or removed at runtime.

---

## Ways to Create Objects
### 1. Object Literal (Most Common)
```js
const obj = { a: 10, b: 20 };
```

### 2. Using `new Object()`
```js
const obj = new Object();
obj.a = 10;
```

### 3. Constructor Function
```js
function Person(name) {
  this.name = name;
}
const p1 = new Person("Aman");
```

### 4. `Object.create()` (Prototype-based creation)
```js
const proto = { greet() { console.log("Hello"); } };
const obj = Object.create(proto);
obj.name = "Kiran";
```

---

## Prototype and `__proto__`

Every object in JavaScript has an internal hidden property `[[Prototype]]` (commonly accessed using `__proto__`).

This property points to another object, forming a chain known as the **Prototype Chain**.

```
obj → __proto__ → Object.prototype → null
```

### Why is this useful?
It allows **shared methods** across objects, saving memory and supporting inheritance.

---

## Prototypal Inheritance Example

```js
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function() {
  console.log(this.name + " makes a sound");
};

const dog = new Animal("Tommy");
dog.speak(); // Tommy makes a sound
```

`dog` inherits `speak` from `Animal.prototype`.

---

## ES6 Classes (Syntactic Sugar)

```js
class Vehicle {
  constructor(name) {
    this.name = name;
  }
  move() {
    console.log(this.name + " is moving");
  }
}

class Car extends Vehicle {
  drive() {
    console.log(this.name + " is driving");
  }
}

const c = new Car("Tesla");
c.move();
c.drive();
```

Classes internally use prototypes.

---

## ✅ PROTOTYPE CHAIN VISUALIZED

```
Car object (instance)
   |
   → Car.prototype
       |
       → Vehicle.prototype
           |
           → Object.prototype
               |
               → null
```

---



### 1. What is `prototype` in JavaScript?

**Answer:**  
`prototype` is a property available on functions that define properties and methods to be inherited by instances created from that function.

---

### 2. What is the difference between `__proto__` and `prototype`?

| `prototype` | `__proto__` |
|------------|-------------|
| Belongs to functions | Belongs to all objects |
| Used to define inheritance | Used to access inherited properties |
| Controls what instances inherit | Forms the prototype chain |

---

### 3. Output-Based Question (Prototype sharing)

```js
function Test() {}
Test.prototype.value = 100;

const a = new Test();
const b = new Test();

a.value++;
console.log(a.value, b.value);
```

**Answer:**  
```
101 100
```
Because `a.value++` creates a new property on `a`, shadowing the prototype.

---

### 4. Output-Based Question

```js
const obj1 = { name: "A" };
const obj2 = Object.create(obj1);
obj2.name = "B";

console.log(obj1.name, obj2.name);
```

**Answer:**  
```
A B
```
`obj2.name` shadows the prototype property; original remains unchanged.

---

### 5. Why use `Object.create()`?

**Answer:**  
`Object.create()` allows us to define an object's prototype directly, enabling **pure prototypal inheritance** without classes or constructor functions.

---

### 6. Tricky Prototype Question

```js
function X() {}
X.prototype = { a: 1 };

const y = new X();
console.log(y.a);

X.prototype.a = 2;
console.log(y.a);
```

**Answer:**  
```
1
2
```
Because `y` references the *same prototype object*.

---


[Resources](https://mostafizur99.medium.com/understanding-prototypes-in-javascript-the-backbone-of-inheritance-dec184153727)


[Resources](https://javascript.info/object)




