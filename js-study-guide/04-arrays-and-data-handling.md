# Section 4: Arrays, Copying, and Data Handling

JavaScript arrays and objects are essential building blocks. Understanding how data is stored, referenced, and manipulated is critical for writing efficient and bug-free code.

---

## 1. Common Array Methods

| Method   | Returns        | Mutates Original? | Purpose                                    |
|---------|----------------|------------------|--------------------------------------------|
| `map()`   | New array       | ❌ No             | Transform each item and return new array     |
| `filter()`| New array       | ❌ No             | Keep only items that satisfy a condition     |
| `reduce()`| Single value    | ❌ No             | Accumulate values into one result            |
| `slice()` | Portion copy    | ❌ No             | Extract a portion of the array               |
| `splice()`| Removed items   | ✅ Yes            | Add/remove items and **modify original**     |

### Example

```js
const arr = [1, 2, 3];

console.log(arr.map(x => x * 2)); // [2, 4, 6]
console.log(arr.filter(x => x % 2 !== 0)); // [1, 3]
console.log(arr.reduce((sum, x) => sum + x, 0)); // 6
console.log(arr.slice(1, 3)); // [2, 3]
console.log(arr.splice(1, 1)); // [2] (original array becomes [1, 3])
```

---

## 2. Value vs Reference

JavaScript stores **primitive values** directly and **objects by reference**.

### Primitive Example (Value Copy)
```js
let a = 5;
let b = a;
b = 10;

console.log(a); // 5
console.log(b); // 10
```

### Object Example (Reference Copy)
```js
const obj1 = { x: 1 };
const obj2 = obj1;
obj2.x = 99;

console.log(obj1.x); // 99 (because both refer to the same object)
```

---

## 3. Shallow vs Deep Copy

| Copy Type | Copies Nested Objects? | Methods |
|----------|------------------------|---------|
| Shallow Copy | ❌ No | spread (`...`), `Object.assign()` |
| Deep Copy | ✅ Yes | `structuredClone()`, JSON method |

### Shallow Copy Example
```js
const original = { a: 1, b: { c: 2 } };
const shallow = { ...original };
shallow.b.c = 99;

console.log(original.b.c); // 99 (changed) ✅
```

### Deep Copy Example
```js
const original = { a: 1, b: { c: 2 } };
const deep = JSON.parse(JSON.stringify(original));
deep.b.c = 99;

console.log(original.b.c); // 2 (unchanged) ✅
```

---

## 4. Most Asked Interview Questions

### Q1: What is the difference between `slice()` and `splice()`?
| slice() | splice() |
|--------|---------|
| Does **not** modify original array ❌ | **Modifies** original array ✅ |
| Returns extracted portion | Removes or inserts elements |
| Used for copying portions | Used for editing arrays |

---

### Q2: Why does modifying one object affect another?
**Because objects are assigned by reference**, not copied.

---

### Q3: How to create a real deep copy?

```js
const deep = structuredClone(obj);
```

---

## 5. Output-Based Coding Questions

### Question 1
```js
const arr = [1, 2, 3];
const arr2 = arr;
arr2.push(4);
console.log(arr);
```
**Answer:** `[1, 2, 3, 4]` (same reference)

---

### Question 2
```js
const obj = { a: 1 };
const copy = { ...obj };
copy.a = 99;
console.log(obj.a);
```
**Answer:** `1` (shallow copy okay since value is primitive)

---

### Question 3
```js
const original = { a: { b: 1 } };
const shallow = { ...original };
shallow.a.b = 9;
console.log(original.a.b);
```
**Answer:** `9` (nested object reference shared)

---



[Resources](https://javascript.info/array)