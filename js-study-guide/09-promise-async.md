## 9. Promises and Modern Asynchronous Patterns

Promises provide a structured way to handle asynchronous operations in JavaScript.

---

### 9.1 What is a Promise?

A **Promise** is an object representing the eventual result of an asynchronous operation.

It has **three states**:

| State         | Meaning                          |
| ------------- | -------------------------------- |
| **Pending**   | The operation is still ongoing   |
| **Fulfilled** | Operation completed successfully |
| **Rejected**  | Operation failed                 |

---

### 9.2 Creating a Promise

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Data received!");
  }, 1000);
});

promise.then(result => console.log(result));
```

---

### 9.3 Chaining Promises

```js
fetchData()
  .then(processData)
  .then(saveData)
  .catch(error => console.error(error));
```

* `.then()` returns **a new Promise**, enabling chaining.
* `.catch()` handles errors in the entire chain.

---

### 9.4 Microtasks: Why Promises Run Before `setTimeout`

Promise callbacks (`.then`, `.catch`) go into the **Microtask Queue**, which has **higher priority** than the callback (Macrotask) queue.

```js
console.log("Start");

setTimeout(() => console.log("Macrotask"));

Promise.resolve().then(() => console.log("Microtask"));

console.log("End");
```

**Output:**

```
Start
End
Microtask
Macrotask
```

---

### 9.5 async / await

`async`/`await` is built on top of Promises and allows writing asynchronous code that looks synchronous.

```js
async function loadData() {
  try {
    const response = await fetch("/api/data");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}
```

* `async` functions always return a Promise.
* `await` pauses execution until the Promise settles.

---

### 9.6 Promise Combinators

| Method                   | Behavior                                                             | Use Case                                             |
| ------------------------ | -------------------------------------------------------------------- | ---------------------------------------------------- |
| **Promise.all()**        | Resolves when **all** promises resolve; rejects on **first** failure | Run tasks in parallel where all results are required |
| **Promise.race()**       | Resolves/rejects with the **first** settled promise                  | Timeouts & fastest-response wins                     |
| **Promise.allSettled()** | Resolves when **all** promises settle (success or failure)           | Logging/reporting operations                         |
| **Promise.any()**        | Resolves on **first success**, rejects only if **all fail**          | Useful when only one successful result matters       |




[Resources](https://javascript.info/promise-chaining)

[Resources](https://medium.com/@lydiahallie/javascript-visualized-promises-async-await-a3f1aad8a943)

[Resources](https://www.freecodecamp.org/news/web-page-rendering-on-the-browser-different-methods/)


[Resources](https://bittukumar-web.medium.com/promise-and-its-methods-all-race-any-allsettled-0dc677b5aee1)

[Resources](https://dev.to/alexmercedcoder/understanding-javascript-promises-in-depth-5ga9)

