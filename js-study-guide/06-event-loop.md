# Section 6: Asynchronous JavaScript and the Event Loop

JavaScript is **single-threaded**, meaning it has only **one call stack** and can execute **one piece of code at a time**. However, JavaScript can still handle asynchronous behavior efficiently using the **Event Loop**, which coordinates tasks between the JavaScript engine and the browser (Web APIs).

---

## 6.1 The Call Stack

The **Call Stack** is a data structure that keeps track of which function is currently running and where execution should return.

When a function is called, it is **pushed onto the stack**. When it completes, it is **popped off**.

```
+-------------------+
|   showMessage()   | ← Top (currently running)
|   greet()         |
|   main()          |
+-------------------+
```

### Key Idea
- JavaScript executes code **synchronously** on the call stack.
- Long-running operations would **block** the stack, so they are offloaded to **Web APIs**.

---

## 6.2 Web APIs (Browser Environment)

Some functions are *not* executed directly in the main thread. Instead, they are delegated to the **Browser Web APIs**:

| Function Type | Examples | Who Handles It? |
|--------------|----------|----------------|
| Timer Tasks  | `setTimeout`, `setInterval` | Timer Web API |
| Network Calls | `fetch` | Network Web API |
| DOM Events | `addEventListener` | Event Web API |

```
JavaScript Engine (Call Stack)          Browser Web APIs
-------------------------------        -------------------
           setTimeout()     ------→       Timer System
           fetch()          ------→       Network System
```

These API operations happen **asynchronously** and **do not block** the call stack.

---

## 6.3 Callback Queue and the Event Loop

Once a Web API finishes its work, it sends the callback to the **Callback Queue**.

The **Event Loop** constantly checks:

> **If the Call Stack is empty → push the next callback into the Call Stack.**

```
Callback Queue → [ callback1, callback2, callback3 ]
                   ↑
                   | (Event Loop)
                   |
+-------------------+
|     Call Stack    |
+-------------------+
```

This mechanism allows JavaScript to appear concurrent despite being single-threaded.

---

## 6.4 Microtasks vs Macrotasks

JavaScript has **two** types of task queues:

| Queue Type | Priority | Examples | Notes |
|----------|----------|----------|------|
| **Microtask Queue** | **Higher** | `Promise.then`, `queueMicrotask` | Always executed **before** macrotasks |
| **Macrotask Queue** | Lower | `setTimeout`, `setInterval`, UI events | Executes **after microtasks finish** |

### Example:

```js
console.log("Start");

setTimeout(() => console.log("Macrotask"), 0);

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

Because **Microtasks run before Macrotasks**.

---

## 6.5 Most Asked Interview Questions

### Q1: Why doesn't `setTimeout(fn, 0)` run immediately?
**Answer:** Because its callback goes to the **macrotask queue**, which only executes **after** all microtasks and synchronous code finish.

---

### Q2: What is the difference between a Microtask and a Macrotask?
**Answer:** Microtasks (Promises) have **higher execution priority** and always run before Macrotasks (Timers, UI events).

---

### Q3: Explain the Event Loop in one line.
**Answer:** The Event Loop continuously checks if the Call Stack is empty and, if so, pushes tasks from the task queues to execute.

---

### Q4: Does JavaScript do multithreading?
**Answer:** No, JavaScript is **single-threaded**, but the browser provides asynchronous capabilities through **Web APIs**.

---

## 6.6 Practice Output Questions With Explanations

### Question 1:
```js
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

console.log("D");
```
**Output:**
```
A
D
C
B
```
**Explanation:** Promise (microtask) runs before `setTimeout` (macrotask).

---

### Question 2:
```js
setTimeout(() => console.log("Timer"), 0);

for(let i = 0; i < 1000000000; i++) {}

console.log("Done");
```
**Output:**
```
Done
Timer
```
**Explanation:** The loop blocks the call stack, delaying the timeout.

---

## Additional Event Loop Practice Questions (Important!)

### Q3:
```js
console.log("Start");

setTimeout(() => console.log("Timeout"), 0);

Promise.resolve().then(() => console.log("Promise"));

console.log("End");
```
**Output:**
```
Start
End
Promise
Timeout
```

---

### Q4:
```js
Promise.resolve().then(() => {
  console.log("Microtask 1");
  Promise.resolve().then(() => console.log("Microtask 2"));
});

setTimeout(() => console.log("Macrotask"), 0);
```
**Output:**
```
Microtask 1
Microtask 2
Macrotask
```

---

### Q5:
```js
console.log("1");

setTimeout(() => console.log("2"), 100);

setTimeout(() => console.log("3"), 0);

console.log("4");
```
**Output:**
```
1
4
3
2
```

---

### Q6:
```js
setTimeout(() => console.log("A"), 0);
Promise.resolve().then(() => console.log("B"));
console.log("C");
```
**Output:**
```
C
B
A
```

---

### Q7:
```js
console.log("X");

Promise.reject().catch(() => console.log("Y"));

console.log("Z");
```
**Output:**
```
X
Z
Y
```
**Explanation:** `.catch()` is also a microtask.

---

### Q8:
```js
setTimeout(() => console.log("Timeout 1"), 0);

Promise.resolve().then(() => {
  console.log("Promise 1");
  setTimeout(() => console.log("Timeout 2"), 0);
});

Promise.resolve().then(() => console.log("Promise 2"));
```
**Output:**
```
Promise 1
Promise 2
Timeout 1
Timeout 2
```

---

### Q9:
```js
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

for(let i = 0; i < 3; i++) {
  Promise.resolve().then(() => console.log("Promise"));
}

console.log("End");
```
**Output:**
```
Start
End
Promise
Promise
Promise
Timeout
```

---

### Q10:
```js
async function test() {
  console.log("1");
  await Promise.resolve();
  console.log("2");
}
console.log("3");
test();
console.log("4");
```
**Output:**
```
3
1
4
2
```




[Resources](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=113s)

[Resources](https://www.youtube.com/watch?v=8zKuNo4ay8E)

[Resources](https://www.greatfrontend.com/questions/quiz/what-is-event-loop-what-is-the-difference-between-call-stack-and-task-queue)


[Event Loop Visualizer](https://www.jsv9000.app/)

