# 🎣 What are React Hooks?

React **Hooks** are **special functions** introduced in **React 16.8** that let you use **state** and other **React features** (like lifecycle methods) in **functional components** without needing to write a class.

---

## 🛠️ Use of React Hooks

Before Hooks, function components were mainly used for simple, stateless logic. Any component that needed state or lifecycle logic had to be written as a class component. Hooks bridge this gap, offering several key advantages and uses:

* **State Management:** Hooks like `useState` allow functional components to manage local state, making them **stateful**.
* **Side Effects:** The `useEffect` Hook lets you perform "side effects" (like data fetching, subscriptions, or manual DOM manipulation) after a component renders. It consolidates the functionality of class component lifecycle methods like `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`.
* **Code Reusability:** You can extract stateful logic into **Custom Hooks** (functions starting with `use`) and reuse that logic across multiple components without complex patterns like Higher-Order Components or Render Props.
* **Simpler Code:** Hooks generally lead to shorter, clearer code by eliminating the need for class syntax, binding `this`, and having lifecycle methods spread across a component.

---

## 🔗 Core Built-in Hooks

React provides several built-in Hooks, but the most common ones you'll encounter are:

| Hook | Purpose | Analogy |
| :--- | :--- | :--- |
| **`useState`** | Adds **state** variables to a functional component. It returns the current state value and a function to update it. | Like a sticky note where your component remembers a piece of information that can change. |
| **`useEffect`** | Performs **side effects** (like data fetching, subscriptions, or manually changing the DOM) after a render. | Like a manager that handles cleanup or connects your component to the outside world after it has been built/updated. |
| **`useContext`** | Allows a component to **subscribe to React context** (a way to pass data deep down the component tree without passing props manually). | Like a direct pipeline that lets a deeply nested component tap into a global source of data. |

[All React Hooks Explained - React Hooks Tutorial 2025](https://www.youtube.com/watch?v=xfKYYRE6-TQ) is a comprehensive tutorial that explains every React Hook with real-world examples.

# ⭐ Commonly Used React Hooks

Here is a list of the most common React Hooks and their primary purposes:

| Hook | Primary Use |
| :--- | :--- |
| **1. `useState`** | Manage **local state** in functional components. |
| **2. `useEffect`** | Handle **side effects** (data fetching, subscriptions, manual DOM changes, etc.) after render. |
| **3. `useContext`** | **Consume context** values from the nearest Provider in the component tree. |
| **4. `useRef`** | Access **DOM elements** directly or store a **mutable value** that doesn't trigger re-renders. |
| **5. `useMemo`** | Optimize performance by **memoizing** (caching) the result of a **function** to avoid recalculation on every render. |
| **6. `useCallback`** | Optimize performance by **memoizing a function definition** itself, often used to prevent unnecessary re-renders in child components. |
| **7. `useReducer`** | Handle **complex state logic** where the next state depends on the previous state (alternative to `useState`). |
| **8. `useLayoutEffect`** | Similar to `useEffect`, but runs **synchronously** immediately after the DOM mutations, before the browser paints. |
