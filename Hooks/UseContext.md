
# React `useContext` Mastery Guide

## I. The Theory: Why Do We Need `useContext`?

The Context API solves the problem of prop drilling. Prop drilling is passing data through multiple layers of components that do not need it, just to reach a deeply nested component.

Context provides a mechanism to share values (for example, user auth or theme) without explicitly passing props down every level.  
Ideal use case: global data that changes infrequently (low frequency).

---

## II. Implementation: Step-by-Step Guide

### Step 1: Create the Context

```
import { createContext } from 'react';

// Default value (null) is used if a component consumes context
// without being inside a matching Provider.
export const UserContext = createContext(null);
```
text

### Step 2: Provide the Context

Wrap the component tree that needs access to the data.
 ```
import { useState } from 'react';
import { UserContext } from './UserContext';

function App() {
const [user, setUser] = useState({ name: 'Alice' });

return (
// The 'value' prop is the data broadcasted to consumers
<UserContext.Provider value={user}>
<Dashboard />
</UserContext.Provider>
);
}
```
text

### Step 3: Consume the Context

Use the `useContext` hook in any nested component.
```

import { useContext } from 'react';
import { UserContext } from './UserContext';

function Profile() {
const user = useContext(UserContext); // Accesses the 'value' from the Provider

return (
<div>
<p>Welcome, {user.name}</p>
</div>
);
}
```
text

---

## III. Advanced Pattern: Custom Provider and Hook (Best Practice)

Encapsulate state logic and provide better error handling.
```
// ThemeContext.js
import { createContext, useContext, useState, useMemo } from 'react';

const ThemeContext = createContext();

export const ThemeProvider = ({ children }) => {
const [theme, setTheme] = useState('light');

const toggleTheme = () => {
setTheme(prev => (prev === 'light' ? 'dark' : 'light'));
};

// Performance fix: useMemo to stabilize the value object reference
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

// Custom hook for consumption
export const useTheme = () => {
const context = useContext(ThemeContext);
if (!context) {
throw new Error('useTheme must be used within a ThemeProvider');
}
return context;
};

```

---

## IV. Interview Mastery: Performance Pitfall

The major pitfall of Context is unnecessary re-renders.  
If the object passed to the `value` prop is not memoized, it creates a new reference on every parent render, forcing all consumers to re-render, even if the data inside has not changed.

**The fix:** Always use `useMemo` for the `value` prop of the provider, as shown in the advanced pattern above.

---

## V. The “State Management Trifecta”: `useState` vs `useContext` vs RTK

Focus on frequency of updates and complexity of logic, not just size.

### Feature Comparison

| Feature        | `useState`                  | `useContext`                            | Redux Toolkit (RTK)                        |
| ------------- | --------------------------- | --------------------------------------- | ------------------------------------------ |
| Scope         | Local / component           | Global / sub-tree                       | Global / centralized                       |
| Update freq   | High or low                 | Low (critical)                          | High (best optimized)                      |
| Re-renders    | Component only              | All consumers                           | Specific subscribers (via selectors)       |
| Complexity    | Simple logic                | Simple logic (sharing only)             | Complex logic (reducers, middleware)       |
| Cost          | Zero boilerplate            | Minimal boilerplate                     | Higher setup cost                          |

---

## VI. Code Examples: State Management Scenarios

### Scenario A: `useState` (Local UI State)

Data type: ephemeral booleans, strings.
```
const DeleteConfirmation = () => {
// Local state for visibility
const [isOpen, setIsOpen] = useState(false);

return (
<div>
<button onClick={() => setIsOpen(true)}>Delete Item</button>
{/* ... Modal content based on isOpen */}
</div>
);
};

// Why: The state is contained and does not affect other components.
```
text

### Scenario B: `useContext` (Global, Low Frequency)

Data type: user auth object, theme settings. Read by many, written by few.
```
// Consuming the User Auth Context
function ProfileHeader() {
// Data is needed here, but its value rarely changes (only on login/logout).
const { user } = useAuth(); // Assume useAuth uses useContext

return <h1>Hello, {user?.name}!</h1>;
}

// Why: Prop drilling is avoided, and since updates are rare,
// the re-render cost is minimal.
````
text

### Scenario C: Redux Toolkit (Global, High Frequency / Complex)

Data type: real-time data, complex entity management.

Problem: If a live stock ticker updates 10 times a second, using Context would force every component consuming the Context to re-render, causing lag.

RTK solution (granular selectors):
```
// --- The Component ---
import { useSelector } from 'react-redux';

const BitcoinTicker = () => {
// This component subscribes ONLY to the 'btc' price.
// If the 'eth' price changes, this component remains stable.
const btcPrice = useSelector(state => state.prices.btc);

return <div>BTC: ${btcPrice}</div>;
};

const EthereumTicker = () => {
// This component subscribes ONLY to the 'eth' price.
const ethPrice = useSelector(state => state.prices.eth);

return <div>ETH: ${ethPrice}</div>;
};

// Why: RTK's selectors allow components to subscribe to small, specific
// slices of the state, optimizing performance for high-frequency updates.
```
text

---

## VII. Conclusion: Choosing Your Tool

- Small, local state: **`useState`**
- Global, infrequent state: **`useContext`**
- Global, frequent or complex state: **Redux Toolkit** (for performance and structured logic)
