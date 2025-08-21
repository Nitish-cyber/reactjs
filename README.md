# React Unit III Documentation

This documentation covers **Props, Styling, Events, State, Lifecycle, and Hooks** in React with theory and examples. All examples are kept simple and well-commented for easy understanding.

---

## 1. Props in React

**Definition:** Props (short for *properties*) are used to pass data from a parent component to a child component.

### Example:

```jsx
// Parent Component
function Parent() {
  return <Child name="React Student" />;
}

// Child Component
function Child(props) {
  return <h2>Hello, {props.name}!</h2>;
}
```

➡️ Output: **Hello, React Student!**

---

## 2. CSS in React

React allows different ways to apply styles:

### (a) Inline Styling

```jsx
function InlineExample() {
  const style = { color: "blue", fontSize: "20px" };
  return <h2 style={style}>This is Inline Styling</h2>;
}
```

### (b) CSS Stylesheets

```css
/* styles.css */
.heading {
  color: red;
  font-size: 22px;
}
```

```jsx
import "./styles.css";

function CssFileExample() {
  return <h2 className="heading">This is CSS File Styling</h2>;
}
```

### (c) CSS Modules

```css
/* Button.module.css */
.btn {
  background-color: green;
  color: white;
  padding: 10px;
}
```

```jsx
import styles from "./Button.module.css";

function Button() {
  return <button className={styles.btn}>Click Me</button>;
}
```

### (d) Tailwind CSS

```jsx
function TailwindExample() {
  return <h2 className="text-purple-600 text-xl">Styled with Tailwind</h2>;
}
```

---

## 3. Events in React

Events are handled using camelCase (e.g., `onClick`).

### Example:

```jsx
function EventExample() {
  function handleClick() {
    alert("Button Clicked!");
  }
  return <button onClick={handleClick}>Click Me</button>;
}
```

---

## 4. State in React

State is data that changes over time inside a component.

### Example using `useState`:

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}
```

---

## 5. Stateless vs Stateful Components

* **Stateless:** Only uses props, no state.
* **Stateful:** Manages its own state.

```jsx
function Stateless(props) {
  return <h2>Hello {props.name}</h2>;
}

function Stateful() {
  const [msg] = useState("I have state");
  return <h2>{msg}</h2>;
}
```

---

## 6. Component Lifecycle

React lifecycle has **Mounting, Updating, and Unmounting** phases.

### Diagram (Simplified):

```
Mounting -> Updating -> Unmounting

useEffect(() => {   // Mounting + Updating
  console.log("Component Mounted/Updated");
  return () => console.log("Component Unmounted");
}, []);
```

### Example:

```jsx
import { useState, useEffect } from "react";

function LifecycleDemo() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Mounted/Updated");
    return () => console.log("Unmounted");
  }, [count]);

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}
```

---

## 7. Understanding Hooks

Hooks let us use state and lifecycle in functional components.

### (a) useState

Manages state.

```jsx
const [value, setValue] = useState("Hello");
```

### (b) useEffect

Runs side effects.

```jsx
useEffect(() => {
  console.log("Effect Runs");
}, []);
```

### (c) useContext

Shares data across components.

```jsx
const ThemeContext = React.createContext();

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Child />
    </ThemeContext.Provider>
  );
}

function Child() {
  const theme = React.useContext(ThemeContext);
  return <h2>Theme is {theme}</h2>;
}
```

### (d) useRef

Accesses DOM or keeps mutable values.

```jsx
import { useRef } from "react";

function RefExample() {
  const inputRef = useRef(null);

  function focusInput() {
    inputRef.current.focus();
  }

  return (
    <div>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

### (e) useReducer

Manages complex state logic.

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "increment": return { count: state.count + 1 };
    default: return state;
  }
}

function ReducerExample() {
  const [state, dispatch] = React.useReducer(reducer, { count: 0 });

  return (
    <div>
      <h2>Count: {state.count}</h2>
      <button onClick={() => dispatch({ type: "increment" })}>Increase</button>
    </div>
  );
}
```

### (f) useCallback

Memoizes functions.

```jsx
const handleClick = React.useCallback(() => {
  console.log("Clicked");
}, []);
```

### (g) useMemo

Memoizes values.

```jsx
const result = React.useMemo(() => expensiveCalculation(num), [num]);
```

### (h) Custom Hook

```jsx
function useCounter() {
  const [count, setCount] = React.useState(0);
  const increment = () => setCount(count + 1);
  return { count, increment };
}

function CounterApp() {
  const { count, increment } = useCounter();
  return (
    <div>
      <h2>{count}</h2>
      <button onClick={increment}>+</button>
    </div>
  );
}
```

---

## 8. Mini App Example: Todo List

This app uses **props, state, events, and hooks** together.

```jsx
import { useState } from "react";

function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState("");

  const addTodo = () => {
    if (input.trim() === "") return;
    setTodos([...todos, input]);
    setInput("");
  };

  return (
    <div>
      <h2>Todo List</h2>
      <input value={input} onChange={(e) => setInput(e.target.value)} />
      <button onClick={addTodo}>Add</button>
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>{todo}</li>
        ))}
      </ul>
    </div>
  );
}
```

➡️ Features: Uses `useState`, event handling, props (if split into child components), and rendering lists.

---

# ✅ Summary

* **Props**: Passing data between components.
* **Styling**: Inline, CSS, Modules, Tailwind.
* **Events**: Handled via camelCase.
* **State**: Managed using `useState`.
* **Lifecycle**: Controlled via `useEffect`.
* **Hooks**: Powerful tools (`useState`, `useEffect`, `useContext`, etc.).
* **Mini App**: Demonstrated practical usage.

This documentation provides both **theory + examples** for teaching Unit III React effectively.
