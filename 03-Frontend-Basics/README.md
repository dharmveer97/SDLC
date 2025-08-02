# 🌐 Frontend Basics - HTML, CSS, and Web Fundamentals

## 🎯 What Is Frontend Development?

Frontend = Everything users see and interact with on a website

Think of building a website like building a house:
- **HTML** = Structure (walls, rooms, doors)
- **CSS** = Design (paint, decorations, style)
- **JavaScript** = Functionality (electricity, plumbing, doorbell)

## 📄 HTML - The Structure

### What Is HTML?
HTML (HyperText Markup Language) is the skeleton of every website. It defines:
- What content appears
- How content is organized
- The meaning of each piece of content

### Basic HTML Structure

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Page Title (shown in browser tab)</title>
  </head>
  <body>
    <h1>This is a heading</h1>
    <p>This is a paragraph of text.</p>
  </body>
</html>
```

### Common HTML Elements

```html
<!-- Headings (h1 is biggest, h6 is smallest) -->
<h1>Main Title</h1>
<h2>Section Title</h2>
<h3>Subsection</h3>

<!-- Text -->
<p>This is a paragraph</p>
<span>This is inline text</span>
<strong>This is bold text</strong>
<em>This is italic text</em>

<!-- Links -->
<a href="https://google.com">Click me!</a>

<!-- Images -->
<img src="photo.jpg" alt="Description of photo">

<!-- Lists -->
<ul>
  <li>Bullet point 1</li>
  <li>Bullet point 2</li>
</ul>

<ol>
  <li>Numbered item 1</li>
  <li>Numbered item 2</li>
</ol>

<!-- Containers -->
<div>A box that takes full width</div>
<span>Inline container</span>

<!-- Forms -->
<form>
  <input type="text" placeholder="Enter your name">
  <input type="email" placeholder="Enter email">
  <input type="password" placeholder="Enter password">
  <button>Submit</button>
</form>

<!-- Tables -->
<table>
  <tr>
    <th>Name</th>
    <th>Age</th>
  </tr>
  <tr>
    <td>John</td>
    <td>30</td>
  </tr>
</table>
```

### HTML Attributes
Attributes provide extra information about elements:

```html
<img src="photo.jpg" alt="Beach sunset" width="300" height="200">
<a href="https://google.com" target="_blank">Opens in new tab</a>
<input type="text" id="username" class="form-input" required>
<div id="unique-id" class="reusable-class" data-user="123">Content</div>
```

## 🎨 CSS - The Style

### What Is CSS?
CSS (Cascading Style Sheets) makes websites look good. It controls:
- Colors and backgrounds
- Fonts and text size
- Spacing and layout
- Animations and effects

### How to Add CSS

#### 1. Inline CSS (directly on element)
```html
<p style="color: blue; font-size: 20px;">Blue text</p>
```

#### 2. Internal CSS (in HTML head)
```html
<head>
  <style>
    p {
      color: blue;
      font-size: 20px;
    }
  </style>
</head>
```

#### 3. External CSS (separate file) - BEST PRACTICE
```html
<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

### CSS Syntax

```css
/* This is a CSS comment */

/* Selector { property: value; } */
p {
  color: blue;
  font-size: 16px;
  margin: 10px;
}

/* Select by class */
.warning {
  color: red;
  background-color: yellow;
}

/* Select by ID */
#header {
  background-color: navy;
  color: white;
}

/* Select multiple */
h1, h2, h3 {
  font-family: Arial, sans-serif;
}
```

### Common CSS Properties

```css
/* Text Styling */
.text-styling {
  color: #333333;                  /* Text color */
  font-size: 16px;                 /* Text size */
  font-weight: bold;               /* Bold text */
  font-family: Arial, sans-serif;  /* Font type */
  text-align: center;              /* Alignment */
  text-decoration: underline;      /* Underline */
  line-height: 1.5;                /* Space between lines */
}

/* Box Model */
.box {
  width: 300px;                    /* Width */
  height: 200px;                   /* Height */
  padding: 20px;                   /* Space inside */
  margin: 10px;                    /* Space outside */
  border: 2px solid black;         /* Border */
  border-radius: 10px;             /* Rounded corners */
}

/* Background */
.background {
  background-color: lightblue;     /* Color */
  background-image: url('bg.jpg'); /* Image */
  background-size: cover;          /* Image sizing */
}

/* Display and Position */
.layout {
  display: block;                  /* Block element */
  display: inline;                 /* Inline element */
  display: flex;                   /* Flexbox layout */
  position: relative;              /* Positioning */
  position: absolute;              /* Absolute positioning */
}
```

### CSS Box Model Explained

Every HTML element is a box with:

```
┌─────────────────────────────────┐
│          MARGIN                 │
│  ┌───────────────────────────┐  │
│  │       BORDER              │  │
│  │  ┌─────────────────────┐  │  │
│  │  │    PADDING          │  │  │
│  │  │  ┌───────────────┐  │  │  │
│  │  │  │   CONTENT     │  │  │  │
│  │  │  │               │  │  │  │
│  │  │  └───────────────┘  │  │  │
│  │  └─────────────────────┘  │  │
│  └───────────────────────────┘  │
└─────────────────────────────────┘
```

## 🔤 Google Fonts - Making Text Beautiful

### How to Use Google Fonts

1. **Visit Google Fonts**
   Go to: https://fonts.google.com/

2. **Choose a Font**
   - Browse fonts
   - Click on one you like
   - Select the styles you need

3. **Add to Your Project**

```html
<!-- Add this to your HTML head -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap" rel="stylesheet">
```

4. **Use in CSS**

```css
body {
  font-family: 'Roboto', sans-serif;
}

h1 {
  font-weight: 700; /* Bold */
}

p {
  font-weight: 300; /* Light */
}
```

### Downloading Fonts for Offline Use

Sometimes you need fonts to work offline:

1. **Download from Google Fonts**
   - Select your font
   - Click download icon
   - Extract the ZIP file

2. **Add to Your Project**

```
project/
  fonts/
    Roboto-Regular.ttf
    Roboto-Bold.ttf
  css/
    styles.css
```

3. **Use @font-face**

```css
@font-face {
  font-family: 'Roboto';
  src: url('../fonts/Roboto-Regular.ttf');
  font-weight: 400;
}

@font-face {
  font-family: 'Roboto';
  src: url('../fonts/Roboto-Bold.ttf');
  font-weight: 700;
}

body {
  font-family: 'Roboto', sans-serif;
}
```

## 📱 Responsive Design

Websites must work on all devices (phones, tablets, desktops).

### Media Queries

```css
/* Default styles (mobile first) */
.container {
  width: 100%;
  padding: 10px;
}

/* Tablet styles */
@media (min-width: 768px) {
  .container {
    width: 750px;
    padding: 20px;
  }
}

/* Desktop styles */
@media (min-width: 1024px) {
  .container {
    width: 1000px;
    padding: 30px;
  }
}
```

### Flexbox - Modern Layout

```css
/* Container */
.flex-container {
  display: flex;
  justify-content: space-between; /* Horizontal alignment */
  align-items: center;            /* Vertical alignment */
  flex-wrap: wrap;                /* Allow wrapping */
}

/* Items */
.flex-item {
  flex: 1;                        /* Equal width */
  margin: 10px;
}
```

### Grid - Advanced Layout

```css
.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 equal columns */
  gap: 20px;                             /* Space between items */
}
```

## 🏗️ Building a Simple Web Page

Let's build a complete example:

### HTML (index.html)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Business Website</title>
  <link rel="stylesheet" href="styles.css">
  <link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;700&display=swap" rel="stylesheet">
</head>
<body>
  <!-- Navigation -->
  <nav class="navbar">
    <div class="container">
      <h1 class="logo">MyBusiness</h1>
      <ul class="nav-links">
        <li><a href="#home">Home</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#services">Services</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </div>
  </nav>

  <!-- Hero Section -->
  <section class="hero">
    <div class="container">
      <h2>Welcome to Our Business</h2>
      <p>We provide amazing services for your needs</p>
      <button class="cta-button">Get Started</button>
    </div>
  </section>

  <!-- Services -->
  <section class="services">
    <div class="container">
      <h2>Our Services</h2>
      <div class="service-grid">
        <div class="service-card">
          <h3>Web Design</h3>
          <p>Beautiful, responsive websites</p>
        </div>
        <div class="service-card">
          <h3>Development</h3>
          <p>Custom web applications</p>
        </div>
        <div class="service-card">
          <h3>SEO</h3>
          <p>Improve your search rankings</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="footer">
    <div class="container">
      <p>&copy; 2024 MyBusiness. All rights reserved.</p>
    </div>
  </footer>
</body>
</html>
```

### CSS (styles.css)

```css
/* Reset default styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* General styles */
body {
  font-family: 'Open Sans', sans-serif;
  line-height: 1.6;
  color: #333;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Navigation */
.navbar {
  background-color: #333;
  color: white;
  padding: 1rem 0;
}

.navbar .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 1.5rem;
}

.nav-links {
  display: flex;
  list-style: none;
  gap: 2rem;
}

.nav-links a {
  color: white;
  text-decoration: none;
  transition: color 0.3s;
}

.nav-links a:hover {
  color: #4CAF50;
}

/* Hero Section */
.hero {
  background-color: #f4f4f4;
  text-align: center;
  padding: 4rem 0;
}

.hero h2 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
}

.cta-button {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 12px 30px;
  font-size: 1rem;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.cta-button:hover {
  background-color: #45a049;
}

/* Services */
.services {
  padding: 4rem 0;
}

.services h2 {
  text-align: center;
  font-size: 2rem;
  margin-bottom: 2rem;
}

.service-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
}

.service-card {
  background-color: #f9f9f9;
  padding: 2rem;
  border-radius: 8px;
  text-align: center;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

/* Footer */
.footer {
  background-color: #333;
  color: white;
  text-align: center;
  padding: 1rem 0;
}

/* Responsive */
@media (max-width: 768px) {
  .navbar .container {
    flex-direction: column;
    gap: 1rem;
  }
  
  .nav-links {
    flex-direction: column;
    text-align: center;
    gap: 1rem;
  }
  
  .hero h2 {
    font-size: 2rem;
  }
}
```

## 🚀 Best Practices

### HTML Best Practices

1. **Use Semantic HTML**
```html
<!-- Good -->
<nav>...</nav>
<main>...</main>
<article>...</article>
<footer>...</footer>

<!-- Not as good -->
<div class="nav">...</div>
<div class="main">...</div>
```

2. **Always Include Alt Text**
```html
<img src="logo.png" alt="Company logo">
```

3. **Use Proper Heading Hierarchy**
```html
<h1>Main Title</h1>
  <h2>Section</h2>
    <h3>Subsection</h3>
```

### CSS Best Practices

1. **Use Classes Over IDs for Styling**
```css
/* Good */
.button { }

/* Avoid for styling */
#button { }
```

2. **Keep Specificity Low**
```css
/* Good */
.card-title { }

/* Too specific */
body div.container div.card h3.card-title { }
```

3. **Use CSS Variables**
```css
:root {
  --primary-color: #4CAF50;
  --text-color: #333;
}

.button {
  background-color: var(--primary-color);
  color: white;
}
```

## 🎯 What This Means for BAs

Understanding HTML and CSS helps you:

1. **Read mockups better** - Know what's possible
2. **Write better requirements** - Use correct terminology
3. **Estimate complexity** - Simple layout vs complex
4. **Communicate with designers** - Understand constraints
5. **Review implementations** - Spot missing elements

## ✅ Key Takeaways

- HTML provides structure and meaning
- CSS provides visual styling
- Every website uses HTML and CSS
- Responsive design is essential
- Google Fonts make typography easy
- Modern CSS (Flexbox, Grid) makes layout simpler
- Semantic HTML improves accessibility

Next Module: [JavaScript & TypeScript →](../04-JavaScript-TypeScript/README.md)