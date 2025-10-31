# Section 1: Core Execution and Scope Fundamentals

## How JavaScript Works, Execution Context, and the Global Object

JavaScript is a synchronous, single-threaded language. It executes code line by line using a single Call Stack.

### Execution Context (EC)
The environment where code is evaluated.

- **Creation Phase:** Memory is allocated (hoisting). `var → undefined`, functions → full definition.
- **Execution Phase:** Code runs line-by-line and assigns actual values.

### Global Execution Context & Window Object
When the script loads, JavaScript creates the **Global Execution Context (GEC)** and attaches variables declared with `var` to the **window** object.

```js
var globalMessage = "Hello from the Global Context!";
let localMessage = "I am block-scoped.";

console.log(window.globalMessage); // Hello from the Global Context!
console.log(window.localMessage); // undefined
```

---

### Variable Declaration, Hoisting, and TDZ

| Keyword | Scope | Hoisting Behavior | Re-assignment | Re-declaration |
|--------|-------|------------------|---------------|----------------|
| `var` | Function-scoped | Hoisted to `undefined` | ✅ Allowed | ✅ Allowed |
| `let` | Block-scoped | Hoisted but uninitialized (**TDZ**) | ✅ Allowed | ❌ Not Allowed |
| `const` | Block-scoped | Hoisted but uninitialized (**TDZ**) | ❌ Not Allowed | ❌ Not Allowed |

```js
console.log(a); // undefined
var a = 10;

// console.log(b); // ReferenceError (TDZ)
let b = 20;

greet(); // Hello!
function greet() { console.log("Hello!"); }
```

---

## Scope, Lexical Environment, and Closures

### Scope
Determines variable accessibility. JavaScript uses **lexical scoping**.

### Lexical Environment
A structure containing variable bindings + reference to parent scope.

### Closures
A function that remembers the scope in which it was created, even after that scope has finished execution.

```js
function makeCounter() {
  let count = 0;
  return function() {
    count++;
    console.log(count);
  };
}

const counterA = makeCounter();
counterA(); // 1
counterA(); // 2
```




### Questions

1.  **Describe the difference between JavaScript being "single-threaded" and "synchronous." How do these characteristics relate to the Call Stack?** 
2.  **What are the two distinct phases of the Execution Context lifecycle, and what role does Hoisting play in the first phase?**
3.  **Explain the difference in how `var` and `let`/`const` variables behave when declared globally, specifically concerning their attachment to the `window` object in a browser environment.**

### Answers

> **1. Single-threaded vs. Synchronous**
>
> * **Single-threaded:** Means the JS engine has only **one Call Stack** to execute code. It can only do one thing at a time.
> * **Synchronous:** Means code is executed **sequentially, line by line**, blocking the Call Stack until the current operation is finished.
> * **Relation to Call Stack:** The Call Stack manages the synchronous execution flow, placing one function call or operation on the stack at a time.

> **2. Execution Context Phases**
>
> * **Creation Phase (Memory Allocation):** This is where **Hoisting** occurs. The JS engine scans the code and allocates memory for variables and functions.
>     * `var` variables are initialized to `undefined`.
>     * Functions are stored with their full definitions.
> * **Execution Phase:** Code is run line-by-line, and variables are assigned their actual values.

> **3. Global Variable Behavior**
>
> * **`var`:** When declared globally, `var` creates a property on the global object, which is the `window` object in browsers. Thus, you can access it via `window.variableName`.
> * **`let`/`const`:** When declared globally, they are block-scoped to the Global Execution Context but **do not** attach to the `window` object. They exist in a separate memory space, making them inaccessible via `window.variableName`.

---


4.  **Define Hoisting. What is the key difference in the hoisting mechanism between `var` (which is initialized) and `let`/`const` (which are not)?**
5.  **What is the Temporal Dead Zone (TDZ), and what is the specific error message generated when you attempt to access a variable within the TDZ?**
6.  **Why is `let` generally preferred over `var` in modern JavaScript development, considering factors like scope and accidental re-declaration?**

### Answers

> **4. Hoisting and Initialization**
>
> * **Hoisting:** The engine moves the **declarations** of variables and functions to the top of their containing scope during the Creation Phase.
> * **Key Difference:**
>     * `var` is hoisted and **initialized** to `undefined`.
>     * `let`/`const` are hoisted but **not initialized**. They remain in the Temporal Dead Zone until the actual line of declaration is executed.

> **5. Temporal Dead Zone (TDZ)**
>
> * **Definition:** The period of time between the variable's creation (start of the scope) and its initialization (where the declaration code is run). During this period, the variable cannot be accessed.
> * **Error Message:** Attempting to access a variable in the TDZ results in a `ReferenceError: Cannot access 'variableName' before initialization`.

> **6. Why `let` is preferred**
>
> * **Block Scoping:** `let` limits a variable's scope to the nearest curly braces (`{}`), preventing variables from leaking into parent scopes (unlike function-scoped `var`).
> * **Prevents Accidental Bugs:** `let` prevents accidental re-declaration within the same scope, and its TDZ mechanism encourages better programming practices by forcing declarations to be at the top of their blocks, reducing bugs related to unintended hoisting.

---


### Questions

7.  **What is Lexical Scoping, and how does it determine a function's access to variables in its parent scopes?**
8.  **What is a Closure? Provide an example of a common pattern where closures are essential for maintaining private state or data encapsulation.**
9.  **In the `makeCounter` example, why does the inner function retain access to and the updated value of the `count` variable, even though the `makeCounter` function has finished executing?**

### Answers

> **7. Lexical Scoping**
>
> * **Definition:** Scoping determined by where a function is **defined** (written in the code), not where it is called.
> * **Variable Access:** A function's Lexical Environment retains a reference to its outer (parent) environment. This chain of references (the Scope Chain) allows the function to look up and access variables in its parent scopes.

> **8. Closure and Use Cases**
>
> * **Definition:** A function bundled together with (and remembering) its Lexical Environment. The function "closes over" the variables of its parent scope.
> * **Use Cases:**
>     * **Data Encapsulation / Private Variables:** Used to create variables accessible only by the closure, acting like private members.
>     * **Functional Programming / Currying:** Maintaining intermediate state across function calls.

> **9. Closure Mechanism in `makeCounter`**
>
> * The inner function (`return function() { ... }`) forms a closure over the Lexical Environment of `makeCounter`.
> * Even after `makeCounter()` finishes and is popped off the Call Stack, its Lexical Environment (which contains the `count` variable) is kept alive in memory because the inner function still holds a reference to it. This allows the inner function to access and update the shared `count` state across multiple calls.






### Q1. What is the output?
```js
function outer() {
  let count = 0;
  return function () {
    return ++count;
  };
}

const counter = outer();
console.log(counter());
console.log(counter());
console.log(counter());
```

**Answer:**
```
1
2
3
```

---

### Q2. Output Question
```js
function createFunctions() {
  let result = [];
  for (var i = 1; i <= 3; i++) {
    result.push(function () {
      console.log(i);
    });
  }
  return result;
}

const arr = createFunctions();
arr[0]();
arr[1]();
arr[2]();
```

**Answer:**
```
3
3
3
```

**Reason:** `var` is function-scoped. All functions share the same final `i` value.

---

### Q3. Fix to print 1, 2, 3
```js
for (let i = 1; i <= 3; i++) {
  result.push(() => console.log(i));
}
```

or

```js
for (var i = 1; i <= 3; i++) {
  (function(i){ result.push(() => console.log(i)); })(i);
}
```

---


### Q4. Output?
```js
function sum(a) {
  return function (b) {
    return function (c) {
      console.log(a + b + c);
    };
  };
}

sum(1)(2)(3);
```

**Answer:**
```
6
```

---

### Q5. Convert function to curried version
```js
function multiply(a, b, c) {
  return a * b * c;
}
```

**Solution:**
```js
const multiply = a => b => c => a * b * c;
console.log(multiply(2)(3)(4)); // 24
```

---



### Q6. Output?
```js
console.log(a);
var a = 10;
```

**Answer:**
```
undefined
```

---

### Q7. Output?
```js
console.log(b);
let b = 10;
```

**Answer:**
```
ReferenceError: Cannot access 'b' before initialization
```

---

### Q8. Output?
```js
function test() {
  var x = 1;
  if (true) {
    var x = 2;
    console.log(x);
  }
  console.log(x);
}
test();
```

**Answer:**
```
2
2
```

---

### Q9. Output?
```js
function test() {
  let x = 1;
  if (true) {
    let x = 2;
    console.log(x);
  }
  console.log(x);
}
test();
```

**Answer:**
```
2
1
```

---



### Q10. Output?
```js
var x = 10;
let y = 20;
const z = 30;

x = 40;
y = 50;
z = 60;

console.log(x, y, z);
```

**Answer:**
```
TypeError: Assignment to constant variable.
```

---

## 5) this + Closures

### Q11. Output?
```js
var name = "Global";

const obj = {
  name: "Object",
  getName: function () {
    return function () {
      console.log(this.name);
    };
  },
};

obj.getName()();
```

**Answer:**
```
Global
```

---

### Fix using arrow function
```js
getName: function () {
  return () => console.log(this.name);
}
```

**Output:**
```
Object
```



[Read More](https://www.freecodecamp.org/news/scope-closures-and-hoisting-in-javascript/)


[Read More](https://medium.com/@happymishra66/execution-context-in-javascript-319dd72e8e2c)


