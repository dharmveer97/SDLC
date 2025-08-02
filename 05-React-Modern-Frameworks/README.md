# ⚛️ React & Modern Frameworks

## 🎯 What Is React?

React is a JavaScript library for building user interfaces. Think of it like:

- **LEGO blocks** for websites
- Each component is a reusable piece
- Snap them together to build complex apps

### Why React?

Before React, updating websites was like:

1. Find the element
2. Change its content
3. Repeat for every change

With React:

1. Describe what you want
2. React figures out what changed
3. Updates only what needs updating

## 🧩 React Concepts

### Components - Building Blocks

A component is a piece of UI that can be reused.

```jsx
// Function Component (modern way)
function WelcomeMessage() {
  return <h1>Welcome to our site!</h1>;
}

// Using the component
<WelcomeMessage />;
```

### JSX - HTML in JavaScript

JSX lets you write HTML-like code in JavaScript:

```jsx
// This is JSX
const element = <h1>Hello, World!</h1>;

// It's actually JavaScript:
const element = React.createElement('h1', null, 'Hello, World!');

// JSX makes it readable!
```

### Props - Passing Data

Props are like function arguments for components:

```jsx
// Component with props
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// Using it
<Greeting name="Alice" />
<Greeting name="Bob" />

// Result:
// Hello, Alice!
// Hello, Bob!
```

### State - Component Memory

State is data that can change over time:

```jsx
import { useState } from 'react';

function Counter() {
  // Create state variable
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

## 🚀 Building a React App

### Setting Up

```bash
# Create new React app
npx create-react-app my-app
cd my-app
npm start

# Or using Vite (faster, modern)
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev
```

### Basic React App Structure

```
my-app/
├── src/
│   ├── App.js        # Main component
│   ├── App.css       # Styles
│   ├── index.js      # Entry point
│   └── components/   # Your components
├── public/
│   └── index.html    # HTML template
├── package.json      # Dependencies
└── README.md
```

### Your First Component

```jsx
// src/components/ProductCard.js
function ProductCard(props) {
  return (
    <div className="product-card">
      <img src={props.image} alt={props.name} />
      <h3>{props.name}</h3>
      <p>${props.price}</p>
      <button>Add to Cart</button>
    </div>
  );
}

export default ProductCard;

// Using it in App.js
import ProductCard from './components/ProductCard';

function App() {
  return (
    <div className="app">
      <h1>Our Products</h1>
      <ProductCard name="Laptop" price={999} image="/laptop.jpg" />
      <ProductCard name="Phone" price={599} image="/phone.jpg" />
    </div>
  );
}
```

## 🎣 React Hooks

Hooks are functions that let you use React features:

### useState - Managing State

```jsx
function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState('');

  const addTodo = () => {
    setTodos([...todos, input]);
    setInput('');
  };

  return (
    <div>
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

### useEffect - Side Effects

```jsx
import { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Run when component mounts or userId changes
    fetch(`/api/users/${userId}`)
      .then((res) => res.json())
      .then((data) => {
        setUser(data);
        setLoading(false);
      });
  }, [userId]); // Dependencies

  if (loading) return <p>Loading...</p>;

  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}
```

### Custom Hooks

```jsx
// Custom hook for API calls
function useApi(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch(url)
      .then((res) => res.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      })
      .catch((err) => {
        setError(err);
        setLoading(false);
      });
  }, [url]);

  return { data, loading, error };
}

// Using the custom hook
function Products() {
  const { data: products, loading, error } = useApi('/api/products');

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <div>
      {products.map((product) => (
        <ProductCard key={product.id} {...product} />
      ))}
    </div>
  );
}
```

## 🏗️ Building a Complete React App

Let's build a simple task management app:

```jsx
// App.js
import React, { useState, useEffect } from 'react';
import './App.css';

// Task component
function Task({ task, onToggle, onDelete }) {
  return (
    <div className={`task ${task.completed ? 'completed' : ''}`}>
      <input
        type="checkbox"
        checked={task.completed}
        onChange={() => onToggle(task.id)}
      />
      <span>{task.text}</span>
      <button onClick={() => onDelete(task.id)}>Delete</button>
    </div>
  );
}

// TaskForm component
function TaskForm({ onAdd }) {
  const [text, setText] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (text.trim()) {
      onAdd(text);
      setText('');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Add a new task..."
      />
      <button type="submit">Add Task</button>
    </form>
  );
}

// Main App component
function App() {
  const [tasks, setTasks] = useState(() => {
    // Load from localStorage
    const saved = localStorage.getItem('tasks');
    return saved ? JSON.parse(saved) : [];
  });

  // Save to localStorage whenever tasks change
  useEffect(() => {
    localStorage.setItem('tasks', JSON.stringify(tasks));
  }, [tasks]);

  const addTask = (text) => {
    setTasks([
      ...tasks,
      {
        id: Date.now(),
        text,
        completed: false,
      },
    ]);
  };

  const toggleTask = (id) => {
    setTasks(
      tasks.map((task) =>
        task.id === id ? { ...task, completed: !task.completed } : task,
      ),
    );
  };

  const deleteTask = (id) => {
    setTasks(tasks.filter((task) => task.id !== id));
  };

  return (
    <div className="app">
      <h1>Task Manager</h1>
      <TaskForm onAdd={addTask} />
      <div className="tasks">
        {tasks.map((task) => (
          <Task
            key={task.id}
            task={task}
            onToggle={toggleTask}
            onDelete={deleteTask}
          />
        ))}
      </div>
      <p>{tasks.filter((t) => !t.completed).length} tasks remaining</p>
    </div>
  );
}

export default App;
```

## 🆚 React vs Other Frameworks

### Create React App vs Vite vs Next.js

#### Create React App (CRA)

```bash
npx create-react-app my-app
```

- ✅ Official React tool
- ✅ Good for learning
- ❌ Slow build times
- ❌ Being phased out

#### Vite

```bash
npm create vite@latest my-app -- --template react
```

- ✅ Super fast
- ✅ Modern tooling
- ✅ Great developer experience
- ✅ Recommended for new projects

#### Next.js

```bash
npx create-next-app@latest my-app
```

- ✅ Full-stack React framework
- ✅ Server-side rendering (SSR)
- ✅ Built-in routing
- ✅ API routes
- ✅ Great for production apps

### When to Use What?

**Use Vite when:**

- Building single-page apps (SPAs)
- Learning React
- Need fast development
- Client-side only apps

**Use Next.js when:**

- Need SEO (Google needs to find your pages)
- Building e-commerce sites
- Need server-side features
- Want built-in optimizations

## 🚀 Next.js Deep Dive

### What Makes Next.js Special?

1. **File-based Routing**

```
pages/
├── index.js        → /
├── about.js        → /about
├── products/
│   ├── index.js    → /products
│   └── [id].js     → /products/123
```

2. **Server-Side Rendering (SSR)**

```jsx
// This runs on the server!
export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/products');
  const products = await res.json();

  return {
    props: { products },
  };
}

function Products({ products }) {
  return (
    <div>
      {products.map((product) => (
        <div key={product.id}>{product.name}</div>
      ))}
    </div>
  );
}
```

3. **API Routes**

```jsx
// pages/api/hello.js
export default function handler(req, res) {
  res.status(200).json({ message: 'Hello from API!' });
}

// Access at: /api/hello
```

### Next.js App Example

```jsx
// pages/index.js
import Link from 'next/link';
import Head from 'next/head';

export default function Home() {
  return (
    <>
      <Head>
        <title>My Store</title>
        <meta name="description" content="Best products online" />
      </Head>

      <main>
        <h1>Welcome to My Store</h1>
        <Link href="/products">
          <a>View Products</a>
        </Link>
      </main>
    </>
  );
}

// pages/products/[id].js
import { useRouter } from 'next/router';

export async function getStaticPaths() {
  // Get all product IDs
  const products = await fetch('/api/products').then(r => r.json());

  const paths = products.map(product => ({
    params: { id: product.id.toString() }
  }));

  return { paths, fallback: false };
}

export async function getStaticProps({ params }) {
  // Get specific product
  const product = await fetch(`/api/products/${params.id}`).then(r => r.json());

  return {
    props: { product }
  };
}

export default function Product({ product }) {
  return (
    <div>
      <h1>{product.name}</h1>
      <p>${product.price}</p>
      <button>Add to Cart</button>
    </div>
  );
}
```

## 🔄 State Management

### Context API (Built-in)

```jsx
// Create context
const ThemeContext = React.createContext();

// Provider component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Using context
function Header() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <header className={theme}>
      <button onClick={toggleTheme}>
        Switch to {theme === 'light' ? 'dark' : 'light'} mode
      </button>
    </header>
  );
}

// App setup
function App() {
  return (
    <ThemeProvider>
      <Header />
      <Main />
    </ThemeProvider>
  );
}
```

### Redux (External Library)

```jsx
// Redux is more complex but powerful for large apps
// Basic concept:

// 1. Actions
const increment = () => ({ type: 'INCREMENT' });
const decrement = () => ({ type: 'DECREMENT' });

// 2. Reducer
const counterReducer = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
};

// 3. Using in component
function Counter() {
  const count = useSelector((state) => state.counter);
  const dispatch = useDispatch();

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  );
}
```

## 🎨 Styling in React

### CSS Modules

```css
/* Button.module.css */
.button {
  background: blue;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
}

.button:hover {
  background: darkblue;
}
```

```jsx
// Button.js
import styles from './Button.module.css';

function Button({ children, onClick }) {
  return (
    <button className={styles.button} onClick={onClick}>
      {children}
    </button>
  );
}
```

### Styled Components

```jsx
import styled from 'styled-components';

const Button = styled.button`
  background: ${props => props.primary ? 'blue' : 'gray'};
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;

  &:hover {
    opacity: 0.8;
  }
`;

// Using it
<Button primary>Primary Button</Button>
<Button>Secondary Button</Button>
```

## 🔍 React Developer Tools

Browser extension that helps debug React:

- See component tree
- Inspect props and state
- Track re-renders
- Profile performance

## 💡 React Best Practices

### 1. Component Organization

```jsx
// Good: Small, focused components
function UserAvatar({ user }) {
  return <img src={user.avatar} alt={user.name} />;
}

function UserInfo({ user }) {
  return (
    <div>
      <UserAvatar user={user} />
      <h3>{user.name}</h3>
    </div>
  );
}
```

### 2. Key Prop for Lists

```jsx
// Always use unique keys
{
  users.map((user) => <UserCard key={user.id} user={user} />);
}

// NOT index (unless no other option)
{
  users.map((user, index) => (
    <UserCard key={index} user={user} /> // Avoid this
  ));
}
```

### 3. Event Handler Naming

```jsx
// Good naming convention
<button onClick={handleClick}>Click</button>
<form onSubmit={handleSubmit}>
  <input onChange={handleInputChange} />
</form>
```

## 🌐 Other Modern Frameworks

### Vue.js

- Similar to React but simpler
- Template-based
- Good for beginners

### Angular

- Full framework (not just UI)
- Uses TypeScript
- Good for large enterprise apps

### Svelte

- Compiles to vanilla JavaScript
- No virtual DOM
- Very fast

### Solid.js

- Similar to React
- Better performance
- Growing popularity

## 🎯 What This Means for BAs

Understanding React helps you:

1. **Know component structure** - How UIs are broken down
2. **Understand reusability** - Why developers create components
3. **Grasp state management** - How data flows in apps
4. **Appreciate rendering** - Why some updates are fast/slow
5. **Write better specs** - Component-based requirements

## ✅ Key Takeaways

- React breaks UIs into reusable components
- Components can have props (inputs) and state (memory)
- JSX makes writing components intuitive
- Hooks add functionality to components
- Next.js adds server-side features to React
- Modern tooling (Vite) makes development fast
- Choose framework based on project needs

Next Module: [Backend Development →](../06-Backend-Development/README.md)
