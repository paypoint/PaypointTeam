---
sidebar_position: 10
title: 10. Advanced React Techniques
toc_min_heading_level: 2
toc_max_heading_level: 3
---

# 10. Advanced React Techniques

---

## Portals

In React, **Portals** allow you to render a component outside the normal DOM hierarchy of its parent component.

You can visually move a component somewhere else in the DOM, even though it is logically inside another component.

---

### HTML (index.html)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>React Portal</title>
  </head>

  <body>
    <div id="root"></div>

    <div id="portal-root"></div>
  </body>
</html>
```

---

### React

```jsx
import React from "react";
import ReactDOM from "react-dom";

function Modal() {
  return ReactDOM.createPortal(
    <div
      style={{
        background: "black",
        color: "white",
        padding: "20px",
      }}
    >
      <h2>This is Portal Modal</h2>
    </div>,
    document.getElementById("portal-root")
  );
}

function App() {
  return (
    <div>
      <h1>Main App</h1>

      <Modal />
    </div>
  );
}

export default App;
```

---

## Error Boundary

In React, an **Error Boundary** is a component that catches JavaScript errors in its child components and prevents the whole app from crashing.

Instead of breaking the entire UI, it shows a fallback UI.

---

### Example

```jsx
import React from "react";

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      hasError: false,
    };
  }

  static getDerivedStateFromError(error) {
    return {
      hasError: true,
    };
  }

  componentDidCatch(error, info) {
    console.log(error, info);
  }

  render() {
    if (this.state.hasError) {
      return <h2>Something went wrong.</h2>;
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

---

## Uncontrolled Form

In React, an **Uncontrolled Form** is a form where the input values are handled by the DOM itself, not by React state.

Instead of using `useState`, we use `refs` to directly access values.

---

### Example

```jsx
import React, { useRef } from "react";

function Form() {
  const nameRef = useRef();

  const handleSubmit = (e) => {
    e.preventDefault();

    alert(nameRef.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        ref={nameRef}
        placeholder="Enter Name"
      />

      <button type="submit">
        Submit
      </button>
    </form>
  );
}

export default Form;
```

---

## TanStack Router

TanStack Router is a modern routing library for React applications that helps manage navigation between pages in a more flexible and scalable way.

---

### Example

```jsx
import {
  RootRoute,
  Route,
  Router,
} from "@tanstack/react-router";

import Home from "./Home";
import About from "./About";

const rootRoute = new RootRoute();

const homeRoute = new Route({
  getParentRoute: () => rootRoute,
  path: "/",
  component: Home,
});

const aboutRoute = new Route({
  getParentRoute: () => rootRoute,
  path: "/about",
  component: About,
});

const routeTree = rootRoute.addChildren([
  homeRoute,
  aboutRoute,
]);

const router = new Router({
  routeTree,
});

export default router;
```

---

## TanStack Query

TanStack Query is a library used for data fetching, caching, and synchronization in React applications.

---

### Example

```jsx
import React from "react";

import {
  QueryClient,
  QueryClientProvider,
  useQuery,
} from "@tanstack/react-query";

const queryClient = new QueryClient();

function Users() {
  const { data, isLoading } = useQuery({
    queryKey: ["users"],
    queryFn: async () => {
      const res = await fetch(
        "https://jsonplaceholder.typicode.com/users"
      );

      return res.json();
    },
  });

  if (isLoading) {
    return <h2>Loading...</h2>;
  }

  return (
    <div>
      {data.map((user) => (
        <p key={user.id}>
          {user.name}
        </p>
      ))}
    </div>
  );
}

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Users />
    </QueryClientProvider>
  );
}

export default App;
```

---

## Lazy Loading and Skeleton UI

Lazy Loading and Skeleton UI are modern React techniques used to improve application performance and user experience.

---

## Lazy Loading

Lazy Loading allows components to load only when they are needed instead of loading everything at once.

This improves:

- Performance
- Initial loading speed
- Bundle optimization

Example: Loading Dashboard only after login.

---

### Example

```jsx
import React, {
  Suspense,
  lazy,
} from "react";

const Dashboard = lazy(() =>
  import("./Dashboard")
);

function App() {
  return (
    <div>
      <h1>React Application</h1>

      <Suspense
        fallback={
          <h2>Loading...</h2>
        }
      >
        <Dashboard />
      </Suspense>
    </div>
  );
}

export default App;
```

---

## Skeleton UI

Skeleton UI displays placeholder content while actual data is loading.

Instead of showing a spinner, it shows a fake layout similar to the real content.

---

### Example

```jsx
import React from "react";

import Skeleton from "@mui/material/Skeleton";

import Stack from "@mui/material/Stack";

function UserLoader() {
  return (
    <Stack spacing={1}>
      <Skeleton
        variant="text"
        width={200}
        height={40}
      />

      <Skeleton
        variant="rectangular"
        width={300}
        height={120}
      />

      <Skeleton
        variant="circular"
        width={50}
        height={50}
      />
    </Stack>
  );
}

export default UserLoader;
```

---

## Example with API Loading

```jsx
import React, {
  useEffect,
  useState,
} from "react";

import Skeleton from "@mui/material/Skeleton";

function Users() {
  const [users, setUsers] =
    useState([]);

  const [loading, setLoading] =
    useState(true);

  useEffect(() => {
    fetch(
      "https://jsonplaceholder.typicode.com/users"
    )
      .then((res) => res.json())
      .then((data) => {
        setUsers(data);
        setLoading(false);
      });
  }, []);

  if (loading) {
    return (
      <div>
        <Skeleton
          variant="text"
          width={200}
          height={40}
        />

        <Skeleton
          variant="rectangular"
          width={300}
          height={100}
        />
      </div>
    );
  }

  return (
    <div>
      {users.map((user) => (
        <h3 key={user.id}>
          {user.name}
        </h3>
      ))}
    </div>
  );
}

export default Users;
```
