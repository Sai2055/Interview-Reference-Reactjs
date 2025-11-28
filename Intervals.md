## What is Interval in JavaScript?
# ⏰ Understanding JavaScript Intervals (`setInterval`)

An **interval** in JavaScript, often referred to as a **repeating timer**, is a mechanism that allows you to execute a specified function or code block **repeatedly** at fixed time delays.

It's managed using the **`setInterval()`** and **`clearInterval()`** functions, which are part of the **Web APIs** (in browsers) or the **Timer module** (in Node.js).

---

## 🔑 Key Functions

### 1. `setInterval()`

This function is used to **start** an interval. It calls a function repeatedly, with a fixed time delay between each call.

* **Syntax:**
    ```javascript
    let intervalID = setInterval(func, delay, arg1, arg2, ...);
    ```

* **Parameters:**
    * `func`: The function to be executed repeatedly.
    * `delay`: The time delay, in **milliseconds (ms)**, between executions of `func`.
    * `arg1, arg2, ...`: Optional arguments that are passed to `func` when it's executed.

* **Return Value:** It returns a unique **`intervalID`** (a numeric value) that is used to reference and later stop the interval.

### 2. `clearInterval()`

This function is used to **stop** the repeated execution set by `setInterval()`.

* **Syntax:**
    ```javascript
    clearInterval(intervalID);
    ```

* **Parameter:**
    * `intervalID`: The ID returned by the corresponding `setInterval()` call.

---

## 📝 Example: Stopping an Interval

This example demonstrates how to set an interval to log a message every second and then stop it after 5 seconds.

```javascript
let count = 0;

// Start the interval: call the function every 1000ms (1 second)
const myInterval = setInterval(() => {
  count++;
  console.log(`Hello! This is call number ${count}`);
  
  // Check if we should stop the interval
  if (count === 5) {
    // Stop the interval using the returned ID
    clearInterval(myInterval); 
    console.log("Interval stopped.");
  }
}, 1000); 

/* Output in the console:
Hello! This is call number 1 (after 1s)
...
Hello! This is call number 5 (after 5s)
Interval stopped.
*/
