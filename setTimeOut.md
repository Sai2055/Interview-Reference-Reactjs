# ⏳ Understanding `setTimeout()` in JavaScript

**`setTimeout()`** is a fundamental function in JavaScript used to execute a specific function or block of code **only once**, after a specified time delay.

It is one of the core mechanisms for handling **asynchronous operations**, meaning it schedules the task to run outside of the normal, linear execution flow (the **Call Stack**), allowing the rest of your code to continue running immediately.

---

## 🔑 Key Characteristics

* **Execution:** Runs the provided function **only once**.
* **Asynchronous:** It does not block the main thread. The timer is set by the browser's **Web API**, and the function's callback is placed into the **Task Queue** only when the timer expires.
* **Non-Guaranteed Time:** The delay you set is the **minimum** time the system will wait. The function may execute later if the Call Stack is busy running other synchronous code.

---

## 📝 Syntax and Structure

The `setTimeout()` function takes a minimum of two arguments: the function to execute and the delay in milliseconds.

### Syntax

```javascript
| Parameter        | Type     | Description                                                  |
|------------------|----------|--------------------------------------------------------------|
| func             | function | The callback function to be executed.                        |
| delay            | number   | The time, in milliseconds (ms), to wait before execution.    |
| arg1, arg2, ...  | optional | Arguments that are passed directly to `func` when it executes. |

## Example — Basic setTimeout Usage

```js
console.log("A: Start of script"); // Runs immediately

let timerID = setTimeout((message) => {
    console.log(`C: Delayed message: ${message}`);
}, 3000, "Hello from the future!"); // Schedules function for 3 seconds later

console.log("B: End of script"); // Runs immediately after scheduling the timer

// Output Order: A, B, C (after 3 seconds)
