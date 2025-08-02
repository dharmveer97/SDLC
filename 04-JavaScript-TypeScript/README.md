# 💻 JavaScript & TypeScript - Making Websites Interactive

## 🎯 What Is JavaScript?

JavaScript is the programming language that makes websites interactive. If HTML is the skeleton and CSS is the skin, JavaScript is the brain and muscles.

### JavaScript Can:
- React to user clicks
- Update content without refreshing
- Validate forms
- Create animations
- Fetch data from servers
- Build entire applications

## 🏁 JavaScript Basics

### Where to Write JavaScript

#### 1. In HTML file
```html
<script>
  alert("Hello World!");
</script>
```

#### 2. External file (recommended)
```html
<script src="script.js"></script>
```

### Variables - Storing Information

```javascript
// Old way (avoid)
var name = "John";

// Modern ways
let age = 25;        // Can change
const city = "NYC";  // Cannot change

// Examples
let score = 0;
score = 10;          // ✅ This works

const pi = 3.14;
pi = 3.15;           // ❌ Error! Cannot change const
```

### Data Types

```javascript
// 1. Numbers
let age = 25;
let price = 19.99;
let temperature = -5;

// 2. Strings (text)
let firstName = "John";
let lastName = 'Doe';
let message = `Hello ${firstName}`; // Template literal

// 3. Booleans (true/false)
let isLoggedIn = true;
let isVip = false;

// 4. Arrays (lists)
let fruits = ["apple", "banana", "orange"];
let numbers = [1, 2, 3, 4, 5];
let mixed = ["John", 25, true];

// 5. Objects (key-value pairs)
let user = {
  name: "John",
  age: 25,
  email: "john@example.com",
  isActive: true
};

// 6. Null and Undefined
let empty = null;        // Intentionally empty
let notDefined;          // undefined (no value assigned)
```

### Basic Operations

```javascript
// Math
let sum = 10 + 5;         // 15
let difference = 10 - 5;  // 5
let product = 10 * 5;     // 50
let quotient = 10 / 5;    // 2
let remainder = 10 % 3;   // 1 (modulo)

// String operations
let greeting = "Hello" + " " + "World";  // "Hello World"
let name = "John";
let welcome = `Welcome, ${name}!`;       // "Welcome, John!"

// Comparison
let isEqual = 5 === 5;        // true (strict equality)
let isNotEqual = 5 !== 3;     // true
let isGreater = 10 > 5;       // true
let isLess = 3 < 8;           // true
```

### Functions - Reusable Code

```javascript
// Function declaration
function greet(name) {
  return `Hello, ${name}!`;
}

// Function call
let message = greet("Alice");  // "Hello, Alice!"

// Arrow function (modern way)
const add = (a, b) => {
  return a + b;
};

// Shorter arrow function
const multiply = (a, b) => a * b;

// Examples
function calculateTotal(price, tax) {
  const taxAmount = price * tax;
  const total = price + taxAmount;
  return total;
}

let finalPrice = calculateTotal(100, 0.08);  // 108
```

### Conditionals - Making Decisions

```javascript
// If statement
let age = 18;

if (age >= 18) {
  console.log("You can vote!");
} else {
  console.log("Too young to vote");
}

// Multiple conditions
let score = 85;

if (score >= 90) {
  console.log("Grade: A");
} else if (score >= 80) {
  console.log("Grade: B");
} else if (score >= 70) {
  console.log("Grade: C");
} else {
  console.log("Grade: F");
}

// Ternary operator (shorthand)
let status = age >= 18 ? "Adult" : "Minor";

// Switch statement
let day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of work week");
    break;
  case "Friday":
    console.log("TGIF!");
    break;
  case "Saturday":
  case "Sunday":
    console.log("Weekend!");
    break;
  default:
    console.log("Regular day");
}
```

### Loops - Repeating Actions

```javascript
// For loop
for (let i = 0; i < 5; i++) {
  console.log(`Count: ${i}`);
}

// While loop
let count = 0;
while (count < 5) {
  console.log(`Count: ${count}`);
  count++;
}

// Looping through arrays
const fruits = ["apple", "banana", "orange"];

// For...of loop (modern)
for (const fruit of fruits) {
  console.log(fruit);
}

// forEach method
fruits.forEach(fruit => {
  console.log(`I like ${fruit}`);
});

// Map - transform array
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(num => num * 2);
// Result: [2, 4, 6, 8, 10]

// Filter - keep certain items
const adults = [
  { name: "John", age: 25 },
  { name: "Jane", age: 17 },
  { name: "Bob", age: 30 }
];

const onlyAdults = adults.filter(person => person.age >= 18);
// Result: John and Bob
```

### Working with Objects

```javascript
// Creating objects
const user = {
  firstName: "John",
  lastName: "Doe",
  age: 30,
  email: "john@example.com",
  address: {
    street: "123 Main St",
    city: "Boston",
    zip: "02101"
  }
};

// Accessing properties
console.log(user.firstName);           // "John"
console.log(user["lastName"]);          // "Doe"
console.log(user.address.city);         // "Boston"

// Modifying properties
user.age = 31;
user.phone = "555-1234";  // Adding new property

// Object methods
const calculator = {
  add: function(a, b) {
    return a + b;
  },
  subtract: (a, b) => a - b,
  // Shorthand method
  multiply(a, b) {
    return a * b;
  }
};

console.log(calculator.add(5, 3));      // 8
console.log(calculator.multiply(4, 2)); // 8
```

## 🌐 DOM Manipulation - Changing Web Pages

The DOM (Document Object Model) is how JavaScript interacts with HTML.

### Selecting Elements

```javascript
// By ID
const header = document.getElementById('header');

// By class
const buttons = document.getElementsByClassName('btn');

// By tag
const paragraphs = document.getElementsByTagName('p');

// Modern selectors (like CSS)
const header2 = document.querySelector('#header');        // One element
const allButtons = document.querySelectorAll('.btn');    // All elements

// Examples
const submitBtn = document.querySelector('#submit-button');
const errorMsg = document.querySelector('.error-message');
const firstParagraph = document.querySelector('p');
```

### Changing Content

```javascript
// Change text
const title = document.querySelector('h1');
title.textContent = "New Title";

// Change HTML
const container = document.querySelector('.container');
container.innerHTML = '<p>New paragraph</p>';

// Change attributes
const image = document.querySelector('img');
image.src = "new-image.jpg";
image.alt = "New description";

// Change styles
const box = document.querySelector('.box');
box.style.backgroundColor = 'blue';
box.style.width = '200px';
box.style.display = 'none';  // Hide element
```

### Event Handling

```javascript
// Click event
const button = document.querySelector('#myButton');
button.addEventListener('click', function() {
  alert('Button clicked!');
});

// With arrow function
button.addEventListener('click', () => {
  console.log('Clicked!');
});

// Form submission
const form = document.querySelector('#myForm');
form.addEventListener('submit', (event) => {
  event.preventDefault();  // Stop form from submitting
  
  const name = document.querySelector('#name').value;
  console.log(`Form submitted with name: ${name}`);
});

// Input events
const input = document.querySelector('#textInput');
input.addEventListener('input', (e) => {
  console.log(`You typed: ${e.target.value}`);
});

// Common events
element.addEventListener('click', handler);     // Click
element.addEventListener('submit', handler);    // Form submit
element.addEventListener('input', handler);     // Text input
element.addEventListener('change', handler);    // Select/checkbox change
element.addEventListener('mouseover', handler); // Mouse hover
element.addEventListener('keydown', handler);   // Key press
```

### Creating Elements

```javascript
// Create new element
const newDiv = document.createElement('div');
newDiv.textContent = 'Hello World';
newDiv.className = 'message';
newDiv.id = 'welcome';

// Add to page
document.body.appendChild(newDiv);

// Insert before another element
const container = document.querySelector('.container');
const reference = document.querySelector('.existing');
container.insertBefore(newDiv, reference);

// Complete example
function addTodoItem(text) {
  const li = document.createElement('li');
  li.textContent = text;
  
  const deleteBtn = document.createElement('button');
  deleteBtn.textContent = 'Delete';
  deleteBtn.addEventListener('click', () => {
    li.remove();
  });
  
  li.appendChild(deleteBtn);
  document.querySelector('#todo-list').appendChild(li);
}
```

## ⏰ Asynchronous JavaScript

### setTimeout and setInterval

```javascript
// Run once after delay
setTimeout(() => {
  console.log('This runs after 2 seconds');
}, 2000);

// Run repeatedly
const timer = setInterval(() => {
  console.log('This runs every second');
}, 1000);

// Stop interval
clearInterval(timer);
```

### Promises

```javascript
// Creating a promise
const myPromise = new Promise((resolve, reject) => {
  const success = true;
  
  if (success) {
    resolve('Operation succeeded!');
  } else {
    reject('Operation failed!');
  }
});

// Using a promise
myPromise
  .then(result => {
    console.log(result);  // "Operation succeeded!"
  })
  .catch(error => {
    console.log(error);
  });
```

### Async/Await (Modern Way)

```javascript
// Async function
async function fetchUserData() {
  try {
    const response = await fetch('/api/user');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
  }
}

// Using it
const userData = await fetchUserData();
```

### Fetching Data from APIs

```javascript
// Using fetch (modern way)
fetch('https://api.example.com/users')
  .then(response => response.json())
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error('Error:', error);
  });

// Using async/await
async function getUsers() {
  try {
    const response = await fetch('https://api.example.com/users');
    const users = await response.json();
    
    // Display users
    users.forEach(user => {
      console.log(user.name);
    });
  } catch (error) {
    console.error('Failed to fetch users:', error);
  }
}
```

## 🔷 TypeScript - JavaScript with Types

### What Is TypeScript?

TypeScript is JavaScript with type checking. It helps catch errors before your code runs.

Think of it like spell-check for code:
- JavaScript: You can write anything, errors show when running
- TypeScript: Errors show while writing, before running

### Basic Types

```typescript
// Type annotations
let username: string = "John";
let age: number = 25;
let isActive: boolean = true;

// Arrays
let numbers: number[] = [1, 2, 3];
let names: string[] = ["John", "Jane"];

// Objects
interface User {
  id: number;
  name: string;
  email: string;
  isAdmin?: boolean;  // Optional property
}

const user: User = {
  id: 1,
  name: "John",
  email: "john@example.com"
};

// Functions
function add(a: number, b: number): number {
  return a + b;
}

const multiply = (x: number, y: number): number => {
  return x * y;
};

// Function with optional parameter
function greet(name: string, title?: string): string {
  if (title) {
    return `Hello, ${title} ${name}`;
  }
  return `Hello, ${name}`;
}
```

### TypeScript Interfaces

```typescript
// Defining structure
interface Product {
  id: number;
  name: string;
  price: number;
  inStock: boolean;
  categories: string[];
}

// Using interface
const laptop: Product = {
  id: 1,
  name: "MacBook Pro",
  price: 1999,
  inStock: true,
  categories: ["Electronics", "Computers"]
};

// Function using interface
function calculateDiscount(product: Product, percent: number): number {
  return product.price * (percent / 100);
}
```

### Type vs Interface

```typescript
// Type alias
type ID = string | number;  // Can be either string or number

type Status = "active" | "inactive" | "pending";  // Specific values

// Using types
let userId: ID = "abc123";
userId = 123;  // Also valid

let userStatus: Status = "active";
userStatus = "deleted";  // ❌ Error! Not a valid status
```

### Generics (Advanced)

```typescript
// Generic function
function getFirstItem<T>(items: T[]): T | undefined {
  return items[0];
}

const firstNumber = getFirstItem([1, 2, 3]);    // number
const firstName = getFirstItem(["a", "b", "c"]); // string

// Generic interface
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

const userResponse: ApiResponse<User> = {
  data: { id: 1, name: "John", email: "john@example.com" },
  status: 200,
  message: "Success"
};
```

## 🛠️ Real-World JavaScript Examples

### Form Validation

```javascript
const form = document.querySelector('#signup-form');
const emailInput = document.querySelector('#email');
const passwordInput = document.querySelector('#password');
const errorDiv = document.querySelector('#error-message');

form.addEventListener('submit', (e) => {
  e.preventDefault();
  
  // Clear previous errors
  errorDiv.textContent = '';
  
  // Validate email
  const email = emailInput.value;
  if (!email.includes('@')) {
    errorDiv.textContent = 'Please enter a valid email';
    return;
  }
  
  // Validate password
  const password = passwordInput.value;
  if (password.length < 8) {
    errorDiv.textContent = 'Password must be at least 8 characters';
    return;
  }
  
  // If valid, submit
  console.log('Form is valid! Submitting...');
});
```

### Dynamic Content Loading

```javascript
// Show loading spinner
function showLoading() {
  document.querySelector('#loading').style.display = 'block';
}

// Hide loading spinner
function hideLoading() {
  document.querySelector('#loading').style.display = 'none';
}

// Load and display posts
async function loadPosts() {
  showLoading();
  
  try {
    const response = await fetch('/api/posts');
    const posts = await response.json();
    
    const container = document.querySelector('#posts-container');
    container.innerHTML = '';
    
    posts.forEach(post => {
      const postDiv = document.createElement('div');
      postDiv.className = 'post';
      postDiv.innerHTML = `
        <h3>${post.title}</h3>
        <p>${post.content}</p>
        <small>By ${post.author}</small>
      `;
      container.appendChild(postDiv);
    });
  } catch (error) {
    console.error('Failed to load posts:', error);
    document.querySelector('#error').textContent = 'Failed to load posts';
  } finally {
    hideLoading();
  }
}

// Load posts when page loads
window.addEventListener('load', loadPosts);
```

### Interactive Todo List

```javascript
class TodoList {
  constructor() {
    this.todos = [];
    this.todoInput = document.querySelector('#todo-input');
    this.todoList = document.querySelector('#todo-list');
    this.addButton = document.querySelector('#add-button');
    
    this.addButton.addEventListener('click', () => this.addTodo());
    this.todoInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') this.addTodo();
    });
  }
  
  addTodo() {
    const text = this.todoInput.value.trim();
    if (!text) return;
    
    const todo = {
      id: Date.now(),
      text: text,
      completed: false
    };
    
    this.todos.push(todo);
    this.todoInput.value = '';
    this.render();
  }
  
  toggleTodo(id) {
    const todo = this.todos.find(t => t.id === id);
    if (todo) {
      todo.completed = !todo.completed;
      this.render();
    }
  }
  
  deleteTodo(id) {
    this.todos = this.todos.filter(t => t.id !== id);
    this.render();
  }
  
  render() {
    this.todoList.innerHTML = '';
    
    this.todos.forEach(todo => {
      const li = document.createElement('li');
      li.className = todo.completed ? 'completed' : '';
      
      li.innerHTML = `
        <input type="checkbox" ${todo.completed ? 'checked' : ''}>
        <span>${todo.text}</span>
        <button class="delete">Delete</button>
      `;
      
      li.querySelector('input').addEventListener('change', () => {
        this.toggleTodo(todo.id);
      });
      
      li.querySelector('.delete').addEventListener('click', () => {
        this.deleteTodo(todo.id);
      });
      
      this.todoList.appendChild(li);
    });
  }
}

// Initialize
const app = new TodoList();
```

## 💡 JavaScript vs TypeScript

### When to Use JavaScript
- Small projects
- Quick prototypes
- Learning to code
- When team doesn't know TypeScript

### When to Use TypeScript
- Large applications
- Team projects
- Long-term maintenance
- When catching bugs early is important

### Example Comparison

```javascript
// JavaScript
function calculatePrice(price, tax, discount) {
  return price + (price * tax) - discount;
}

calculatePrice(100, 0.08, "10");  // Bug! "10" is a string
```

```typescript
// TypeScript
function calculatePrice(price: number, tax: number, discount: number): number {
  return price + (price * tax) - discount;
}

calculatePrice(100, 0.08, "10");  // ❌ Error caught immediately!
```

## 🎯 What This Means for BAs

Understanding JavaScript helps you:

1. **Know what's possible** - Interactive features, validations, animations
2. **Estimate complexity** - Simple click vs complex calculation
3. **Write better requirements** - "On click, validate email format"
4. **Understand limitations** - Browser compatibility, performance
5. **Debug issues** - "The button click handler isn't firing"

## ✅ Key Takeaways

- JavaScript makes websites interactive
- Variables store data, functions perform actions
- DOM manipulation changes what users see
- Events respond to user actions
- Async code handles time-based operations
- TypeScript adds type safety to JavaScript
- Modern JavaScript (ES6+) is cleaner and more powerful

Next Module: [React & Modern Frameworks →](../05-React-Modern-Frameworks/README.md)