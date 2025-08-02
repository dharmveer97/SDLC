# 🖥️ Backend Development - Node.js, APIs, and Server-Side Programming

## 🎯 What Is Backend Development?

Think of a restaurant:

- **Frontend** = Dining room (what customers see)
- **Backend** = Kitchen (where the work happens)
- **Database** = Pantry (where ingredients are stored)
- **API** = Waiter (carries orders and food)

Backend handles:

- User authentication (login/logout)
- Database operations
- Business logic
- Security
- API endpoints
- Email sending
- File processing

## 🟢 Node.js - JavaScript on the Server

### What Is Node.js?

Node.js lets you run JavaScript outside the browser. Before Node.js:

- JavaScript = Only in browsers
- Servers = PHP, Java, Python

With Node.js:

- JavaScript everywhere!
- Same language for frontend and backend

### Your First Node.js Program

```javascript
// hello.js
console.log('Hello from Node.js!');

// Run it:
// node hello.js
```

### Node.js Built-in Modules

```javascript
// File System (fs) - Work with files
const fs = require('fs');

// Read a file
fs.readFile('data.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Error reading file:', err);
    return;
  }
  console.log('File contents:', data);
});

// Write a file
fs.writeFile('output.txt', 'Hello World', (err) => {
  if (err) {
    console.error('Error writing file:', err);
    return;
  }
  console.log('File written successfully');
});

// HTTP - Create web server
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello World from Node.js server!');
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

## 📦 npm - Node Package Manager

### What Is npm?

npm = App store for Node.js packages

```bash
# Initialize a new project
npm init -y

# Install a package
npm install express

# Install dev dependency
npm install --save-dev nodemon

# Install globally
npm install -g typescript
```

### package.json Explained

```json
{
  "name": "my-backend",
  "version": "1.0.0",
  "description": "My backend server",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.0", // For building APIs
    "mongoose": "^7.0.0", // For MongoDB
    "bcrypt": "^5.1.0", // For password hashing
    "jsonwebtoken": "^9.0.0" // For authentication
  },
  "devDependencies": {
    "nodemon": "^3.0.0", // Auto-restart server
    "jest": "^29.0.0" // Testing
  }
}
```

## 🚀 Express.js - Web Framework

### What Is Express?

Express makes building web servers easy. It's like Rails for Ruby or Django for Python.

### Basic Express Server

```javascript
const express = require('express');
const app = express();
const port = 3000;

// Middleware to parse JSON
app.use(express.json());

// Basic route
app.get('/', (req, res) => {
  res.send('Hello World!');
});

// Start server
app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

### Express Routes

```javascript
// GET - Retrieve data
app.get('/users', (req, res) => {
  res.json([
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' },
  ]);
});

// GET - Single item
app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
  res.json({ id: userId, name: 'John' });
});

// POST - Create new data
app.post('/users', (req, res) => {
  const newUser = req.body;
  // Save to database...
  res.status(201).json({
    id: 3,
    ...newUser,
    createdAt: new Date(),
  });
});

// PUT - Update existing data
app.put('/users/:id', (req, res) => {
  const userId = req.params.id;
  const updates = req.body;
  // Update in database...
  res.json({
    id: userId,
    ...updates,
    updatedAt: new Date(),
  });
});

// DELETE - Remove data
app.delete('/users/:id', (req, res) => {
  const userId = req.params.id;
  // Delete from database...
  res.status(204).send(); // No content
});
```

### Middleware - Processing Requests

```javascript
// Middleware runs between request and response

// Logger middleware
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next(); // Pass to next middleware
});

// Authentication middleware
const authenticate = (req, res, next) => {
  const token = req.headers.authorization;

  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }

  // Verify token...
  req.user = { id: 1, name: 'John' }; // Add user to request
  next();
};

// Protected route
app.get('/profile', authenticate, (req, res) => {
  res.json({ user: req.user });
});

// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something went wrong!' });
});
```

## 🔌 APIs - Application Programming Interfaces

### What Is an API?

API = Menu at a restaurant

- Lists what's available
- How to order it
- What you'll get back

### RESTful API Design

REST = Rules for designing APIs

```javascript
// RESTful routes for a blog

// GET /posts          - Get all posts
// GET /posts/:id      - Get single post
// POST /posts         - Create new post
// PUT /posts/:id      - Update entire post
// PATCH /posts/:id    - Update part of post
// DELETE /posts/:id   - Delete post

// Example implementation
const posts = []; // In real app, this would be a database

// Get all posts
app.get('/api/posts', (req, res) => {
  res.json(posts);
});

// Get single post
app.get('/api/posts/:id', (req, res) => {
  const post = posts.find((p) => p.id === parseInt(req.params.id));

  if (!post) {
    return res.status(404).json({ error: 'Post not found' });
  }

  res.json(post);
});

// Create post
app.post('/api/posts', (req, res) => {
  const { title, content, author } = req.body;

  // Validation
  if (!title || !content) {
    return res.status(400).json({
      error: 'Title and content are required',
    });
  }

  const newPost = {
    id: posts.length + 1,
    title,
    content,
    author,
    createdAt: new Date(),
    likes: 0,
  };

  posts.push(newPost);
  res.status(201).json(newPost);
});
```

### API Response Standards

```javascript
// Success response
res.json({
  success: true,
  data: {
    id: 1,
    name: 'John',
  },
});

// Error response
res.status(400).json({
  success: false,
  error: {
    code: 'VALIDATION_ERROR',
    message: 'Email is required',
    field: 'email',
  },
});

// Paginated response
res.json({
  success: true,
  data: users,
  pagination: {
    page: 1,
    limit: 10,
    total: 100,
    pages: 10,
  },
});
```

## 🔐 Authentication & Security

### Password Hashing

```javascript
const bcrypt = require('bcrypt');

// Never store plain text passwords!

// Hash password
async function hashPassword(password) {
  const saltRounds = 10;
  return await bcrypt.hash(password, saltRounds);
}

// Verify password
async function verifyPassword(password, hash) {
  return await bcrypt.compare(password, hash);
}

// Usage
app.post('/api/register', async (req, res) => {
  const { email, password } = req.body;

  // Hash password
  const hashedPassword = await hashPassword(password);

  // Save to database
  const user = {
    email,
    password: hashedPassword,
  };

  // In real app, save to database
  res.json({ message: 'User registered successfully' });
});
```

### JWT (JSON Web Tokens)

```javascript
const jwt = require('jsonwebtoken');

const JWT_SECRET = 'your-secret-key'; // In production, use environment variable

// Create token
function createToken(userId) {
  return jwt.sign({ userId }, JWT_SECRET, { expiresIn: '24h' });
}

// Verify token
function verifyToken(token) {
  try {
    return jwt.verify(token, JWT_SECRET);
  } catch (err) {
    return null;
  }
}

// Login endpoint
app.post('/api/login', async (req, res) => {
  const { email, password } = req.body;

  // Find user (in real app, from database)
  const user = users.find((u) => u.email === email);

  if (!user) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }

  // Verify password
  const validPassword = await verifyPassword(password, user.password);

  if (!validPassword) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }

  // Create token
  const token = createToken(user.id);

  res.json({
    token,
    user: {
      id: user.id,
      email: user.email,
    },
  });
});

// Protected route middleware
function authMiddleware(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1]; // "Bearer TOKEN"

  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }

  const decoded = verifyToken(token);

  if (!decoded) {
    return res.status(401).json({ error: 'Invalid token' });
  }

  req.userId = decoded.userId;
  next();
}

// Protected route
app.get('/api/profile', authMiddleware, (req, res) => {
  const user = users.find((u) => u.id === req.userId);
  res.json({ user });
});
```

## 🏗️ Building a Complete Backend

Let's build a simple blog API:

```javascript
const express = require('express');
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');

const app = express();
app.use(express.json());

// In-memory database (use real database in production)
const users = [];
const posts = [];

// Register
app.post('/api/auth/register', async (req, res) => {
  try {
    const { email, password, name } = req.body;

    // Check if user exists
    if (users.find((u) => u.email === email)) {
      return res.status(400).json({ error: 'User already exists' });
    }

    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10);

    // Create user
    const user = {
      id: users.length + 1,
      email,
      password: hashedPassword,
      name,
      createdAt: new Date(),
    };

    users.push(user);

    // Create token
    const token = jwt.sign({ userId: user.id }, 'secret', { expiresIn: '7d' });

    res.status(201).json({
      token,
      user: {
        id: user.id,
        email: user.email,
        name: user.name,
      },
    });
  } catch (error) {
    res.status(500).json({ error: 'Server error' });
  }
});

// Login
app.post('/api/auth/login', async (req, res) => {
  try {
    const { email, password } = req.body;

    // Find user
    const user = users.find((u) => u.email === email);
    if (!user) {
      return res.status(401).json({ error: 'Invalid credentials' });
    }

    // Check password
    const validPassword = await bcrypt.compare(password, user.password);
    if (!validPassword) {
      return res.status(401).json({ error: 'Invalid credentials' });
    }

    // Create token
    const token = jwt.sign({ userId: user.id }, 'secret', { expiresIn: '7d' });

    res.json({
      token,
      user: {
        id: user.id,
        email: user.email,
        name: user.name,
      },
    });
  } catch (error) {
    res.status(500).json({ error: 'Server error' });
  }
});

// Auth middleware
const auth = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];

  if (!token) {
    return res.status(401).json({ error: 'Access denied' });
  }

  try {
    const decoded = jwt.verify(token, 'secret');
    req.userId = decoded.userId;
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
};

// Create post
app.post('/api/posts', auth, (req, res) => {
  const { title, content } = req.body;

  const post = {
    id: posts.length + 1,
    title,
    content,
    authorId: req.userId,
    createdAt: new Date(),
    likes: 0,
  };

  posts.push(post);
  res.status(201).json(post);
});

// Get all posts
app.get('/api/posts', (req, res) => {
  const postsWithAuthors = posts.map((post) => {
    const author = users.find((u) => u.id === post.authorId);
    return {
      ...post,
      author: {
        id: author.id,
        name: author.name,
      },
    };
  });

  res.json(postsWithAuthors);
});

// Like post
app.post('/api/posts/:id/like', auth, (req, res) => {
  const post = posts.find((p) => p.id === parseInt(req.params.id));

  if (!post) {
    return res.status(404).json({ error: 'Post not found' });
  }

  post.likes += 1;
  res.json({ likes: post.likes });
});

// Start server
app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

## 🔄 REST vs GraphQL

### REST API

```javascript
// Multiple endpoints
GET / api / users / 1;
GET / api / users / 1 / posts;
GET / api / users / 1 / comments;

// Fixed response structure
// May over-fetch or under-fetch data
```

### GraphQL

```graphql
# Single endpoint, flexible queries
query {
  user(id: 1) {
    name
    email
    posts {
      title
      comments {
        text
      }
    }
  }
}

# Get exactly what you need
```

### GraphQL Server Example

```javascript
const { ApolloServer, gql } = require('apollo-server-express');

// Schema
const typeDefs = gql`
  type User {
    id: ID!
    name: String!
    email: String!
    posts: [Post!]!
  }

  type Post {
    id: ID!
    title: String!
    content: String!
    author: User!
  }

  type Query {
    users: [User!]!
    user(id: ID!): User
    posts: [Post!]!
  }

  type Mutation {
    createUser(name: String!, email: String!): User!
    createPost(title: String!, content: String!, authorId: ID!): Post!
  }
`;

// Resolvers
const resolvers = {
  Query: {
    users: () => users,
    user: (_, { id }) => users.find((u) => u.id === id),
    posts: () => posts,
  },
  Mutation: {
    createUser: (_, { name, email }) => {
      const user = { id: String(users.length + 1), name, email };
      users.push(user);
      return user;
    },
    createPost: (_, { title, content, authorId }) => {
      const post = {
        id: String(posts.length + 1),
        title,
        content,
        authorId,
      };
      posts.push(post);
      return post;
    },
  },
  User: {
    posts: (user) => posts.filter((p) => p.authorId === user.id),
  },
  Post: {
    author: (post) => users.find((u) => u.id === post.authorId),
  },
};

// Setup
const server = new ApolloServer({ typeDefs, resolvers });
```

## 🚀 Advanced Backend Concepts

### File Uploads

```javascript
const multer = require('multer');
const path = require('path');

// Configure storage
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    const uniqueName = Date.now() + '-' + file.originalname;
    cb(null, uniqueName);
  },
});

// File filter
const fileFilter = (req, file, cb) => {
  const allowedTypes = /jpeg|jpg|png|gif/;
  const extname = allowedTypes.test(
    path.extname(file.originalname).toLowerCase(),
  );

  if (extname) {
    cb(null, true);
  } else {
    cb(new Error('Only images allowed'));
  }
};

const upload = multer({
  storage,
  fileFilter,
  limits: { fileSize: 5 * 1024 * 1024 }, // 5MB limit
});

// Upload endpoint
app.post('/api/upload', upload.single('image'), (req, res) => {
  if (!req.file) {
    return res.status(400).json({ error: 'No file uploaded' });
  }

  res.json({
    filename: req.file.filename,
    size: req.file.size,
    url: `/uploads/${req.file.filename}`,
  });
});
```

### WebSockets with Socket.io

```javascript
const { Server } = require('socket.io');
const io = new Server(server);

// Connection
io.on('connection', (socket) => {
  console.log('User connected:', socket.id);

  // Join room
  socket.on('join-room', (roomId) => {
    socket.join(roomId);
    socket.to(roomId).emit('user-joined', socket.id);
  });

  // Chat message
  socket.on('chat-message', (data) => {
    io.to(data.room).emit('message', {
      user: socket.id,
      text: data.text,
      timestamp: new Date(),
    });
  });

  // Disconnect
  socket.on('disconnect', () => {
    console.log('User disconnected:', socket.id);
  });
});
```

### Background Jobs

```javascript
const Queue = require('bull');
const emailQueue = new Queue('email');

// Process jobs
emailQueue.process(async (job) => {
  const { to, subject, body } = job.data;

  // Send email
  await sendEmail(to, subject, body);

  return { sent: true };
});

// Add job to queue
app.post('/api/send-welcome-email', async (req, res) => {
  const { userId } = req.body;

  await emailQueue.add({
    to: user.email,
    subject: 'Welcome!',
    body: 'Thanks for joining...',
  });

  res.json({ message: 'Email queued' });
});
```

## 🎯 What This Means for BAs

Understanding backend helps you:

1. **API Requirements** - Know what endpoints are needed
2. **Data Flow** - Understand how data moves through system
3. **Security Needs** - Identify authentication requirements
4. **Performance** - Recognize what operations are expensive
5. **Integration Points** - See how systems connect

## ✅ Key Takeaways

- Backend handles business logic and data
- Node.js runs JavaScript on servers
- Express makes building APIs easy
- REST and GraphQL are API styles
- Authentication protects routes
- Middleware processes requests
- Always hash passwords
- Use tokens for authentication

Next Module: [Databases →](../07-Databases/README.md)
