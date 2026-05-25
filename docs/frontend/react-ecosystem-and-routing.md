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

### Using Error Boundary

```jsx
import React from "react";
import ErrorBoundary from "./ErrorBoundary";
import Home from "./Home";

function App() {
  return (
    <ErrorBoundary>
      <Home />
    </ErrorBoundary>
  );
}

export default App;
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

TanStack Router is made for client-side use cases.

React Router and Remix have shifted a lot of focus to a more full-stack experience, while TanStack Router focuses on scalable and type-safe routing.

TanStack Router is a modern routing library for React applications that helps manage navigation between pages in a more flexible and scalable way.

---

### Define Routes

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

### Use Router

```jsx
import React from "react";
import ReactDOM from "react-dom/client";

import {
  RouterProvider,
} from "@tanstack/react-router";

import router from "./router";

ReactDOM.createRoot(
  document.getElementById("root")
).render(
  <RouterProvider router={router} />
);
```

---

### Navigation

```jsx
import { Link } from "@tanstack/react-router";

function Navbar() {
  return (
    <div>
      <Link to="/">
        Home
      </Link>

      <Link to="/about">
        About
      </Link>
    </div>
  );
}

export default Navbar;
```

---

## TanStack Query

TanStack Query is a library used for data fetching, caching, and synchronization in React applications.

It replaces manual API handling using `useEffect` and `fetch` with a smarter and optimized system.

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