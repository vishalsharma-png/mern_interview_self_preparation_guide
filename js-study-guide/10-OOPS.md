## 10. Classes and Object-Oriented Patterns in JavaScript

JavaScript uses **prototypal inheritance**, and ES6 `class` syntax is syntactic sugar over prototypes.

---

### 10.1 ES6 Class Syntax

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  introduce() {
    console.log(`Hi, I'm ${this.name} and I'm ${this.age} years old.`);
  }
}

const user = new Person("Ava", 22);
user.introduce();
```

---

### 10.2 Class Inheritance

```js
class Animal {
  speak() {
    console.log("Animal makes sound");
  }
}

class Dog extends Animal {
  speak() {
    console.log("Bark!");
  }
}

const d = new Dog();
d.speak(); // Bark!
```

`extends` allows a class to inherit methods from another class.

---

### 10.3 super Keyword

Used to call the parent constructor or parent methods.

```js
class Vehicle {
  constructor(type) {
    this.type = type;
  }
}

class Car extends Vehicle {
  constructor(type, model) {
    super(type);
    this.model = model;
  }
}
```

---

### 10.4 Getters and Setters

```js
class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name.toUpperCase();
  }

  set name(value) {
    this._name = value;
  }
}
```

---

### 10.5 Static Methods

Static methods belong to the **class**, not objects.

```js
class MathUtil {
  static add(a, b) {
    return a + b;
  }
}

console.log(MathUtil.add(2, 3)); // 5
```

---

### 10.6 Prototype Link Recap

* Instances inherit from `ClassName.prototype`
* Classes themselves are functions internally

```js
console.log(typeof Person); // function
console.log(Object.getPrototypeOf(user)); // Person.prototype
```

---

Section 10 complete ✅
