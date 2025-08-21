1. Installing React
a) Using Create React App (CRA)
npx create-react-app my-app
cd my-app
npm start

b) Using Vite (Recommended – faster)
npm create vite@latest my-app
cd my-app
npm install
npm run dev

2. React Environment Setup

Node.js & npm → Required for running React.

Code Editor → VS Code (recommended).

Browser → Chrome/Edge.

Package Manager → npm or yarn.

3. React Folder Structure (Default Vite)
my-app/
 ├── node_modules/   → Installed dependencies
 ├── public/         → Static files (favicon, index.html)
 ├── src/            → Application code
 │    ├── main.jsx   → Entry file
 │    ├── pp.jsx     → Root component
 │    └── components/→ Custom components
 ├── package.json    → Project config
 └── vite.config.js  → Vite config

4. JSX (JavaScript XML)

JSX allows us to write HTML-like syntax in JavaScript.

Example:

const element = <h1>Hello, React!</h1>;

5. Rules of JSX

Must return only one parent element.

// Wrong ❌
return <h1>Hello</h1> <p>World</p>;  

// Correct ✅
return (
  <div>
    <h1>Hello</h1>
    <p>World</p>
  </div>
);


Attributes use camelCase.

<div className="container"></div>  // ✅
<button onClick={handleClick}></button>


Close all tags.

<img src="logo.png" /> ✅


JavaScript expressions inside { }.

const name = "React";
const element = <h2>Hello, {name}</h2>;

6. Understanding Component Basics

Components are reusable UI building blocks.

Types:

Functional Components (modern, preferred)

Class Components (older, still valid)

7. React.createElement() Arguments

Syntax:

React.createElement(type, props, ...children)


Example:

const element = React.createElement(
  "h1",
  { className: "title" },
  "Hello React"
);

8. JSX vs React.createElement()
// JSX
const element = <h1>Hello JSX</h1>;

// React.createElement()
const element = React.createElement("h1", null, "Hello JSX");

9. Rendering Elements into the DOM
import React from "react";
import ReactDOM from "react-dom/client";
import pp from "./pp";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<pp />);

🎨 Unit II – Components & Styles
1. Creating Components
Functional Component (Modern)
function Welcome() {
  return <h2>Welcome to React!</h2>;
}
export default Welcome;

Class Component (Old Style)
import React, { Component } from "react";

class Greeting extends Component {
  render() {
    return <h2>Hello from Class Component</h2>;
  }
}
export default Greeting;

2. Styling in React

# React

This documentation covers **Props, Styling, Events, State, Lifecycle, and Hooks** in React with theory and examples. All examples are kept simple and well-commented for easy understanding.

---

## 0. Setup & Installation

### Install React (using Vite)

```bash
# Create React project
npm create vite@latest my-app
cd my-app
npm install
npm run dev
```

### Install Tailwind CSS in React

```bash
# Step 1: Install Tailwind
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Step 2: Add paths in tailwind.config.js
module.exports = {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: { extend: {} },
  plugins: [],
}

# Step 3: Add Tailwind directives to src/index.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

➡️ Now Tailwind is ready to use in your project.

---

## 1. Props in React

**Definition:** Props (short for *properties*) are used to pass data from a parent component to a child component.
🔹 Syntax of Using Props

**2.Pass props from Parent → Child
**
<ChildComponent name="React Student" age={21} />
**2.Receive props inside Child component
**
function ChildComponent(props) {
  return <h2>Hello, {props.name}. You are {props.age} years old.</h2>;
}

### Basic Example:

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

**🔹 Complete Example
**
// Parent Component
function Parent() {
  return (
    <div>
      <h1>Props Example</h1>
      {/* Passing props to child */}
      <Child name="Alice" age={20} />
      <Child name="Bob" age={22} />
    </div>
  );
}

// Child Component
function Child(props) {
  return (
    <p>
      Hello, {props.name}! You are {props.age} years old.
    </p>
  );
}

export default Parent;

### Complete Example (Props with multiple children):

```jsx
function Student(props) {
  return (
    <div>
      <h3>Name: {props.name}</h3>
      <p>Age: {props.age}</p>
      <p>Course: {props.course}</p>
    </div>
  );
}

function StudentsList() {
  return (
    <div>
      <h2>Students Information</h2>
      <Student name="Alice" age={20} course="React" />
      <Student name="Bob" age={22} course="Node.js" />
      <Student name="Charlie" age={19} course="JavaScript" />
    </div>
  );
}
```

➡️ This example shows **how to pass multiple props** and render different child components with data.

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

➡️ Tailwind is already set up from installation steps.

---

## 3. Events in React

Events are handled using camelCase (e.g., `onClick`).
# React Fundamentals 📘

This document covers the basics of **React** concepts including **Event Handling, State, Stateless vs Stateful Components, and Component Lifecycle** with examples.

---

## 🔹 1. Event Handling in React

In React, events are written in **camelCase** and event handlers are passed as functions.

### ✅ Example: Button Click Event
```jsx
function ClickExample() {
  function handleClick() {
    alert("Button was clicked!");
  }

  return (
    <button onClick={handleClick}>Click Me</button>
  );
}

export default ClickExample;
**Example: Passing Parameters
**
function GreetUser() {
  function greet(name) {
    alert("Hello, " + name);
  }

  return (
    <button onClick={() => greet("Alice")}>Greet</button>
  );
}
**2. Creating State

State allows a component to store and manage data that changes over time.
**

import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0); // Initial state = 0

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(count + 1)}>Increase</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}

export default Counter;


### Basic Example:

```jsx
function EventExample() {
  function handleClick() {
    alert("Button Clicked!");
  }
  return <button onClick={handleClick}>Click Me</button>;
}
```

### Complete Example (Multiple Events):

```jsx
function FormEvents() {
  function handleChange(e) {
    console.log("Input value:", e.target.value);
  }

  function handleSubmit(e) {
    e.preventDefault();
    alert("Form Submitted!");
  }

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

---

## 4. State in React

State is data that changes over time inside a component.

### Basic Example using `useState`:

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

### Complete Example (Multiple States):

```jsx
function Profile() {
  const [name, setName] = useState("John");
  const [age, setAge] = useState(25);

  return (
    <div>
      <h2>{name} is {age} years old</h2>
      <button onClick={() => setAge(age + 1)}>Increase Age</button>
      <button onClick={() => setName("Alice")}>Change Name</button>
    </div>
  );
}
```

---

## 5. Stateless vs Stateful Components

* **Stateless:** Only uses props, no state.
* **Stateful:** Manages its own state.
3. Stateless vs Stateful Components
(a) Stateless Component

Uses props only.

No internal state.

Reusable and predictable.
### Example:

```jsx
function Stateless(props) {
  return <h2>Hello {props.name}</h2>;
}

function Stateful() {
  const [msg] = useState("I have state");
  return <h2>{msg}</h2>;
}
```

### Complete Example (Stateless + Stateful Together):

```jsx
function Message({ text }) {
  return <p>{text}</p>; // Stateless
}

function MessageContainer() {
  const [message, setMessage] = useState("Hello World");
  return (
    <div>
      <Message text={message} />
      <button onClick={() => setMessage("Updated Message")}>Change</button>
    </div>
  );
}
```

---

## 6. Component Lifecycle

React lifecycle has **Mounting, Updating, and Unmounting** phases.
4. Component Lifecycle

React components go through three main lifecycle phases:

Mounting → When the component is created and added to the DOM.

Updating → When state or props change, causing a re-render.

Unmounting → When the component is removed from the DOM.

**✅ Class Component Example
**
import React from "react";

class Example extends React.Component {
  componentDidMount() {
    console.log("Component Mounted!");
  }

  componentDidUpdate() {
    console.log("Component Updated!");
  }

  componentWillUnmount() {
    console.log("Component Unmounted!");
  }

  render() {
    return <h2>Hello, React Lifecycle!</h2>;
  }
}

export default Example;


**✅ Functional Component Example (using useEffect)
**In modern React, lifecycle methods are handled with hooks.

import { useState, useEffect } from "react";

function Timer() {
  const [count, setCount] = useState(0);

  // Runs after render
  useEffect(() => {
    console.log("Component Mounted/Updated");

    return () => {
      console.log("Component Unmounted"); // cleanup
    };
  }, [count]); // runs whenever count changes

  return (
    <div>
      <h2>Timer: {count}</h2>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}

export default Timer;


📊 Lifecycle Summary
Phase	Class Component Method(s)	Functional Hook Equivalent
Mounting	componentDidMount()	useEffect(() => {...}, [])
Updating	componentDidUpdate()	useEffect(() => {...}, [deps])
Unmounting	componentWillUnmount()	return () => {...} inside useEffect


### Diagram (Simplified):

```
Mounting -> Updating -> Unmounting

useEffect(() => {   // Mounting + Updating
  console.log("Component Mounted/Updated");
  return () => console.log("Component Unmounted");
}, []);
```

### Complete Example:

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

```jsx
const [value, setValue] = useState("Hello");
```

### (b) useEffect

```jsx
useEffect(() => {
  console.log("Effect Runs");
}, []);
```

### (c) useContext

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

```jsx
const handleClick = React.useCallback(() => {
  console.log("Clicked");
}, []);
```

### (g) useMemo

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

* **Props**: Passing data between components (basic + complete example).
* **Styling**: Inline, CSS, Modules, Tailwind (with installation).
* **Events**: Basic + multiple event example.
* **State**: Single + multiple state example.
* **Stateless vs Stateful**: Compared with examples.
* **Lifecycle**: Explained with diagram and example.
* **Hooks**: All major hooks explained with code.
* **Mini App**: Practical Todo List implementation.

This documentation provides both **theory + examples** for teaching Unit III React effectively.
