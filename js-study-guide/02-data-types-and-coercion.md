# Section 2: Data Types, Coercion, and Equality

## Primitive and Non-Primitive Types

### Primitive Types
- string
- number
- boolean
- null
- undefined
- symbol
- bigint

### Non-Primitive
- object (arrays, functions included)

#### `null` vs `undefined`

| Value | Meaning | Assignability |
|-------|---------|---------------|
| `undefined` | Variable declared but not assigned | Happens automatically |
| `null` | Intentional absence of value | Assigned manually |

```js
let x;
console.log(x); // undefined

let y = null;
console.log(y); // null
```

---

## Type Coercion

### Implicit Conversion
```js
console.log(5 + "5"); // "55"
```

### Explicit Conversion
```js
console.log(Number("42")); // 42
```

---

## Equality: `==` vs `===`

| Operator | Description | Converts Types? | Example |
|---------|-------------|----------------|---------|
| `==` | Loose Equality | ✅ Yes | `10 == "10"` → true |
| `===` | Strict Equality | ❌ No | `10 === "10"` → false |

**Use `===` by default.**





### Questions

1.  **List all seven Primitive data types in JavaScript. Why are objects, arrays, and functions considered non-primitive?**
2.  **What is the fundamental difference between `null` and `undefined`? Provide a scenario where explicitly assigning `null` would be appropriate.**
3.  **Explain the result of `typeof null` and why this specific behavior is often cited as a long-standing "bug" in JavaScript.**

### Answers

> **1. Primitive vs. Non-Primitive**
>
> * **Primitive Types:** `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `bigint`.
> * **Non-Primitive Reason:** Non-primitive types (objects, arrays, functions) are **mutable** and are compared by **reference**, not by value. Primitives, conversely, are immutable and compared by value.

> **2. `null` vs `undefined`**
>
> * **`undefined`:** Represents a variable that has been declared but has not been assigned a value. It is assigned automatically by JavaScript.
> * **`null`:** Represents the **intentional absence** of any object value. It must be assigned manually by a developer.
> * **Scenario for `null`:** Clearing a reference to a large object or DOM element after you are done with it to explicitly signal that memory can be freed (though garbage collection handles much of this today).

> **3. The `typeof null` Bug**
>
> * **Result:** `typeof null` returns `"object"`.
> * **Explanation:** This is a bug originating from the early days of JavaScript (Netscape). `null` was intended to be a placeholder for an object reference, and the internal representation checks for object type first. Since fixing this would break vast amounts of existing web code, the bug remains a feature.

---


### Questions

4.  **Define Type Coercion. Provide an example of both Implicit and Explicit Coercion when dealing with numbers and strings.**
5.  **What is the critical difference between the loose equality operator (`==`) and the strict equality operator (`===`)?**
6.  **Without looking at the table, determine the result of `0 == false` and `"" == 0`. Briefly explain the logic behind one of these results.** 

### Answers

> **4. Type Coercion Definition and Examples**
>
> * **Definition:** The automatic (implicit) or manual (explicit) conversion of values from one data type to another.
> * **Implicit (Automatic):** When operators (like `+`, `==`) are used with mixed types.
>    ```javascript
>    console.log(10 + '5'); // Result: "105" (10 is coerced to '10')
>    ```
> * **Explicit (Manual):** When conversion functions are used.
>    ```javascript
>    console.log(String(42)); // Result: "42"
>    ```

> **5. Loose (`==`) vs Strict (`===`) Equality**
>
> * **`===` (Strict Equality):** Compares both the **value** and the **type** without performing any type conversion. This is why it is preferred for predictability.
> * **`==` (Loose Equality):** Compares only the **value** after attempting to convert (coerce) the operands to a common type. This conversion process follows complex internal rules.

> **6. Coercion Results**
>
> * `0 == false` → **`true`**
> * `"" == 0` → **`true`**
> * **Logic:** When comparing a boolean to a number using `==`, the boolean is first coerced to a number (`false` becomes `0`, `true` becomes `1`). Therefore, the comparison becomes `0 == 0`, which is `true`.


### Additional Questions on Data Types and Coercion

7.  **How is non-primitive data (objects, arrays) passed to a function, and why does modifying an object inside a function affect the object outside the function?**
8.  **What does `NaN` stand for, and what two distinct equality checks are unique to `NaN` that do not apply to any other value in JavaScript?**
9.  **Describe the outcome of the following expressions and explain the type coercion process that leads to the result:**
    * `'1' + 2 + 3`
    * `1 + 2 + '3'`
    * `'10' - 5`

### Answers to Additional Questions

> **7. Non-Primitive Data Passing**
>
> * Non-primitive data (objects/arrays) are passed by **Call by Sharing** (or "Call by Reference Value").
> * This means a copy of the **reference (memory address)** is passed to the function. Since both the function parameter and the original variable point to the *same* object in memory, modifying a property of the object inside the function modifies the original object outside.

> **8. Properties of `NaN`**
>
> * `NaN` stands for **Not a Number**. It's the result of an operation that couldn't return a valid number (e.g., `0 / 0`, `Math.sqrt(-1)`).
> * Its two unique equality checks are:
>     1.  `NaN === NaN` evaluates to **`false`**. (It is the only value in JS not strictly equal to itself).
>     2.  `NaN == NaN` also evaluates to **`false`**.
> * To check if a value is genuinely `NaN`, you must use the global function `isNaN()` or, preferably, the more strict ES6 method `Number.isNaN()`.

> **9. Coercion in Complex Expressions**
>
> * **`'1' + 2 + 3`** $\rightarrow$ **`"123"`**
>     * The first operation is `'1' + 2`. Since the first operand is a string, the number `2` is coerced to the string `'2'`. Result: `'12'`.
>     * The second operation is `'12' + 3`. The number `3` is coerced to `'3'`. Final Result: `"123"`.
> * **`1 + 2 + '3'`** $\rightarrow$ **`"33"`**
>     * The first operation is `1 + 2`. Both are numbers, so standard math occurs. Result: `3`.
>     * The second operation is `3 + '3'`. Now, string concatenation occurs. Final Result: `"33"`.
> * **`'10' - 5`** $\rightarrow$ **`5`**
>     * The subtraction (`-`) operator only works on numbers. JavaScript implicitly coerces the string `'10'` to the number `10` before performing the mathematical subtraction.



[Resources](https://medium.com/@mila.mirovic98/javascript-fundamentals-type-conversion-coercion-8bbba10c9925)


[Resources](https://medium.com/@happymishra66/execution-context-in-javascript-319dd72e8e2c)

