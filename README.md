# Complete React Guide: Props, CSS, Events, State & Hooks

## Table of Contents
1. [Props in React](#props-in-react)
2. [CSS in React](#css-in-react)
3. [Events, States and Component Lifecycle](#events-states-and-component-lifecycle)
4. [Understanding Hooks](#understanding-hooks)
5. [Complete Application Example](#complete-application-example)

---

## Props in React

### Theory
Props (short for properties) are a way to pass data from parent components to child components in React. They are read-only and help make components reusable and modular.

**Key Concepts:**
- Props are immutable (cannot be changed by the receiving component)
- Data flows downward (from parent to child)
- Props can be any JavaScript data type
- Default props can be set for components

### Basic Props Example

```jsx
// Parent Component
function App() {
  const user = {
    name: "John Doe",
    age: 25,
    email: "john@example.com"
  };

  return (
    <div>
      <UserProfile 
        name={user.name} 
        age={user.age} 
        email={user.email}
        isActive={true}
      />
    </div>
  );
}

// Child Component
function UserProfile({ name, age, email, isActive }) {
  return (
    <div className="user-profile">
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
      <span className={isActive ? "status-active" : "status-inactive"}>
        {isActive ? "Active" : "Inactive"}
      </span>
    </div>
  );
}
```

### Props with Default Values

```jsx
function Button({ text, color, size, onClick }) {
  return (
    <button 
      className={`btn btn-${color} btn-${size}`}
      onClick={onClick}
    >
      {text}
    </button>
  );
}

// Setting default props
Button.defaultProps = {
  text: "Click Me",
  color: "primary",
  size: "medium",
  onClick: () => console.log("Button clicked!")
};

// Usage
function App() {
  return (
    <div>
      <Button /> {/* Uses all defaults */}
      <Button text="Submit" color="success" />
      <Button text="Delete" color="danger" size="small" />
    </div>
  );
}
```

### Props Validation with PropTypes

```jsx
import PropTypes from 'prop-types';

function ProductCard({ title, price, image, onSale, rating }) {
  return (
    <div className="product-card">
      <img src={image} alt={title} />
      <h3>{title}</h3>
      <p className="price">
        ${price}
        {onSale && <span className="sale-badge">ON SALE!</span>}
      </p>
      <div className="rating">Rating: {rating}/5</div>
    </div>
  );
}

ProductCard.propTypes = {
  title: PropTypes.string.isRequired,
  price: PropTypes.number.isRequired,
  image: PropTypes.string.isRequired,
  onSale: PropTypes.bool,
  rating: PropTypes.number
};

ProductCard.defaultProps = {
  onSale: false,
  rating: 0
};
```

---

## CSS in React

### Theory
React offers multiple ways to style components, each with its own advantages and use cases.

### 1. Inline Styling

**Theory:** Inline styles are defined as JavaScript objects and applied directly to elements.

```jsx
function InlineStyleExample() {
  const containerStyle = {
    backgroundColor: '#f0f0f0',
    padding: '20px',
    borderRadius: '8px',
    boxShadow: '0 2px 4px rgba(0,0,0,0.1)'
  };

  const headingStyle = {
    color: '#333',
    fontSize: '24px',
    marginBottom: '16px',
    textAlign: 'center'
  };

  const buttonStyle = {
    backgroundColor: '#007bff',
    color: 'white',
    border: 'none',
    padding: '10px 20px',
    borderRadius: '4px',
    cursor: 'pointer',
    fontSize: '16px'
  };

  return (
    <div style={containerStyle}>
      <h1 style={headingStyle}>Inline Styled Component</h1>
      <button 
        style={buttonStyle}
        onMouseOver={(e) => e.target.style.backgroundColor = '#0056b3'}
        onMouseOut={(e) => e.target.style.backgroundColor = '#007bff'}
      >
        Hover Me
      </button>
    </div>
  );
}
```

### 2. CSS Stylesheets

**Theory:** Traditional CSS files imported and used with className prop.

```css
/* styles.css */
.card {
  background: white;
  border-radius: 8px;
  padding: 20px;
  margin: 10px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.2s;
}

.card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 16px rgba(0,0,0,0.15);
}

.card-title {
  color: #333;
  font-size: 1.5em;
  margin-bottom: 10px;
}

.card-content {
  color: #666;
  line-height: 1.6;
}

.btn {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.2s;
}

.btn-primary {
  background-color: #007bff;
  color: white;
}

.btn-primary:hover {
  background-color: #0056b3;
}
```

```jsx
// Component using CSS stylesheet
import './styles.css';

function Card({ title, content, onAction }) {
  return (
    <div className="card">
      <h3 className="card-title">{title}</h3>
      <p className="card-content">{content}</p>
      <button className="btn btn-primary" onClick={onAction}>
        Learn More
      </button>
    </div>
  );
}
```

### 3. CSS Modules

**Theory:** CSS Modules provide locally scoped CSS, preventing naming conflicts.

```css
/* Card.module.css */
.container {
  background: white;
  border-radius: 8px;
  padding: 20px;
  margin: 10px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.title {
  color: #333;
  font-size: 1.5em;
  margin-bottom: 10px;
}

.content {
  color: #666;
  line-height: 1.6;
}

.primaryButton {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

.primaryButton:hover {
  background-color: #0056b3;
}
```

```jsx
// Card.jsx
import styles from './Card.module.css';

function Card({ title, content, onAction }) {
  return (
    <div className={styles.container}>
      <h3 className={styles.title}>{title}</h3>
      <p className={styles.content}>{content}</p>
      <button className={styles.primaryButton} onClick={onAction}>
        Learn More
      </button>
    </div>
  );
}

export default Card;
```

### 4. Adding TailwindCSS

**Theory:** Tailwind CSS is a utility-first CSS framework that provides low-level utility classes.

**Installation:**
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

**Configuration (tailwind.config.js):**
```javascript
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        primary: '#007bff',
        secondary: '#6c757d',
      }
    },
  },
  plugins: [],
}
```

**CSS file (src/index.css):**
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

**Tailwind Example:**
```jsx
function TailwindCard({ title, content, image, tags, onLike, liked }) {
  return (
    <div className="max-w-sm mx-auto bg-white rounded-xl shadow-md overflow-hidden hover:shadow-lg transition-shadow duration-300">
      <img 
        className="w-full h-48 object-cover" 
        src={image} 
        alt={title}
      />
      <div className="p-6">
        <div className="flex justify-between items-start mb-2">
          <h3 className="text-xl font-bold text-gray-900">{title}</h3>
          <button
            onClick={onLike}
            className={`p-2 rounded-full transition-colors ${
              liked 
                ? 'text-red-500 hover:text-red-600' 
                : 'text-gray-400 hover:text-red-500'
            }`}
          >
            ❤️
          </button>
        </div>
        <p className="text-gray-600 mb-4 leading-relaxed">{content}</p>
        <div className="flex flex-wrap gap-2 mb-4">
          {tags.map((tag, index) => (
            <span
              key={index}
              className="px-3 py-1 bg-blue-100 text-blue-800 text-sm rounded-full"
            >
              {tag}
            </span>
          ))}
        </div>
        <button className="w-full bg-gradient-to-r from-blue-500 to-purple-600 hover:from-blue-600 hover:to-purple-700 text-white font-medium py-2 px-4 rounded-lg transition-all duration-200 transform hover:scale-105">
          Read More
        </button>
      </div>
    </div>
  );
}
```

---

## Events, States and Component Lifecycle

### Event Handling in React

**Theory:** React uses SyntheticEvents, which wrap native events and provide consistent behavior across browsers.

```jsx
function EventHandlingExamples() {
  const [message, setMessage] = useState('');
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });

  // Basic click handler
  const handleClick = (e) => {
    e.preventDefault();
    console.log('Button clicked!', e.target);
    setMessage('Button was clicked!');
  };

  // Input change handler
  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  };

  // Form submission handler
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Form submitted:', formData);
    alert('Form submitted successfully!');
  };

  // Mouse event handlers
  const handleMouseEnter = () => {
    console.log('Mouse entered!');
  };

  const handleMouseLeave = () => {
    console.log('Mouse left!');
  };

  // Keyboard event handler
  const handleKeyPress = (e) => {
    if (e.key === 'Enter') {
      console.log('Enter key pressed!');
    }
  };

  return (
    <div className="p-6 max-w-md mx-auto bg-white rounded-lg shadow-lg">
      <h2 className="text-2xl font-bold mb-4">Event Handling Examples</h2>
      
      {message && (
        <div className="mb-4 p-2 bg-green-100 text-green-800 rounded">
          {message}
        </div>
      )}

      <form onSubmit={handleSubmit} className="space-y-4">
        <div>
          <input
            type="text"
            name="name"
            placeholder="Your name"
            value={formData.name}
            onChange={handleInputChange}
            onKeyPress={handleKeyPress}
            className="w-full p-2 border rounded"
          />
        </div>

        <div>
          <input
            type="email"
            name="email"
            placeholder="Your email"
            value={formData.email}
            onChange={handleInputChange}
            className="w-full p-2 border rounded"
          />
        </div>

        <div>
          <textarea
            name="message"
            placeholder="Your message"
            value={formData.message}
            onChange={handleInputChange}
            rows="3"
            className="w-full p-2 border rounded"
          />
        </div>

        <div className="flex space-x-2">
          <button
            type="submit"
            className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
          >
            Submit
          </button>
          
          <button
            type="button"
            onClick={handleClick}
            onMouseEnter={handleMouseEnter}
            onMouseLeave={handleMouseLeave}
            className="px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600"
          >
            Click Me
          </button>
        </div>
      </form>
    </div>
  );
}
```

### Creating State

**Theory:** State is a way to store and manage data that can change over time in a React component.

```jsx
import { useState } from 'react';

function Counter() {
  // Simple state
  const [count, setCount] = useState(0);
  
  // Object state
  const [user, setUser] = useState({
    name: '',
    age: 0,
    preferences: {
      theme: 'light',
      notifications: true
    }
  });

  // Array state
  const [items, setItems] = useState(['apple', 'banana', 'orange']);
  const [newItem, setNewItem] = useState('');

  // State update functions
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(0);

  // Update object state
  const updateUser = (field, value) => {
    setUser(prev => ({
      ...prev,
      [field]: value
    }));
  };

  // Update nested object state
  const updatePreference = (key, value) => {
    setUser(prev => ({
      ...prev,
      preferences: {
        ...prev.preferences,
        [key]: value
      }
    }));
  };

  // Add item to array
  const addItem = () => {
    if (newItem.trim()) {
      setItems(prev => [...prev, newItem.trim()]);
      setNewItem('');
    }
  };

  // Remove item from array
  const removeItem = (index) => {
    setItems(prev => prev.filter((_, i) => i !== index));
  };

  return (
    <div className="p-6 max-w-2xl mx-auto space-y-6">
      {/* Counter State */}
      <div className="bg-white p-4 rounded-lg shadow">
        <h3 className="text-xl font-bold mb-4">Counter: {count}</h3>
        <div className="space-x-2">
          <button 
            onClick={decrement}
            className="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600"
          >
            -
          </button>
          <button 
            onClick={increment}
            className="px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600"
          >
            +
          </button>
          <button 
            onClick={reset}
            className="px-4 py-2 bg-gray-500 text-white rounded hover:bg-gray-600"
          >
            Reset
          </button>
        </div>
      </div>

      {/* User Object State */}
      <div className="bg-white p-4 rounded-lg shadow">
        <h3 className="text-xl font-bold mb-4">User Profile</h3>
        <div className="space-y-2">
          <input
            type="text"
            placeholder="Name"
            value={user.name}
            onChange={(e) => updateUser('name', e.target.value)}
            className="w-full p-2 border rounded"
          />
          <input
            type="number"
            placeholder="Age"
            value={user.age}
            onChange={(e) => updateUser('age', parseInt(e.target.value) || 0)}
            className="w-full p-2 border rounded"
          />
          <div className="flex items-center space-x-4">
            <label>
              <input
                type="radio"
                name="theme"
                checked={user.preferences.theme === 'light'}
                onChange={() => updatePreference('theme', 'light')}
              />
              Light Theme
            </label>
            <label>
              <input
                type="radio"
                name="theme"
                checked={user.preferences.theme === 'dark'}
                onChange={() => updatePreference('theme', 'dark')}
              />
              Dark Theme
            </label>
          </div>
          <div>
            <label>
              <input
                type="checkbox"
                checked={user.preferences.notifications}
                onChange={(e) => updatePreference('notifications', e.target.checked)}
              />
              Enable Notifications
            </label>
          </div>
        </div>
        <div className="mt-4 p-2 bg-gray-100 rounded">
          <pre>{JSON.stringify(user, null, 2)}</pre>
        </div>
      </div>

      {/* Items Array State */}
      <div className="bg-white p-4 rounded-lg shadow">
        <h3 className="text-xl font-bold mb-4">Items List</h3>
        <div className="flex space-x-2 mb-4">
          <input
            type="text"
            placeholder="Add new item"
            value={newItem}
            onChange={(e) => setNewItem(e.target.value)}
            onKeyPress={(e) => e.key === 'Enter' && addItem()}
            className="flex-1 p-2 border rounded"
          />
          <button 
            onClick={addItem}
            className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
          >
            Add
          </button>
        </div>
        <ul className="space-y-2">
          {items.map((item, index) => (
            <li key={index} className="flex justify-between items-center p-2 bg-gray-50 rounded">
              <span>{item}</span>
              <button
                onClick={() => removeItem(index)}
                className="px-2 py-1 bg-red-500 text-white text-sm rounded hover:bg-red-600"
              >
                Remove
              </button>
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
}
```

### Stateless and Stateful Components

**Theory:**
- **Stateless Components (Functional Components)**: Don't manage their own state, receive data via props
- **Stateful Components**: Manage their own internal state using hooks or class state

```jsx
// Stateless Component - Pure function that renders UI based on props
function StatelessButton({ label, color, onClick, disabled }) {
  return (
    <button
      onClick={onClick}
      disabled={disabled}
      className={`px-4 py-2 rounded font-medium ${
        disabled
          ? 'bg-gray-300 text-gray-500 cursor-not-allowed'
          : `bg-${color}-500 hover:bg-${color}-600 text-white`
      }`}
    >
      {label}
    </button>
  );
}

// Stateless Component - Display component
function UserCard({ user, onEdit, onDelete }) {
  return (
    <div className="bg-white p-4 rounded-lg shadow border">
      <div className="flex items-center space-x-4">
        <img
          src={user.avatar}
          alt={user.name}
          className="w-12 h-12 rounded-full"
        />
        <div className="flex-1">
          <h3 className="font-semibold">{user.name}</h3>
          <p className="text-gray-600">{user.email}</p>
          <p className="text-sm text-gray-500">{user.role}</p>
        </div>
        <div className="space-x-2">
          <StatelessButton
            label="Edit"
            color="blue"
            onClick={() => onEdit(user.id)}
          />
          <StatelessButton
            label="Delete"
            color="red"
            onClick={() => onDelete(user.id)}
          />
        </div>
      </div>
    </div>
  );
}

// Stateful Component - Manages users data and operations
function UserManagement() {
  const [users, setUsers] = useState([
    {
      id: 1,
      name: "John Doe",
      email: "john@example.com",
      role: "Developer",
      avatar: "https://via.placeholder.com/50"
    },
    {
      id: 2,
      name: "Jane Smith",
      email: "jane@example.com",
      role: "Designer",
      avatar: "https://via.placeholder.com/50"
    }
  ]);

  const [newUser, setNewUser] = useState({
    name: '',
    email: '',
    role: ''
  });

  const [editingUser, setEditingUser] = useState(null);

  const handleAddUser = () => {
    if (newUser.name && newUser.email && newUser.role) {
      const user = {
        id: Date.now(),
        ...newUser,
        avatar: "https://via.placeholder.com/50"
      };
      setUsers(prev => [...prev, user]);
      setNewUser({ name: '', email: '', role: '' });
    }
  };

  const handleEditUser = (userId) => {
    const user = users.find(u => u.id === userId);
    setEditingUser(user);
  };

  const handleDeleteUser = (userId) => {
    setUsers(prev => prev.filter(u => u.id !== userId));
  };

  const handleUpdateUser = () => {
    setUsers(prev => 
      prev.map(u => u.id === editingUser.id ? editingUser : u)
    );
    setEditingUser(null);
  };

  return (
    <div className="p-6 max-w-4xl mx-auto">
      <h2 className="text-2xl font-bold mb-6">User Management</h2>

      {/* Add New User Form */}
      <div className="bg-gray-50 p-4 rounded-lg mb-6">
        <h3 className="text-lg font-semibold mb-4">Add New User</h3>
        <div className="grid grid-cols-1 md:grid-cols-4 gap-4">
          <input
            type="text"
            placeholder="Name"
            value={newUser.name}
            onChange={(e) => setNewUser(prev => ({ ...prev, name: e.target.value }))}
            className="p-2 border rounded"
          />
          <input
            type="email"
            placeholder="Email"
            value={newUser.email}
            onChange={(e) => setNewUser(prev => ({ ...prev, email: e.target.value }))}
            className="p-2 border rounded"
          />
          <input
            type="text"
            placeholder="Role"
            value={newUser.role}
            onChange={(e) => setNewUser(prev => ({ ...prev, role: e.target.value }))}
            className="p-2 border rounded"
          />
          <StatelessButton
            label="Add User"
            color="green"
            onClick={handleAddUser}
            disabled={!newUser.name || !newUser.email || !newUser.role}
          />
        </div>
      </div>

      {/* Edit User Modal */}
      {editingUser && (
        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
          <div className="bg-white p-6 rounded-lg w-96">
            <h3 className="text-lg font-semibold mb-4">Edit User</h3>
            <div className="space-y-4">
              <input
                type="text"
                value={editingUser.name}
                onChange={(e) => setEditingUser(prev => ({ ...prev, name: e.target.value }))}
                className="w-full p-2 border rounded"
              />
              <input
                type="email"
                value={editingUser.email}
                onChange={(e) => setEditingUser(prev => ({ ...prev, email: e.target.value }))}
                className="w-full p-2 border rounded"
              />
              <input
                type="text"
                value={editingUser.role}
                onChange={(e) => setEditingUser(prev => ({ ...prev, role: e.target.value }))}
                className="w-full p-2 border rounded"
              />
              <div className="flex space-x-2">
                <StatelessButton
                  label="Update"
                  color="blue"
                  onClick={handleUpdateUser}
                />
                <StatelessButton
                  label="Cancel"
                  color="gray"
                  onClick={() => setEditingUser(null)}
                />
              </div>
            </div>
          </div>
        </div>
      )}

      {/* Users List */}
      <div className="space-y-4">
        {users.map(user => (
          <UserCard
            key={user.id}
            user={user}
            onEdit={handleEditUser}
            onDelete={handleDeleteUser}
          />
        ))}
      </div>
    </div>
  );
}
```

### Component Lifecycle

**Theory:** Component lifecycle refers to the different phases a component goes through from creation to destruction.

**Lifecycle Phases:**
1. **Mounting**: Component is being created and inserted into the DOM
2. **Updating**: Component is being re-rendered as a result of changes to props or state
3. **Unmounting**: Component is being removed from the DOM

```jsx
import { useState, useEffect } from 'react';

// Component demonstrating lifecycle phases
function LifecycleDemo({ visible }) {
  const [count, setCount] = useState(0);
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  // Mounting Phase - useEffect with empty dependency array
  useEffect(() => {
    console.log('Component Mounted');
    
    // Simulate API call
    const fetchData = async () => {
      setLoading(true);
      try {
        await new Promise(resolve => setTimeout(resolve, 1000)); // Simulate delay
        setData({ message: 'Data loaded successfully!', timestamp: new Date().toLocaleTimeString() });
      } catch (error) {
        console.error('Failed to fetch data:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchData();

    // Cleanup function (runs on unmount)
    return () => {
      console.log('Component will unmount - cleaning up');
    };
  }, []); // Empty dependency array = runs once on mount

  // Update Phase - useEffect with dependencies
  useEffect(() => {
    console.log('Count updated:', count);
    
    // Update document title
    document.title = `Count: ${count}`;
    
    // Cleanup for this effect
    return () => {
      document.title = 'React App'; // Reset title
    };
  }, [count]); // Runs when count changes

  // Effect that runs on every render
  useEffect(() => {
    console.log('Component rendered/re-rendered');
  });

  // Effect with multiple dependencies
  useEffect(() => {
    if (data && count > 5) {
      console.log('Special condition met: data loaded and count > 5');
    }
  }, [data, count]);

  if (!visible) {
    return null; // This will trigger unmounting
  }

  return (
    <div className="p-6 max-w-md mx-auto bg-white rounded-lg shadow-lg">
      <h2 className="text-2xl font-bold mb-4">Lifecycle Demo</h2>
      
      {loading ? (
        <div className="text-center py-4">
          <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-500 mx-auto"></div>
          <p className="mt-2">Loading...</p>
        </div>
      ) : (
        <div className="mb-4 p-3 bg-green-100 rounded">
          <p className="text-green-800">{data?.message}</p>
          <small className="text-green-600">Loaded at: {data?.timestamp}</small>
        </div>
      )}

      <div className="text-center mb-4">
        <h3 className="text-xl font-semibold">Count: {count}</h3>
      </div>

      <div className="flex space-x-2 justify-center">
        <button
          onClick={() => setCount(c => c - 1)}
          className="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600"
        >
          -1
        </button>
        <button
          onClick={() => setCount(c => c + 1)}
          className="px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600"
        >
          +1
        </button>
        <button
          onClick={() => setCount(0)}
          className="px-4 py-2 bg-gray-500 text-white rounded hover:bg-gray-600"
        >
          Reset
        </button>
      </div>

      <div className="mt-4 text-sm text-gray-600">
        <p>Check the console to see lifecycle messages!</p>
        <p>Document title updates with count.</p>
        {count > 5 && <p className="text-blue-600 font-semibold">Special condition triggered!</p>}
      </div>
    </div>
  );
}

// Parent component to control visibility
function LifecycleParent() {
  const [showComponent, setShowComponent] = useState(true);

  return (
    <div className="p-6">
      <div className="text-center mb-6">
        <button
          onClick={() => setShowComponent(!showComponent)}
          className="px-6 py-2 bg
