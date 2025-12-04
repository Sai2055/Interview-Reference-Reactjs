# 🌟 React `useContext` — Mastery Guide

---

## 🧠 I. The Theory: Why Do We Need `useContext`?

The Context API solves the problem of **prop drilling** — passing props through multiple layers of components that do not need them, just to reach a deeply nested one.

Context provides a mechanism to **share values globally** (like user auth or theme) without manually passing props.

> 📌 **Ideal Use Case:** Global data that changes *infrequently*.

---

## 🛠️ II. Implementation: Step-by-Step Guide

---

### 🔹 **Step 1: Create the Context**

```js

import { createContext } from 'react';

// Default value (null) is used if a component consumes context
// without being inside a matching Provider.
export const UserContext = createContext(null);
```
### Step 2: Provide the Context

Wrap the component tree that needs access to the shared data.
```
import { useState } from 'react';
import { UserContext } from './UserContext';

function App() {
  const [user, setUser] = useState({ name: 'Alice' });

  return (
    // 'value' is the data broadcasted to all nested consumers
    <UserContext.Provider value={user}>
      <Dashboard />
    </UserContext.Provider>
  );
}
````
### 🔹 Step 3: Consume the Context
```
import { useContext } from 'react';
import { UserContext } from './UserContext';

function Profile() {
  const user = useContext(UserContext); // Access value from Provider

  return (
    <div>
      <p>Welcome, {user.name}</p>
    </div>
  );
}
```
### 🚀 III. Advanced Pattern: Custom Provider + Custom Hook (Best Practice)

Encapsulate state logic and prevent misuse.
```
// ThemeContext.js
import { createContext, useContext, useState, useMemo } from 'react';

const ThemeContext = createContext();

export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () =>
    setTheme(prev => (prev === 'light' ? 'dark' : 'light'));

  // useMemo prevents re-renders caused by new object references
  const contextValue = useMemo(
    () => ({ theme, toggleTheme }),
    [theme]
  );

  return (
    <ThemeContext.Provider value={contextValue}>
      {children}
    </ThemeContext.Provider>
  );
};

// Custom hook for consuming context
export const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within a ThemeProvider');
  }
  return context;
};
```
### ⚠️ IV. Interview Mastery: The Performance Pitfall

⚠️ Major Pitfall:
If the object passed to the value prop is not memoized,
every consumer will re-render on every parent render.

✔ Always wrap Provider values inside useMemo
```

const value = useMemo(() => ({ theme, toggleTheme }), [theme]);
```

### 🎛️ V. The “State Management Trifecta”

useState vs useContext vs Redux Toolkit (RTK)
Feature	useState	useContext	Redux Toolkit (RTK)
Scope	Local / component	Global / subtree	Global / centralized
Update freq	High or low	Low (important)	High / optimized
Re-renders	Only this component	All consumers	Only subscribed components
Complexity	Very simple	Simple (sharing only)	Complex (reducers, middleware)
Boilerplate	None	Minimal	Higher


### 🎯 VI. Code Scenarios — When to Use What?

### 🔹 Scenario A: useState (Local UI State)
```
const DeleteConfirmation = () => {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setIsOpen(true)}>Delete Item</button>
      {/* Modal content based on isOpen */}
    </div>
  );
};
```

🟢 Use When: State is local, does not affect siblings or global UI.
```
🔹 Scenario B: useContext (Global, Low Frequency)
function ProfileHeader() {
  const { user } = useAuth(); // useAuth -> useContext

  return <h1>Hello, {user?.name}!</h1>;
}

```
🟦 Use When:

Many components read the same data

The data changes rarely (login/logout, theme toggle)

🔹 Scenario C: Redux Toolkit (High Frequency / Complex)
```
import { useSelector } from 'react-redux';

const BitcoinTicker = () => {
  const btcPrice = useSelector(state => state.prices.btc);
  return <div>BTC: ${btcPrice}</div>;
};

const EthereumTicker = () => {
  const ethPrice = useSelector(state => state.prices.eth);
  return <div>ETH: ${ethPrice}</div>;
};
```

🟥 Use When:
High-frequency updates (e.g., stock tickers, chat messages),
where Context causes unnecessary re-renders.

### 🧾 VII. Conclusion: Choosing the Right Tool
✔ Small, local state → useState
✔ Global, infrequent updates → useContext
✔ Global + frequent + complex state → Redux Toolkit (RTK)
