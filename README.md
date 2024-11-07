# Expressjs-Documentaion

## What is Express.JS?

**Express.js** ek web framework hai jo Node.js ka use krke bnaya gya hai.   
Yeh hume web applications banane mein madad karta hai.  
Iske features like routing, middleware, and template engine integration hmare code ko simple and organized bnay rkthe hai.

### **Web Application Kya Hai?**
A **web application** ek aisi software application hai jo web browser ke through use ki ja sakti hai. Iska matlab hai ki aapko is application ko use karne ke liye apne computer pe koi special software install karne ki zarurat nahi hoti, bas internet aur web browser (jaise Chrome, Firefox) hona chahiye.

### **Kaise Kaam Karti Hai Web Application?**
1. **Client-Side**: Yeh wo part hota hai jo user ke browser mein run hota hai. Isme HTML, CSS, aur JavaScript ka use hota hai taaki aapko ek interactive aur user-friendly interface mil sake.
   - **Example**: Jab aap Google Docs use karte hain, to uska interface aapke browser mein load hota hai.
   
2. **Server-Side**: Yeh wo part hota hai jo server pe run hota hai aur client se aane wale requests ko handle karta hai. Isme data processing, storage, aur logic manage hoti hai.
   - **Example**: Jab aap Google Docs mein koi document save karte hain, to wo data server pe save hota h

## Framework

Ek framework predefined tools aur guidelines ka set hota hai jo development process ko simplify aur standardize karta hai.

<b>Web App Framework </b>- framework jo web applications develop karne ke liye tools aur guidelines provide karta hai.

## Node.Js

Runtime environment hai jo JavaScript ko server-side pe run karne ke liye use hota hai.

<b>Runntime</b>- Runtime woh time period hota hai jab aapka program actually execute ho raha hota hai.

<b>Enviroment</b>- Surrounding or condition for something to exist or happen.

It is enviroment where we can excecute javascript to give instructions to computer to do a task.

Because of nodejs we can create server side applications wriiten in javascript.

## Why we need expressjs and not plain nodejs

Let’s compare Node.js and Express.js code examples.

### **1. Simplified Routing**

#### **Node.js Only:**
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  if (req.method === 'GET' && req.url === '/') {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello, World!');
  } else if (req.method === 'POST' && req.url === '/submit') {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ message: 'Data received' }));
  } else {
    res.writeHead(404, { 'Content-Type': 'text/plain' });
    res.end('Not Found');
  }
});

server.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

#### **Express.js:**
```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.post('/submit', (req, res) => {
  res.json({ message: 'Data received' });
});

app.use((req, res) => {
  res.status(404).send('Not Found');
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

### **2. Middleware Support**

#### **Node.js Only:**
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  // Log the request method and URL
  console.log(`${req.method} ${req.url}`);

  if (req.method === 'GET' && req.url === '/') {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello, World!');
  } else {
    res.writeHead(404, { 'Content-Type': 'text/plain' });
    res.end('Not Found');
  }
});

server.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

### **3. Static File Serving**


**Node.js** without any additional frameworks requires more manual setup to serve static files.
#### **Example: Node.js Only**

2. **Server Setup (server.js)**:
   ```javascript
   const http = require('http');
   const fs = require('fs');
   const path = require('path');

   const server = http.createServer((req, res) => {
     let filePath = path.join(__dirname, 'public', req.url === '/' ? 'index.html' : req.url);
     let extname = path.extname(filePath);
     let contentType = 'text/html';

     switch (extname) {
       case '.js':
         contentType = 'text/javascript';
         break;
       case '.css':
         contentType = 'text/css';
         break;
       case '.json':
         contentType = 'application/json';
         break;
       case '.png':
         contentType = 'image/png';
         break;
       case '.jpg':
         contentType = 'image/jpg';
         break;
     }

     fs.readFile(filePath, (err, content) => {
       if (err) {
         if (err.code == 'ENOENT') {
           fs.readFile(path.join(__dirname, 'public', '404.html'), (err, content) => {
             res.writeHead(404, { 'Content-Type': 'text/html' });
             res.end(content, 'utf8');
           });
         } else {
           res.writeHead(500);
           res.end(`Server Error: ${err.code}`);
         }
       } else {
         res.writeHead(200, { 'Content-Type': contentType });
         res.end(content, 'utf8');
       }
     });
   });

   server.listen(3000, () => {
     console.log('Server running on port 3000');
   });
   ```


**Express.js** provides a built-in middleware function to serve static files, which simplifies the process.

#### **Example: Express.js**
   ```javascript
   const express = require('express');
   const path = require('path');

   const app = express();

   // Serve static files from the "public" directory
   app.use(express.static(path.join(__dirname, 'public')));

   // Route for handling 404 requests
   app.use((req, res) => {
     res.status(404).sendFile(path.join(__dirname, 'public', '404.html'));
   });

   app.listen(3000, () => {
     console.log('Server running on port 3000');
   });
   ```

#### **Express.js:**
```javascript
const express = require('express');
const app = express();

// Middleware to log the request method and URL
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
});

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.use((req, res) => {
  res.status(404).send('Not Found');
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```


### **4. Template Engines**

#### **Node.js Only:**
```javascript
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
  if (req.method === 'GET' && req.url === '/') {
    fs.readFile('index.html', (err, data) => {
      if (err) {
        res.writeHead(500, { 'Content-Type': 'text/plain' });
        res.end('Internal Server Error');
      } else {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end(data);
      }
    });
  } else {
    res.writeHead(404, { 'Content-Type': 'text/plain' });
    res.end('Not Found');
  }
});

server.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

#### **Express.js:**
```javascript
const express = require('express');
const app = express();

// Set the template engine to Pug
app.set('view engine', 'pug');

app.get('/', (req, res) => {
  res.render('index', { title: 'Hello', message: 'Hello, World!' });
});

app.use((req, res) => {
  res.status(404).send('Not Found');
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

### **Summary**
Express.js significantly simplifies web development tasks like routing, middleware integration, code organization, and rendering templates, compared to plain Node.js. It allows for cleaner, more modular, and maintainable code.









