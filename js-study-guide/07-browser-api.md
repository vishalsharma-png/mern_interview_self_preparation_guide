
# Section 7: Browser APIs and Web Fundamentals

The **Browser Environment** provides additional functionality that JavaScript alone does not have. These built-in features are known as **Web APIs**, and they allow JavaScript to interact with the UI, network, storage, and more.

---

## 7.1 DOM (Document Object Model)

The DOM represents the structure of a webpage as a **tree of nodes**, where every HTML element becomes a JavaScript object that you can manipulate.

### Selecting and Updating Elements

```js
const heading = document.getElementById("title");
heading.textContent = "Updated Title!";
```

### Creating & Appending Elements

```js
const p = document.createElement("p");
p.textContent = "New paragraph added.";
document.body.appendChild(p);
```

---

## 7.2 Events and Event Flow

When something happens on a page (click, scroll, key press), the browser dispatches an **event**.

### Event Flow Stages

| Stage | Description |
|------|-------------|
| **Capturing Phase** | Event travels from `window → document → html → body → ... → target element` |
| **Target Phase** | Event reaches the element where it occurred |
| **Bubbling Phase** | Event travels back outward from the target to `window` |

### Example Listener

```js
button.addEventListener("click", function(event) {
  console.log("Button was clicked");
});
```

---

## 7.3 Bubbling and Capturing

You can control event flow using the third parameter of `addEventListener`:

```js
element.addEventListener("click", handler, true); // Capturing
element.addEventListener("click", handler, false); // Bubbling (default)
```

---

## 7.4 Event Propagation Control

### Prevent Bubbling
```js
event.stopPropagation();
```

### Prevent Default Browser Behavior
```js
event.preventDefault();
```

---

## 7.5 `target` vs `currentTarget`

| Property | Meaning |
|---------|---------|
| **event.target** | The *actual* element clicked |
| **event.currentTarget** | The element *whose listener is executing* |

```js
document.querySelector("ul").addEventListener("click", function(event) {
  console.log("target:", event.target); // actual clicked li
  console.log("currentTarget:", event.currentTarget); // the ul
});
```

---

## 7.6 Event Delegation (Very Important!)

Instead of adding listeners to every child element → Add **one listener** to their parent.

```js
document.querySelector("ul").addEventListener("click", function(event) {
  if (event.target.tagName === "LI") {
    console.log("Clicked:", event.target.textContent);
  }
});
```

✅ Efficient  
✅ Works for dynamically added items

---

## 7.7 Browser Storage

| Storage Type | Persistence | Capacity | Sent in HTTP Requests? | Use Case |
|-------------|-------------|----------|------------------------|----------|
| **Cookies** | Optional expiration | ~4 KB | ✅ Yes | Authentication |
| **localStorage** | Until manually cleared | ~5–10 MB | ❌ No | User settings, caching |
| **sessionStorage** | Until tab closes | ~5–10 MB | ❌ No | Session state |

---

## 7.8 Fetch API

Used to make network requests:

```js
fetch("/api/data")
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

---

# Interview Questions & Output Exercises

### Q1: What is Event Bubbling?
**Answer:** The event travels from the target element upward to the window.

---

### Q2: What is Event Capturing?
**Answer:** The event travels from the window down to the target element before bubbling.

---

### Q3: What is `event.stopPropagation()`?
**Answer:** Prevents the event from continuing to bubble or capture.

---

### Q4: Output Based Question

```html
<div id="parent">
  <button id="child">Click Me</button>
</div>
```
```js
document.getElementById("parent").addEventListener("click", () => console.log("Parent"));
document.getElementById("child").addEventListener("click", () => console.log("Child"));
```

**Output when clicking button:**
```
Child
Parent
```

---

### Q5: Stop Propagation Example

```js
document.getElementById("child").addEventListener("click", (e) => {
  e.stopPropagation();
  console.log("Child Only");
});
```
**Output when clicking button:**
```
Child Only
```

---

### Q6: `target` vs `currentTarget` Example

```js
document.querySelector("ul").addEventListener("click", function(event) {
  console.log(event.target.tagName);
  console.log(event.currentTarget.tagName);
});
```

If clicking `<li>`:
```
LI
UL
```

---

### Q7: DOM Manipulation Practice Task (Important)
Write JavaScript to:
- Add a new `<li>` to a list when a button is clicked
- Use **event delegation** so the new item also works on click

*(Student exercises section)*

---


[Resources](https://thenomadtechie.medium.com/mastering-javascript-event-handling-techniques-bubbling-capturing-delegation-and-propagation-0cdbe56f0b39)

[Resources](https://www.greatfrontend.com/questions/quiz/describe-event-bubbling)

[Resources](https://www.greatfrontend.com/questions/quiz/what-is-event-loop-what-is-the-difference-between-call-stack-and-task-queue)


[Resources](https://www.freecodecamp.org/news/dom-events-and-javascript-event-listeners/)