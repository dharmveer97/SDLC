# 💾 Databases - SQL, NoSQL, and Data Storage

## 🎯 What Is a Database?

Think of a database like:
- **Filing Cabinet** = Database
- **Drawers** = Tables/Collections
- **Folders** = Records/Documents
- **Papers** = Data Fields

Databases store information in an organized way so you can find it quickly.

## 📊 Types of Databases

### SQL (Relational) Databases
Like Excel spreadsheets with relationships:
- Data in tables with rows and columns
- Tables can reference each other
- Examples: PostgreSQL, MySQL, SQLite

### NoSQL (Non-Relational) Databases
Like flexible document storage:
- Data in various formats
- More flexible structure
- Examples: MongoDB, DynamoDB, Redis

## 🗃️ SQL Databases

### Understanding Tables

Imagine a table for users:

| id | name    | email           | age | city     |
|----|---------|-----------------|-----|----------|
| 1  | John    | john@email.com  | 25  | New York |
| 2  | Jane    | jane@email.com  | 30  | Boston   |
| 3  | Bob     | bob@email.com   | 28  | Chicago  |

### SQL Commands

```sql
-- Create a table
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  age INTEGER,
  city VARCHAR(100),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert data
INSERT INTO users (name, email, age, city) 
VALUES ('John', 'john@email.com', 25, 'New York');

-- Select (read) data
SELECT * FROM users;                          -- Get all users
SELECT name, email FROM users;                -- Get specific columns
SELECT * FROM users WHERE age > 25;           -- Filter results
SELECT * FROM users WHERE city = 'Boston';    -- Exact match

-- Update data
UPDATE users 
SET age = 26 
WHERE id = 1;

-- Delete data
DELETE FROM users WHERE id = 3;
```

### Relationships in SQL

```sql
-- Users table
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(255)
);

-- Posts table (references users)
CREATE TABLE posts (
  id INTEGER PRIMARY KEY,
  title VARCHAR(200),
  content TEXT,
  user_id INTEGER,
  FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Join tables to get related data
SELECT 
  posts.title,
  posts.content,
  users.name as author
FROM posts
JOIN users ON posts.user_id = users.id;
```

### Common SQL Queries

```sql
-- Sorting
SELECT * FROM users ORDER BY age DESC;        -- Oldest first
SELECT * FROM users ORDER BY name ASC;        -- Alphabetical

-- Limiting results
SELECT * FROM users LIMIT 10;                 -- First 10 users
SELECT * FROM users LIMIT 10 OFFSET 20;       -- Skip 20, take 10

-- Counting
SELECT COUNT(*) FROM users;                   -- Total users
SELECT COUNT(*) FROM users WHERE age > 25;    -- Users over 25

-- Grouping
SELECT city, COUNT(*) as user_count 
FROM users 
GROUP BY city;                               -- Users per city

-- Aggregate functions
SELECT AVG(age) FROM users;                  -- Average age
SELECT MAX(age) FROM users;                  -- Oldest user
SELECT MIN(age) FROM users;                  -- Youngest user
SELECT SUM(age) FROM users;                  -- Total of all ages
```

## 🍃 MongoDB (NoSQL)

### Document Structure

Instead of tables, MongoDB uses collections of documents:

```javascript
// User document
{
  "_id": "507f1f77bcf86cd799439011",
  "name": "John Doe",
  "email": "john@email.com",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "zip": "10001"
  },
  "hobbies": ["reading", "gaming", "cooking"],
  "createdAt": "2024-01-15T10:30:00Z"
}
```

### MongoDB with Mongoose (Node.js)

```javascript
const mongoose = require('mongoose');

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/myapp');

// Define a schema
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true
  },
  age: {
    type: Number,
    min: 0,
    max: 120
  },
  address: {
    street: String,
    city: String,
    zip: String
  },
  hobbies: [String],
  createdAt: {
    type: Date,
    default: Date.now
  }
});

// Create model
const User = mongoose.model('User', userSchema);

// Create user
const newUser = new User({
  name: 'John Doe',
  email: 'john@email.com',
  age: 25,
  address: {
    city: 'New York'
  },
  hobbies: ['reading', 'gaming']
});

// Save to database
await newUser.save();

// Find users
const users = await User.find();                    // All users
const adults = await User.find({ age: { $gte: 18 } }); // Age >= 18
const johnDoe = await User.findOne({ name: 'John Doe' });

// Update user
await User.updateOne(
  { _id: userId },
  { $set: { age: 26 } }
);

// Delete user
await User.deleteOne({ _id: userId });
```

### MongoDB Queries

```javascript
// Comparison operators
User.find({ age: { $gt: 25 } });      // Greater than
User.find({ age: { $gte: 25 } });     // Greater than or equal
User.find({ age: { $lt: 25 } });      // Less than
User.find({ age: { $lte: 25 } });     // Less than or equal
User.find({ age: { $ne: 25 } });      // Not equal

// Logical operators
User.find({
  $or: [
    { age: { $lt: 18 } },
    { age: { $gt: 65 } }
  ]
});  // Young OR old

User.find({
  $and: [
    { city: 'New York' },
    { age: { $gte: 21 } }
  ]
});  // New York AND adult

// Array queries
User.find({ hobbies: 'gaming' });              // Has gaming hobby
User.find({ hobbies: { $in: ['gaming', 'reading'] } }); // Has any
User.find({ 'address.city': 'New York' });     // Nested field

// Sorting and limiting
User.find()
  .sort({ age: -1 })     // Sort by age descending
  .limit(10)             // Take first 10
  .skip(20);             // Skip first 20
```

## 🔄 SQL vs NoSQL Comparison

### SQL Example: E-commerce

```sql
-- Users table
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  email VARCHAR(255),
  name VARCHAR(100)
);

-- Products table
CREATE TABLE products (
  id INTEGER PRIMARY KEY,
  name VARCHAR(200),
  price DECIMAL(10,2),
  stock INTEGER
);

-- Orders table
CREATE TABLE orders (
  id INTEGER PRIMARY KEY,
  user_id INTEGER,
  total DECIMAL(10,2),
  status VARCHAR(50),
  FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Order items table
CREATE TABLE order_items (
  id INTEGER PRIMARY KEY,
  order_id INTEGER,
  product_id INTEGER,
  quantity INTEGER,
  price DECIMAL(10,2),
  FOREIGN KEY (order_id) REFERENCES orders(id),
  FOREIGN KEY (product_id) REFERENCES products(id)
);
```

### NoSQL Example: Same E-commerce

```javascript
// Order document (everything in one place)
{
  "_id": "order123",
  "user": {
    "id": "user456",
    "email": "john@email.com",
    "name": "John Doe"
  },
  "items": [
    {
      "productId": "prod789",
      "name": "Laptop",
      "price": 999.99,
      "quantity": 1
    },
    {
      "productId": "prod790",
      "name": "Mouse",
      "price": 29.99,
      "quantity": 2
    }
  ],
  "total": 1059.97,
  "status": "shipped",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

## 📈 When to Use What?

### Use SQL When:
- Data has clear relationships
- You need ACID compliance (transactions)
- Complex queries and reporting
- Data structure is well-defined
- Examples: Banking, inventory, accounting

### Use NoSQL When:
- Data structure varies
- Need to scale horizontally
- Working with big data
- Rapid development
- Examples: Social media, content management, real-time analytics

## 🔍 Database Indexing

Indexes are like a book's index - they help find data faster.

### SQL Index
```sql
-- Create index on email for faster lookups
CREATE INDEX idx_users_email ON users(email);

-- Create composite index
CREATE INDEX idx_users_city_age ON users(city, age);

-- Unique index
CREATE UNIQUE INDEX idx_users_username ON users(username);
```

### MongoDB Index
```javascript
// Single field index
db.users.createIndex({ email: 1 });

// Compound index
db.users.createIndex({ city: 1, age: -1 });

// Text index for search
db.products.createIndex({ name: "text", description: "text" });

// Search using text index
db.products.find({ $text: { $search: "laptop" } });
```

## 🏗️ Database Design Best Practices

### 1. Normalization (SQL)
Don't repeat data:

```sql
-- Bad: Repeating data
CREATE TABLE orders (
  id INTEGER,
  product_name VARCHAR(200),  -- Repeated
  product_price DECIMAL,      -- Repeated
  customer_name VARCHAR(100), -- Repeated
  customer_email VARCHAR(255) -- Repeated
);

-- Good: Separate tables
CREATE TABLE products (
  id INTEGER PRIMARY KEY,
  name VARCHAR(200),
  price DECIMAL
);

CREATE TABLE customers (
  id INTEGER PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(255)
);

CREATE TABLE orders (
  id INTEGER PRIMARY KEY,
  product_id INTEGER,
  customer_id INTEGER,
  quantity INTEGER,
  FOREIGN KEY (product_id) REFERENCES products(id),
  FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

### 2. Schema Design (MongoDB)

```javascript
// Embedding vs Referencing

// Embedding (good for 1:1 or 1:few)
{
  "_id": "user123",
  "name": "John",
  "address": {  // Embedded
    "street": "123 Main St",
    "city": "Boston"
  }
}

// Referencing (good for 1:many)
// User document
{
  "_id": "user123",
  "name": "John"
}

// Post documents
{
  "_id": "post456",
  "userId": "user123",  // Reference
  "title": "My Post",
  "content": "..."
}
```

## 💾 Other Database Types

### Redis (Key-Value Store)
```javascript
// Super fast for caching
redis.set('user:123', JSON.stringify(userData));
const user = JSON.parse(await redis.get('user:123'));

// Expire after 1 hour
redis.setex('session:abc', 3600, sessionData);
```

### Elasticsearch (Search Database)
```javascript
// Full-text search
{
  "query": {
    "match": {
      "description": "wireless mouse"
    }
  }
}
```

### DynamoDB (AWS NoSQL)
```javascript
// Key-value and document database
const params = {
  TableName: 'Users',
  Key: {
    'userId': { S: '123' }
  }
};
dynamodb.getItem(params);
```

## 🔒 Database Security

### SQL Security
```sql
-- Use parameterized queries (prevent SQL injection)
-- Bad (vulnerable):
SELECT * FROM users WHERE email = '" + userInput + "'";

-- Good (safe):
SELECT * FROM users WHERE email = ?;
```

### MongoDB Security
```javascript
// Never build queries with string concatenation
// Bad:
db.users.find({ email: userInput });  // If userInput is an object

// Good:
db.users.find({ email: String(userInput) });
```

## 🚀 Real-World Example: Blog Database

### SQL Version
```sql
-- Users
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  username VARCHAR(50) UNIQUE NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Posts
CREATE TABLE posts (
  id SERIAL PRIMARY KEY,
  user_id INTEGER NOT NULL,
  title VARCHAR(200) NOT NULL,
  slug VARCHAR(200) UNIQUE NOT NULL,
  content TEXT,
  published BOOLEAN DEFAULT false,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Comments
CREATE TABLE comments (
  id SERIAL PRIMARY KEY,
  post_id INTEGER NOT NULL,
  user_id INTEGER NOT NULL,
  content TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY (post_id) REFERENCES posts(id),
  FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Tags
CREATE TABLE tags (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50) UNIQUE NOT NULL
);

-- Post tags (many-to-many)
CREATE TABLE post_tags (
  post_id INTEGER NOT NULL,
  tag_id INTEGER NOT NULL,
  PRIMARY KEY (post_id, tag_id),
  FOREIGN KEY (post_id) REFERENCES posts(id),
  FOREIGN KEY (tag_id) REFERENCES tags(id)
);
```

### MongoDB Version
```javascript
// User schema
const userSchema = {
  username: String,
  email: String,
  passwordHash: String,
  createdAt: Date
};

// Post schema
const postSchema = {
  userId: ObjectId,
  title: String,
  slug: String,
  content: String,
  published: Boolean,
  tags: [String],
  comments: [{
    userId: ObjectId,
    content: String,
    createdAt: Date
  }],
  createdAt: Date,
  updatedAt: Date
};
```

## 🎯 What This Means for BAs

Understanding databases helps you:

1. **Data Requirements** - Know what data to store
2. **Relationships** - Understand how data connects
3. **Performance** - Recognize expensive operations
4. **Reporting** - Know what queries are possible
5. **Data Integrity** - Ensure data quality rules

## ✅ Key Takeaways

- Databases organize and store data
- SQL uses tables with relationships
- NoSQL uses flexible documents
- Choose based on data structure and needs
- Indexes make queries faster
- Security is crucial (prevent injection)
- Design affects performance

Next Module: [AWS & Cloud Services →](../08-AWS-Cloud-Services/README.md)