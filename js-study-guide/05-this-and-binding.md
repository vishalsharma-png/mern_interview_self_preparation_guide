# Section 5: The `this` Keyword and Binding

The `this` keyword in JavaScript refers to the *execution context* — the object that is currently calling or owning the function. Its value depends on **how** a function is *invoked*, not where it is written.

JavaScript provides multiple rules to determine what `this` refers to. Understanding these rules is crucial for debugging and writing clean code.

---

## 1. Default Binding (Global Context)

When a function is called **without any owning object**, `this` refers to:

- `window` (in browsers)
- `global` / `undefined` in strict mode (Node.js)

```js
function show() { 
  console.log(this); 
}
show(); 
```
If not in strict mode → logs the global object.

---

## 2. Implicit Binding (Object Method Call)

If a function is called as a **method on an object**, `this` refers to the object owning the method.

```js
const user = {
  name: "Ava",
  greet() { 
    console.log(this.name); 
  }
};
user.greet(); // Ava
```

Here, `this` → `user`

---

## 3. Explicit Binding (call, apply, bind)

We can *manually* set `this` using:

| Method | Executes Immediately? | Argument Format | Use Case |
|-------|----------------------|----------------|---------|
| `call()` | ✅ Yes | Comma-separated list | Pass parameters individually |
| `apply()` | ✅ Yes | Array | Useful when arguments are already in an array |
| `bind()` | ❌ No (returns new function) | Comma-separated list | Use when you want to *store* a bound function for later |

```js
function greet(msg) { 
  console.log(msg, this.name); 
}

const obj = { name: "Liam" };

greet.call(obj, "Hi");   // Hi Liam
greet.apply(obj, ["Hey"]); // Hey Liam

const newFn = greet.bind(obj, "Hello");
newFn(); // Hello Liam
```

---

## 4. Arrow Functions and Lexical `this`

Arrow functions **do not create their own `this`**.  
Instead, they inherit `this` from their surrounding (parent) scope.

```js
const obj = {
  value: 42,
  show() {
    setTimeout(() => console.log(this.value), 100);
  }
};
obj.show(); // 42
```

If `setTimeout` had used a normal function, `this` would be `window` and output undefined.

---

## Common Mistake Example

```js
const user = {
  name: "Eve",
  show: () => console.log(this.name)
};

user.show(); // undefined (not "Eve")
```
Arrow functions are **not recommended** for object methods when using `this`.

---

## Interview-Level Questions

### Q1: What is `this` inside arrow functions?
**Answer:** Arrow functions do not have their own `this`. They lexically inherit `this` from the surrounding scope.

---

### Q2: What is the difference between `call` and `bind`?
**Answer:**
- `call()` invokes the function immediately.
- `bind()` returns a new function with a permanently bound `this` but does **not** execute it immediately.

---

### Q3: What will be the output?
```js
var name = "Global";

const obj = {
  name: "Local",
  say() {
    console.log(this.name);
  }
};

const ref = obj.say;
ref();
```
**Answer:** `"Global"`  
Reason: `ref` is called without an object → default binding applies.

---

### Q4: Output-based Trick Question
```js
const person = {
  name: "John",
  greet: function () {
    return function () {
      console.log(this.name);
    };
  }
};

const fn = person.greet();
fn();
```
**Answer:** `undefined` (or global value in non-strict mode)  
Reason: inner function loses the outer object’s `this`.

To fix:
```js
const fn2 = person.greet().bind(person);
fn2(); // John
```

---

## Summary Table

| Invocation Type | Determines `this` From | Example |
|----------------|----------------------|---------|
| Default Binding | Global / undefined | `fn()` |
| Implicit Binding | Object owning the function | `obj.fn()` |
| Explicit Binding | Manually passed | `fn.call(obj)` |
| Arrow Function | Lexical (parent scope) | `() => this` |

---



[Resources](https://www.freecodecamp.org/news/javascript-this-keyword-binding-rules/)

[Resources](https://dev.to/codecraftjs/understanding-call-apply-and-bind-essential-methods-in-javascript-d62)