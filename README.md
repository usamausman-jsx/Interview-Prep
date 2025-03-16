# 🚀 Frontend & System Design Interview Guide

This guide covers essential topics for **Frontend, JavaScript, React, System Design, Performance Optimization, and Security**. It includes key concepts, best practices, and coding challenges to help you ace your technical interviews.  

---

## 📌 **Table of Contents**

1. [JavaScript Fundamentals](#javascript-fundamentals)
2. [Advanced JavaScript](#advanced-javascript)
3. [Asynchronous JavaScript](#asynchronous-javascript)
4. [React Basics](#react-basics)
5. [React Hooks](#react-hooks)
6. [State Management](#state-management)
7. [Performance Optimization](#performance-optimization)
8. [React Router & Navigation](#react-router--navigation)
9. [Testing & Debugging](#testing--debugging)
10. [Advanced React](#advanced-react)
11. [System Design](#system-design)
12. [Security & Best Practices](#security--best-practices)
13. [CI/CD & DevOps](#cicd--devops)
14. [Miscellaneous Topics](#miscellaneous-topics)
15. [Practice Questions](#practice-questions)
16. [Resources for Further Learning](#resources-for-further-learning)

---

## 1️⃣ JavaScript Fundamentals

### **What is Hoisting?**  
Hoisting is JavaScript's behavior of moving variable and function **declarations** to the top of their scope before execution.

```js
console.log(a); // undefined (hoisted)
var a = 10;

foo(); // Works due to function hoisting
function foo() { console.log("Hoisted function!"); }
```

✅ **Variables declared with `let` and `const` are hoisted but remain in the Temporal Dead Zone.**

---

### **Closures in JavaScript**  
Closures allow functions to remember variables from their **outer scope**, even after execution.

```js
function outer() {
  let count = 0;
  return function inner() {
    count++;
    console.log(count);
  };
}

const increment = outer();
increment(); // 1
increment(); // 2
```

✅ **Used for encapsulation, caching, and event handlers.**

---

## 2️⃣ Advanced JavaScript

### **Difference between `==` and `===`**
- `==` → Performs type coercion (loose comparison).  
- `===` → Strict comparison (checks both value and type).  

```js
console.log(5 == "5");  // true (coercion)
console.log(5 === "5"); // false (different types)
```

---

## 3️⃣ Asynchronous JavaScript

### **Event Loop & Microtasks**
The **Event Loop** allows JavaScript to handle async operations.  

```js
console.log("Start");

setTimeout(() => console.log("Timeout"), 0);
Promise.resolve().then(() => console.log("Promise"));

console.log("End");

// Output: Start → End → Promise → Timeout
```

✅ **Microtasks (`Promise.then()`) run before macrotasks (`setTimeout`).**

---

## 4️⃣ React Basics

### **React Virtual DOM & Reconciliation**
React uses the **Virtual DOM** to efficiently update the UI.

```jsx
function Counter() {
  const [count, setCount] = React.useState(0);

  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}
```

✅ **Only the changed part of the UI is updated.**

---

## 5️⃣ React Hooks

### **Using `useEffect()` for Side Effects**
```jsx
useEffect(() => {
  fetch("https://api.example.com/data")
    .then(res => res.json())
    .then(setData);
}, []);  // Runs only on mount
```

✅ **Use `useEffect()` to fetch data or handle subscriptions.**

---

## 6️⃣ State Management

### **Context API vs. Redux vs. Zustand**

| State Type  | Best Approach |
|------------|--------------|
| Local State | `useState()` |
| Global State | Context API, Redux |
| Async Data | React Query, SWR |

---

## 7️⃣ Performance Optimization

### **React.memo() for Preventing Unnecessary Re-renders**  
```jsx
const MemoizedComponent = React.memo(({ count }) => {
  return <div>{count}</div>;
});
```

✅ **Only re-renders when `count` changes.**

---

## 8️⃣ Security & Best Practices

### **Preventing XSS (Cross-Site Scripting)**
```js
document.getElementById("output").textContent = userInput; // ✅ Safe
```

✅ **Always escape user input before rendering.**

---

## 9️⃣ System Design

### **Scaling a React Application (Micro Frontends)**
```js
new ModuleFederationPlugin({
  name: "dashboard",
  filename: "remoteEntry.js",
  exposes: { "./Dashboard": "./src/Dashboard" },
});
```

✅ **Allows independent deployment of frontend modules.**

---

## 🔟 Practice Questions

### **JavaScript Challenges**
1. Implement a **polyfill for `Array.prototype.map()`**.  
2. Write a function to **deep clone an object**.  
3. Implement a **debounce function** that delays execution.  
4. Convert a function into a **curried version** (`sum(1)(2)(3) → 6`).  
5. Implement **a retry mechanism** for an API call (retry 3 times on failure).  

### **React Challenges**
1. Create a **custom hook for fetching data (`useFetch()`)**.  
2. Implement a **React Error Boundary** to catch runtime errors.  
3. Optimize a React component to **prevent unnecessary re-renders**.  
4. Use **React Suspense** to **lazy load a component**.  
5. Implement **a search bar with debounced API calls**.  

---

## 📚 Resources for Further Learning  
- **JavaScript:** [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript)  
- **React:** [React Official Docs](https://reactjs.org/docs/getting-started.html)  
- **System Design:** [System Design Primer](https://github.com/donnemartin/system-design-primer)  
- **Security Best Practices:** [OWASP Guidelines](https://owasp.org/)  

---

🚀 **Happy Coding & Best of Luck for Your Interviews!** 😃  
