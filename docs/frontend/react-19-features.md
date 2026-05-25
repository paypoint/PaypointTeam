---
sidebar_position: 11
title: 11. React 19
toc_min_heading_level: 2
toc_max_heading_level: 3
---

# 11. React 19

## What is React 19?

React 19 is the latest version of React that focuses on:

- Simplifying code
- Improving performance
- Handling async operations automatically

It reduces the need for manual logic like:

- `useEffect` for data fetching
- Extra state for loading and errors

---

## Features of React 19

---

## Form Actions

Form Actions are functions directly connected to form submission.

They automatically handle:

- Loading (pending state)
- Errors
- Updates

Earlier, developers manually handled all this logic.

---

### Example (Old Way)

```jsx
import React, { useState } from "react";

function LoginForm() {
  const [loading, setLoading] = useState(false);

  async function handleSubmit(e) {
    e.preventDefault();

    setLoading(true);

    await fetch("/login", {
      method: "POST",
    });

    setLoading(false);
  }

  return (
    <form onSubmit={handleSubmit}>
      <button disabled={loading}>
        {loading ? "Loading..." : "Login"}
      </button>
    </form>
  );
}

export default LoginForm;
```

---

### React 19 Way

```jsx
async function loginAction(formData) {
  "use server";

  console.log(formData.get("username"));
}

function App() {
  return (
    <form action={loginAction}>
      <input
        type="text"
        name="username"
        placeholder="Enter Username"
      />

      <button type="submit">
        Submit
      </button>
    </form>
  );
}

export default App;
```

---

## use() and Suspense

---

## What is use()?

`use()` is a new hook that allows you to directly read async data (Promises).

Instead of using:

- `useEffect`
- Loading state
- Extra async logic

React handles async rendering automatically.

---

### Example

```jsx
import { use } from "react";

function fetchUsers() {
  return fetch(
    "https://jsonplaceholder.typicode.com/users"
  ).then((res) => res.json());
}

const usersPromise = fetchUsers();

function Users() {
  const users = use(usersPromise);

  return (
    <div>
      {users.map((user) => (
        <p key={user.id}>
          {user.name}
        </p>
      ))}
    </div>
  );
}

export default Users;
```

---

## Suspense

Suspense provides a way to manage loading states.

It suspends component rendering until data is ready, with the ability to show a fallback UI during loading.

---

### Example

```jsx
import React, { Suspense, use } from "react";

function fetchData() {
  return fetch(
    "https://jsonplaceholder.typicode.com/posts"
  ).then((res) => res.json());
}

const postsPromise = fetchData();

function Posts() {
  const posts = use(postsPromise);

  return (
    <div>
      {posts.map((post) => (
        <p key={post.id}>
          {post.title}
        </p>
      ))}
    </div>
  );
}

function App() {
  return (
    <Suspense fallback={<h2>Loading...</h2>}>
      <Posts />
    </Suspense>
  );
}

export default App;
```

---

## React Compiler

React Compiler automatically optimizes rendering, reducing unnecessary re-renders and improving performance.

It replaces:

- `useMemo`
- `useCallback`
- Manual optimization

Before, developers had to manually optimize components.

Now, React Compiler automatically:

- Detects unnecessary re-renders
- Memoizes values
- Optimizes components

---

### Example

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  console.log("Component Re-rendered");

  return (
    <div>
      <h2>{count}</h2>

      <button
        onClick={() =>
          setCount(count + 1)
        }
      >
        Increment
      </button>
    </div>
  );
}

export default Counter;
```

React Compiler automatically optimizes rendering and re-renders components only when necessary, such as when `count` changes.