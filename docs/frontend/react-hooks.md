# 9. React Hooks

Hooks allow us to manage state and lifecycle inside functions, removing the need for class components.

---

# useState

`useState` is used to store and update data inside a component.

## Example

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>Count: {count}</h2>

      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}

export default Counter;
```

---

# useEffect

`useEffect` allows us to handle external operations like API calls, timers, and side effects without affecting the component structure.

## Example

```jsx
import React, { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((data) => setUsers(data));
  }, []);

  return (
    <div>
      <h2>User List</h2>

      {users.map((user) => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}

export default Users;
```

---

# useContext

`useContext` is used to share data globally without prop drilling.

---

## Step 1: Create Context

```jsx
import { createContext } from "react";

export const UserContext = createContext();
```

---

## Step 2: Provide Data

```jsx
import React from "react";
import { UserContext } from "./UserContext";
import Home from "./Home";

function App() {
  return (
    <UserContext.Provider value={"Shreya"}>
      <Home />
    </UserContext.Provider>
  );
}

export default App;
```

---

## Step 3: Consume Data

```jsx
import React, { useContext } from "react";
import { UserContext } from "./UserContext";

function Home() {
  const user = useContext(UserContext);

  return <h2>Welcome {user}</h2>;
}

export default Home;
```

---

# Custom Hooks

One thing that's special about hooks is their composability. We can use hooks to create other hooks called **Custom Hooks**.

Custom hooks are user-defined functions that allow us to reuse logic across multiple components.

## Rules of Custom Hooks

- Their name must start with `use`
- They can use built-in hooks like `useState`, `useEffect`, etc.

---

# Create Custom Hook

```jsx
import { useState } from "react";

function useCounter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  return { count, increment };
}

export default useCounter;
```

---

# Use Custom Hook

```jsx
import React from "react";
import useCounter from "./useCounter";

function Counter() {
  const { count, increment } = useCounter();

  return (
    <div>
      <h2>Count: {count}</h2>

      <button onClick={increment}>
        Increment
      </button>
    </div>
  );
}

export default Counter;
```