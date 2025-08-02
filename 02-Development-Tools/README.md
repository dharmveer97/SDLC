# 🛠️ Development Tools - VS Code, ESLint, and More

## 🎯 What Are Development Tools?

Think of development tools like a chef's kitchen equipment:
- **VS Code** = Your main workspace (like a kitchen counter)
- **ESLint** = Quality checker (like a food safety inspector)
- **Prettier** = Formatter (like a plate decorator)
- **npm/yarn** = Ingredient manager (like a pantry organizer)

## 📝 VS Code (Visual Studio Code)

### What Is VS Code?
VS Code is where developers write code. It's like Microsoft Word, but for programming. It's:
- Free and open source
- Used by millions of developers
- Works with every programming language
- Highly customizable

### Why VS Code?
Before VS Code, developers used simple text editors. VS Code is special because it:
- Highlights code in colors
- Auto-completes your code
- Shows errors before you run the program
- Has thousands of helpful extensions

### VS Code Interface Explained

```
┌─────────────────────────────────────────────────────┐
│ File  Edit  View  Go  Run  Terminal  Help          │ ← Menu Bar
├──────┬──────────────────────────────────┬──────────┤
│      │                                  │          │
│  📁  │     Code Editor                  │   🔍     │
│      │                                  │          │
│ Side │     Where you write code         │ Minimap  │
│ Bar  │                                  │          │
│      │                                  │          │
├──────┴──────────────────────────────────┴──────────┤
│ Terminal / Problems / Output / Debug Console        │ ← Bottom Panel
└─────────────────────────────────────────────────────┘
```

### Essential VS Code Features

#### 1. **File Explorer** (Ctrl/Cmd + Shift + E)
Shows all your project files, like Windows Explorer or Mac Finder.

#### 2. **Search** (Ctrl/Cmd + Shift + F)
Find any text across all files in your project.

#### 3. **Terminal** (Ctrl/Cmd + `)
Run commands without leaving VS Code.

#### 4. **Command Palette** (Ctrl/Cmd + Shift + P)
The "Google search" of VS Code - type what you want to do!

### Must-Have VS Code Extensions

#### 1. **Prettier - Code Formatter**
Automatically formats your code to look nice.
```javascript
// Before Prettier
function messy(  a,b,   c){return a+b   +c}

// After Prettier
function messy(a, b, c) {
  return a + b + c;
}
```

#### 2. **ESLint**
Finds problems in your JavaScript code.

#### 3. **Live Server**
Preview your website instantly as you code.

#### 4. **GitLens**
See who wrote each line of code and when.

#### 5. **Auto Rename Tag**
When you change an HTML tag, it updates the closing tag too.

### VS Code Shortcuts Every Developer Uses

```
Ctrl/Cmd + S         → Save file
Ctrl/Cmd + /         → Comment/uncomment code
Ctrl/Cmd + D         → Select next occurrence
Ctrl/Cmd + F         → Find in file
Ctrl/Cmd + Shift + F → Find in all files
Alt + Up/Down        → Move line up/down
Ctrl/Cmd + Z         → Undo
Ctrl/Cmd + Shift + Z → Redo
```

## 🔍 ESLint - The Code Quality Checker

### What Is ESLint?
ESLint is like a spelling and grammar checker for code. It:
- Finds errors before you run the code
- Enforces coding standards
- Makes code consistent across the team

### Common ESLint Rules

```javascript
// ESLint catches these problems:

// 1. Unused variables
const userName = "John"; // ⚠️ 'userName' is declared but never used

// 2. Missing semicolons (if required)
console.log("Hello") // ⚠️ Missing semicolon

// 3. Console statements in production
console.log("Debug info"); // ⚠️ Unexpected console statement

// 4. Undefined variables
userAge = 25; // ❌ 'userAge' is not defined
```

### Setting Up ESLint

```bash
# Install ESLint
npm install --save-dev eslint

# Set it up
npx eslint --init

# Run ESLint
npx eslint yourfile.js
```

### ESLint Configuration Example

```json
{
  "rules": {
    "no-console": "warn",        // Warn about console.log
    "no-unused-vars": "error",   // Error for unused variables
    "semi": ["error", "always"], // Require semicolons
    "quotes": ["error", "double"] // Use double quotes
  }
}
```

## 🎨 Prettier - The Code Formatter

### What Is Prettier?
Prettier automatically formats your code to look consistent. No more debates about:
- Tabs vs spaces
- Where to put brackets
- Line length

### Prettier in Action

```javascript
// You write:
const data={name:"John",age:30,city:"New York"}

// Prettier formats to:
const data = {
  name: "John",
  age: 30,
  city: "New York"
};
```

## 📦 npm (Node Package Manager)

### What Is npm?
npm is like an app store for code. Instead of reinventing the wheel, developers download pre-made code packages.

### Understanding package.json
This file is like a shopping list for your project:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "My awesome project",
  "scripts": {
    "start": "node server.js",
    "test": "jest",
    "build": "webpack"
  },
  "dependencies": {
    "react": "^18.0.0",    // Packages your app needs to run
    "axios": "^1.0.0"
  },
  "devDependencies": {
    "eslint": "^8.0.0",    // Packages only for development
    "jest": "^29.0.0"
  }
}
```

### Common npm Commands

```bash
# Install all packages
npm install

# Install a specific package
npm install react

# Install dev-only package
npm install --save-dev eslint

# Run a script
npm run start
npm run test
npm run build

# Update packages
npm update
```

## 🔧 Other Essential Tools

### 1. **Git Integration in VS Code**
See changes right in the editor:
- Green lines = Added code
- Blue lines = Modified code
- Red arrows = Deleted code

### 2. **Debugger**
Instead of using console.log everywhere, use breakpoints:
```javascript
function calculateTotal(items) {
  let total = 0;           // Set breakpoint here
  for (let item of items) {
    total += item.price;   // Step through code
  }
  return total;
}
```

### 3. **Extensions for Specific Languages**

#### For React Development:
- ES7+ React/Redux/React-Native snippets
- Simple React Snippets

#### For Python:
- Python extension by Microsoft
- Pylint

#### For General Web Development:
- Live Server
- Path Intellisense
- CSS Peek

## 🏗️ Setting Up a Development Environment

### Step 1: Install VS Code
Download from: https://code.visualstudio.com/

### Step 2: Install Node.js
This gives you npm. Download from: https://nodejs.org/

### Step 3: Install Essential Extensions
1. Open VS Code
2. Click Extensions icon (or Ctrl+Shift+X)
3. Search and install:
   - Prettier
   - ESLint
   - GitLens
   - Live Server

### Step 4: Configure Settings
```json
// VS Code settings.json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "editor.fontSize": 14,
  "editor.tabSize": 2,
  "editor.wordWrap": "on"
}
```

## 💡 Developer Workflow Example

Here's how a developer uses these tools together:

```bash
# 1. Open project in VS Code
code my-project

# 2. Install dependencies
npm install

# 3. Start development server
npm run dev

# 4. Write code in VS Code
# - ESLint shows errors as you type
# - Prettier formats on save
# - Git shows what changed

# 5. Test your code
npm test

# 6. Commit changes
git add .
git commit -m "Add new feature"

# 7. Push to GitHub
git push
```

## 🚀 Pro Tips

### 1. **Use Snippets**
Type shortcuts that expand to full code:
```javascript
// Type "rafce" + Tab in a React file:
import React from 'react'

const ComponentName = () => {
  return (
    <div>
      
    </div>
  )
}

export default ComponentName
```

### 2. **Multi-Cursor Editing**
Hold Alt and click to edit multiple places at once!

### 3. **Emmet for HTML**
```html
<!-- Type this: -->
div.container>ul>li*5

<!-- Hit Tab to get: -->
<div class="container">
  <ul>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
  </ul>
</div>
```

## 🎯 Why These Tools Matter for BAs

Understanding development tools helps you:
1. **Read code** when reviewing implementations
2. **Understand errors** developers mention
3. **Know what's possible** in terms of automation
4. **Communicate better** using developer terminology
5. **Review pull requests** more effectively

## ✅ Key Takeaways

- VS Code is the developer's main workspace
- ESLint catches errors before they happen
- Prettier makes code look consistent
- npm manages project dependencies
- Extensions make developers more productive
- Everything is customizable to team preferences

Next Module: [Frontend Basics →](../03-Frontend-Basics/README.md)