# 🎭 What is `useEffect`?

`useEffect` is a React Hook that lets you synchronize a component with some **external system**. It is used to handle **side effects** in functional components, which are actions that happen outside of the normal component rendering process.

---

## 🔬 Key Purposes of `useEffect`

The primary use of `useEffect` is to manage operations that interact with the "outside world," such as:

* **Data Fetching:** Getting data from an API when the component mounts or when a dependency changes.
* **Manual DOM Manipulation:** Directly changing the browser's Document Object Model (e.g., setting the document title).
* **Subscriptions:** Setting up a subscription to an external data source (like a chat service) and cleaning it up when the component unmounts.
* **Timers:** Setting up `setTimeout` or `setInterval` calls.

---

## 📐 The `useEffect` Signature

The `useEffect` Hook takes two arguments:

$$
\text{useEffect}(\text{setup}, [\text{dependencies}]);
$$

1.  **`setup` (The Callback Function):** This is where you put your side effect logic.
2.  **`[dependencies]` (The Dependency Array):** This optional array tells React when to re-run the effect.

## 🎛️ Master Detail: Controlling Execution with the Dependency Array

The dependency array is the **master detail control** for your effect's execution:

| State | Dependency Array | Execution Behavior | Analogy |
| :--- | :--- | :--- | :--- |
| **0** | **Omitted Array** | The effect runs **after every single render** of the component (and initial mount). | The effect is a restless child; it runs every time anything changes. |
| **1** | **Empty Array (`[]`)** | The effect runs **only once** after the **initial mount** and its cleanup runs once upon **unmount**. | The effect is a housewarming party; it happens once when you move in and once when you move out. |
| **2** | **Array with Values (`[propA, stateB]`)** | The effect runs after the **initial mount** and then only when any of the values in the dependency array **change** between renders. | The effect is a temperature check; it runs only when the value you're watching (the temperature) changes. |

### The Cleanup Function

The `setup` function can optionally return another function, which is the **cleanup function**.

```javascript
useEffect(() => {
  // ⭐️ SETUP CODE (Runs after render)
  const subscription = source.subscribe();

  return () => {
    // 🧹 CLEANUP CODE (Runs before the next effect or on unmount)
    subscription.unsubscribe();
  };
}, [source]);
