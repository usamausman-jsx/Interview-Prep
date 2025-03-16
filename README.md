
# 🚀 Frontend & System Design Interview Guide

This guide covers essential topics for **Frontend, JavaScript, React, System Design, Performance Optimization, and Security**. It includes key concepts, best practices, and coding challenges to help you ace your technical interviews.  



## 1. JavaScript Fundamentals

This section covers core JavaScript concepts like hoisting, closures, scope, execution context, and prototypes.

### 1.1 What is a JavaScript engine, and how does it work?
A JavaScript engine is a program that executes JavaScript code. It converts JavaScript into machine code for execution.

#### Examples: V8 (Chrome, Node.js), SpiderMonkey (Firefox), Chakra (Edge).
How it works:
- Parsing – Converts JS code into an Abstract Syntax Tree (AST).
- Compilation – Uses Just-In-Time (JIT) compilation for faster execution.
- Execution – Runs the optimized machine code.

```javascript
console.log("Hello, World!"); 
// This code runs inside the JS engine, compiled and executed by V8 in Chrome.
```

### 1.2 What is Hoisting, and how does JavaScript handle it?
Hoisting is JavaScript's behavior of moving function and variable declarations to the top of their scope before execution.
#### 👉 Example:

```javascript
console.log(a); // undefined
var a = 10; // Declaration is hoisted but not initialization

foo(); // Works due to function hoisting
function foo() { console.log("Hoisted function!"); }
```
- Let & Const are not hoisted in the same way:

```javascript
console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 20;
```

- var is hoisted but initialized as undefined.
- let and const are hoisted but remain in the Temporal Dead Zone until assigned.

### 1.3 What is the difference between == and === in JavaScript?
- == (loose equality) compares values with type conversion.
- === (strict equality) compares both values and types.

#### 👉 Example:

```javascript
console.log(5 == "5");  // true (string "5" is converted to a number)
console.log(5 === "5"); // false (different types)
console.log(null == undefined); // true
console.log(null === undefined); // false
```

✅ Best Practice: Always use === to avoid unexpected type coercion.

### 1.4 Explain Closures with an Example.
A closure is a function that remembers the variables from its outer scope even after the outer function has finished execution.

#### 👉 Example:

```javascript
function outer() {
  let count = 0;  // Variable in outer scope
  return function inner() {
    count++; 
    console.log(count); 
  };
}

const increment = outer();
increment(); // 1
increment(); // 2
``` 
📌 Use Cases of Closures:
- ✔️ Data encapsulation (e.g., private variables).
- ✔️ Memoization and caching.
- ✔️ Event handlers & callback functions.

#### 1.5 What is the difference between var, let, and const?

| Feature       | `var`               | `let`               | `const`             |
|--------------|--------------------|--------------------|--------------------|
| **Scope**    | Function-scoped     | Block-scoped       | Block-scoped       |
| **Hoisting** | Yes (initialized as `undefined`) | Yes (TDZ applies) | Yes (TDZ applies) |
| **Reassignment** | ✅ Allowed | ✅ Allowed | ❌ Not Allowed |

#### 👉 Example:

```javascript
function test() {
  if (true) {
    var x = 10;  // Accessible outside block
    let y = 20;  // Block scoped
    const z = 30; // Block scoped
  }
  console.log(x); // ✅ Works
  console.log(y); // ❌ Error
  console.log(z); // ❌ Error
}
test();
```

✅ Best Practice:

- Use const by default.
- Use let if reassignment is required.
- Avoid var due to function scoping issues.

### 1.6 Explain the JavaScript Event Loop.
JavaScript has a single-threaded model but can handle asynchronous tasks using the Event Loop.

📌 Key Concepts:

- Call Stack – Handles function execution.
- Web APIs – Handles async operations (setTimeout, DOM events, fetch).
- Callback Queue – Stores callbacks to be executed.
- Microtask Queue – Priority queue for Promises & async/await.

#### 👉 Example:
```javascript
console.log("Start");

setTimeout(() => console.log("Timeout"), 0); 

Promise.resolve().then(() => console.log("Promise"));

console.log("End");

// Output: Start → End → Promise → Timeout
```

Microtasks (Promise.then) execute before Macrotasks (setTimeout).


### 1.7 Explain the difference between deep copy and shallow copy.
- Shallow Copy: Copies only references, not nested objects.
- Deep Copy: Recursively copies all nested objects.

#### 👉 Example (Shallow Copy using Spread):

```javascript
let obj1 = { a: 1, b: { c: 2 } };
let obj2 = { ...obj1 };

obj2.b.c = 99;
console.log(obj1.b.c); // 99 (Modified)
```

#### 👉 Example (Deep Copy using JSON.parse(JSON.stringify())):

```javascript
let obj3 = JSON.parse(JSON.stringify(obj1));
obj3.b.c = 100;
console.log(obj1.b.c); // 99 (Original remains intact)
```

📌 Limitations: JSON doesn't handle functions, undefined, Symbol, or Date.

#### ✔ Using structuredClone() (ES2021+):

```javascript
let deepCopy = structuredClone(obj1);
```

### 1.8 What is ```this``` in JavaScript?
```this``` refers to the execution context (who is calling the function).

#### 👉 Rules:

- Global Scope (window in browser, global in Node.js).
- Object Method → this refers to the object.
- Arrow Functions → this is lexically bound (does not change).
- Event Handlers → this refers to the element triggering the event.

#### 👉 Example:

```javascript
const obj = {
  name: "Osama",
  greet: function() {
    console.log(this.name); // "Osama"
  }
};
obj.greet();

const arrowFunc = () => console.log(this);  
arrowFunc();  // Window/global

```

✅ Best Practice: Use arrow functions for lexical this.

### 1.9 What is call, apply, and bind?
call & apply invoke a function with a specified this value.
bind creates a new function with a fixed this.

#### 👉 Example:
```javascript
const person = { name: "Osama" };

function greet(age) {
  console.log(`Hello, ${this.name}. Age: ${age}`);
}

greet.call(person, 25);  // Call with arguments
greet.apply(person, [25]); // Apply with array
const boundGreet = greet.bind(person, 25);
boundGreet(); // Binds permanently
```

### 1.10 What is a Promise, and how does it work?
A Promise represents a future value, avoiding callback hell.

#### 📌 States of a Promise:
- ✔ Pending – Initial state.
- ✔ Fulfilled – Operation successful.
- ✔ Rejected – Operation failed.

#### 👉 Example:


```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve("Data fetched!"), 1000);
});

promise.then(data => console.log(data)).catch(err => console.log(err));
```







## 2. Advanced JavaScript Concepts
This section covers deeper JavaScript concepts like prototypes, inheritance, event delegation, and design patterns.

### 2.1 How does Prototypal Inheritance work in JavaScript?
JavaScript uses prototypal inheritance instead of classical inheritance (like Java or C++). Each object can inherit properties and methods from another object via its prototype chain.

#### 👉 Example:

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  console.log(`Hello, my name is ${this.name}`);
};

const user1 = new Person("Osama");
user1.greet();  // "Hello, my name is Osama"
console.log(user1.__proto__ === Person.prototype);  // true
```

#### 📌 Key Points:

Every function in JavaScript has a prototype object.
Objects created from a constructor inherit methods via prototype.

```__proto__``` links an object to its prototype.

### 2.2 What is the difference between ES6 Classes and Prototypes?

| Feature        | Prototypes         | ES6 Classes         |
|---------------|-------------------|---------------------|
| **Definition**  | Function-based    | Class-based syntax  |
| **Inheritance** | Prototype chain   | `extends` keyword   |
| **Constructor** | Defined manually  | Uses `constructor()` |
| **Instantiation** | `new Object()`  | `new ClassName()`   |

#### 👉 Example (ES6 Class-based Inheritance):

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks.`);
  }
}

const myDog = new Dog("Buddy");
myDog.speak();  // "Buddy barks."
```

✅ Use ES6 classes for cleaner, more structured code.

### 2.3 What is Function Currying in JavaScript?
Currying is a technique where a function returns another function until all arguments are provided.

#### 👉 Example:

```javascript
function add(a) {
  return function(b) {
    return function(c) {
      return a + b + c;
    };
  };
}

console.log(add(2)(3)(5));  // Output: 10

```
#### 📌 Use Cases:

- Function composition.
- Avoiding repetitive arguments.
- Partial application.

#### ✅ Currying with Arrow Functions:
```javascript
const multiply = a => b => c => a * b * c;
console.log(multiply(2)(3)(4));  // 24
```

### 2.4 Explain Debouncing and Throttling.
Both techniques optimize function execution for performance.

#### Difference Between Debouncing and Throttling

| Feature      | Debouncing                              | Throttling                           |
|-------------|----------------------------------------|--------------------------------------|
| **Definition** | Delays execution until inactivity    | Executes at regular intervals       |
| **Use Case**  | Search input                          | Window resize, scroll events        |


#### 👉 Debouncing Example:

```javascript
function debounce(func, delay) {
  let timer;
  return function(...args) {
    clearTimeout(timer);
    timer = setTimeout(() => func(...args), delay);
  };
}

const searchHandler = debounce(() => console.log("Searching..."), 500);
document.getElementById("search").addEventListener("input", searchHandler);
```

#### 👉 Throttling Example:

```javascript
function throttle(func, limit) {
  let lastFunc, lastRan;
  return function(...args) {
    if (!lastRan) {
      func(...args);
      lastRan = Date.now();
    } else {
      clearTimeout(lastFunc);
      lastFunc = setTimeout(() => {
        if (Date.now() - lastRan >= limit) {
          func(...args);
          lastRan = Date.now();
        }
      }, limit - (Date.now() - lastRan));
    }
  };
}

const resizeHandler = throttle(() => console.log("Resizing..."), 500);
window.addEventListener("resize", resizeHandler);
```

### 2.5 What is Event Delegation?
Event Delegation is a pattern where a single event listener handles events for multiple child elements.
#### 📌 Why use it?
- ✔ Improves performance by reducing event listeners.
- ✔ Useful when dynamically adding elements.

#### 👉 Example:
```javascript
document.getElementById("parent").addEventListener("click", function(event) {
  if (event.target.matches(".child")) {
    console.log("Child element clicked:", event.target.innerText);
  }
});
```

✅ Use `event.target.matches()` to filter events.

### 2.6 What are Higher-Order Functions?
A Higher-Order Function is a function that takes a function as an argument or returns a function.

#### 👉 Example:

```javascript
function higherOrder(fn) {
  return function(x) {
    return fn(x) * 2;
  };
}

const double = higherOrder(x => x + 10);
console.log(double(5));  // Output: 30
```

#### 📌 Use Cases:
- ✔ Functional programming.
- ✔ Callback functions (e.g., map, filter, reduce).

### 2.7 What is Memoization?
Memoization is a performance optimization technique that caches function results.

#### 👉 Example:

```javascript
function memoize(fn) {
  let cache = {};
  return function(...args) {
    let key = JSON.stringify(args);
    if (cache[key]) {
      return cache[key];
    } else {
      let result = fn(...args);
      cache[key] = result;
      return result;
    }
  };
}

const factorial = memoize(n => (n <= 1 ? 1 : n * factorial(n - 1)));
console.log(factorial(5));  // 120 (calculated)
console.log(factorial(5));  // 120 (cached)
```

✅ Use memoization in expensive calculations like Fibonacci, factorials, API calls.

### 2.8 Explain Lazy Loading in JavaScript.
Lazy loading is a technique where resources (images, scripts, components) load only when needed, improving performance.

#### 👉 Example (Lazy Loading Images):

```javascript
<img data-src="image.jpg" class="lazy-load" />

<script>
document.addEventListener("DOMContentLoaded", () => {
  const lazyImages = document.querySelectorAll(".lazy-load");

  const observer = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        let img = entry.target;
        img.src = img.getAttribute("data-src");
        observer.unobserve(img);
      }
    });
  });

  lazyImages.forEach(img => observer.observe(img));
});
</script>
```

#### ✅ Use Cases:

- Lazy loading React components (React.lazy, Suspense).
- Optimizing images, videos.

### 2.9 What are JavaScript WeakMaps and WeakSets?

**WeakMaps & WeakSets** allow garbage collection of unused references, preventing memory leaks.

| Feature            | WeakMap            | WeakSet           |
|--------------------|-------------------|-------------------|
| **Stores**        | Key-Value Pairs    | Unique values    |
| **Key Type**      | Objects only       | Objects only     |
| **Garbage Collection** | ✅ Yes      | ✅ Yes          |



#### 👉 Example (WeakMap for caching):

```javascript
let cache = new WeakMap();

function getData(obj) {
  if (!cache.has(obj)) {
    cache.set(obj, Math.random());  
  }
  return cache.get(obj);
}

let obj = {};
console.log(getData(obj));  // Cached value
obj = null;  // Garbage collected
```


## 3. Asynchronous JavaScript

JavaScript is single-threaded, meaning it executes one operation at a time. However, it uses the Event Loop to handle asynchronous tasks efficiently.

### 3.1 Explain the JavaScript Event Loop and Asynchronous Execution.

The Event Loop ensures JavaScript remains non-blocking, handling asynchronous operations like setTimeout, Promises, and I/O operations.

#### 📌 Key Components:

- Call Stack – Handles synchronous code.
- Web APIs – Handles async tasks (setTimeout, fetch, etc.).
- Callback Queue – Stores pending callbacks.
- Microtask Queue – Priority queue for Promise.then().

#### 👉 Example:

```javascript
console.log("Start");

setTimeout(() => console.log("Timeout"), 0);

Promise.resolve().then(() => console.log("Promise resolved"));

console.log("End");

// Output: Start → End → Promise resolved → Timeout
✅ Microtasks (Promises) execute before Macrotasks (setTimeout).
```

### 3.2 What is the difference between Callbacks, Promises, and Async/Await?

| Feature         | Callbacks          | Promises            | Async/Await          |
|---------------|------------------|-------------------|-------------------|
| **Syntax**    | Nested functions  | `.then().catch()` | Cleaner with `async/await` |
| **Readability** | Hard to maintain | Better            | Best              |
| **Error Handling** | Callback hell  | `.catch()`        | `try/catch`       |


#### 👉 Callback Example:

```javascript
function fetchData(callback) {
  setTimeout(() => callback("Data fetched"), 1000);
}

fetchData(data => console.log(data));
```


#### ❌ Callback Hell (Nested Callbacks):


```javascript
asyncOperation1(() => {
  asyncOperation2(() => {
    asyncOperation3(() => {
      console.log("Too much nesting!");
    });
  });
});
```

#### 👉 Promise Example (Avoids Callback Hell):

```javascript
function fetchData() {
  return new Promise(resolve => setTimeout(() => resolve("Data fetched"), 1000));
}

fetchData().then(console.log); // Data fetched
```

#### 👉 Async/Await Example (Best Readability):

```javascript
async function fetchData() {
  return "Data fetched";
}

async function main() {
  let data = await fetchData();
  console.log(data); // Data fetched
}
main();
```

✅ Use async/await for better readability and maintainability.

### 3.3 How does async/await work internally?
- async functions always return a Promise.
- await pauses execution until the Promise resolves.
- Error Handling with try/catch.

#### 👉 Example:
```javascript
async function fetchUser() {
  try {
    let response = await fetch("https://jsonplaceholder.typicode.com/users/1");
    let user = await response.json();
    console.log(user);
  } catch (error) {
    console.error("Error fetching user:", error);
  }
}
fetchUser();
```

#### 📌 Key Benefits:
- ✔️ Clean, readable syntax.
- ✔️ Error handling with try/catch.
- ✔️ No need for .then() chaining.

### 3.4 What is the difference between `Promise.all`, `Promise.allSettled`, `Promise.race`, and `Promise.any`?

#### Method	Behavior
```javascript
const p1 = Promise.resolve("User data");
const p2 = Promise.resolve("Posts");
const p3 = Promise.reject("Error fetching comments");
```

- `Promise.all([])`	Resolves when all promises succeed (fails if any reject).

`👉 Example (Promise.all):`

```javascript
Promise.all([p1, p2, p3])
  .then(console.log)
  .catch(console.error); // ❌ Error: One failed
```

- `Promise.allSettled([])`	Waits for all promises to finish (even if some fail).

`👉 Example (Promise.allSettled):`


```javascript
Promise.allSettled([p1, p2, p3]).then(console.log);
```


`✅ Returns:`
```
[
  {status: "fulfilled", value: "User data"},
  {status: "fulfilled", value: "Posts"},
  {status: "rejected", reason: "Error fetching comments"}
]

```


- `Promise.race([])`	Resolves/rejects when the first promise completes.
`👉 Example (Promise.race):`

```javascript
const fast = new Promise(resolve => setTimeout(() => resolve("Fastest"), 500));
const slow = new Promise(resolve => setTimeout(() => resolve("Slowest"), 1000));

Promise.race([fast, slow]).then(console.log); // ✅ "Fastest" (resolves first)
```

- `Promise.any([])`	Resolves when the first fulfilled promise succeeds (ignores rejections).

`👉 Example (Promise.any):`

```javascript
const p4 = Promise.reject("Error 1");
const p5 = Promise.reject("Error 2");
const p6 = Promise.resolve("Success!");

Promise.any([p4, p5, p6]).then(console.log); // ✅ "Success!"
```

#### ✅ Use Cases:

- Promise.all → Fetch multiple critical data (User, Profile, Posts).
- Promise.allSettled → Fetch multiple APIs but handle errors individually.
- Promise.race → Timeout a slow API request.
- Promise.any → Get the first successful API response.

### 3.5 How can you implement a custom retry mechanism for failed Promises?
A retry mechanism retries an API call N times before failing.

#### 👉 Example (Retry 3 times with delay):

```javascript
async function retryFetch(url, attempts = 3, delay = 1000) {
  for (let i = 0; i < attempts; i++) {
    try {
      let response = await fetch(url);
      if (!response.ok) throw new Error("Fetch failed");
      return await response.json();
    } catch (error) {
      console.log(`Retrying... (${i + 1})`);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
  throw new Error("Max retries reached");
}

retryFetch("https://jsonplaceholder.typicode.com/users/1")
  .then(console.log)
  .catch(console.error);
```


✅ Use this for unreliable APIs.

### 3.6 What is the difference between Microtasks and Macrotasks?

- 📌 Macrotasks (Task Queue):

setTimeout, setInterval, setImmediate (Node.js).
I/O operations.

- 📌 Microtasks (Priority Execution Queue):
Promise.then(), MutationObserver.

#### 👉 Example:
```javascript
console.log("Start");

setTimeout(() => console.log("setTimeout"), 0);
Promise.resolve().then(() => console.log("Promise"));

console.log("End");

// Output: Start → End → Promise → setTimeout
```

✅ Microtasks (Promise.then) run before Macrotasks (setTimeout).

### 3.7 How do you cancel a pending Promise?
JavaScript doesn’t provide built-in Promise cancellation, but we can use AbortController.

👉 Example (Fetch API with Cancellation):
```javascript
const controller = new AbortController();
const { signal } = controller;

fetch("https://jsonplaceholder.typicode.com/users/1", { signal })
  .then(response => response.json())
  .then(console.log)
  .catch(err => console.log("Request aborted:", err));

// Cancel request after 100ms
setTimeout(() => controller.abort(), 100);
```
✅ Use AbortController for handling user-initiated cancellations.



