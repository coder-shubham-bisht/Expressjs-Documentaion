# Express.js-Documentaion
<img src="./images/express-node.png"/>

## What is <a href="https://expressjs.com/">Express.JS </a>

**Express.js** ek web framework hai jo Node.js ka use krke bnaya gya hai.   
Yeh hume web applications banane mein madad karta hai.  
Iske features like routing, middleware,static file serving and template engine integration hmare code ko simple and organized bnay rkthe hai.

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

##  <a href="https://nodejs.org/en">Node.Js</a>

Runtime environment hai jo JavaScript ko server-side pe run karne ke liye use hota hai.

<b>Runtime</b>- Runtime woh time period hota hai jab aapka program actually execute ho raha hota hai.

<b>Enviroment</b>- Surrounding or condition for something to exist or happen.

It is enviroment where we can excecute javascript to give instructions to computer to do a task.

Because of nodejs we can create server side applications wriiten in javascript.

## Why we need expressjs and not plain nodejs

Let’s compare Node.js and Express.js code examples.

### **1. Routing**

Routing ka matlab hai ki application kaise respond karti hai user ki specific URL requests par. Yeh define karta hai ki jab user kisi particular URL ko access karega, toh application kya action legi.

#### **Node.js:**

Node.js mein routing ko manually handle karna padta hai jo kaafi complex ho sakta hai.

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

Express.js routing ko bahut simple aur readable banata hai.

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

### **2. Middleware function Support**
Middleware funciton is a function that is executed after the request has been sent to the server but before any response given back by the server.
So it basically sits at middle of request and response cycle.

#### **Node.js**

In pure Node.js, middleware isn't a built-in feature, but you can create middleware-like functions manually.

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


#### **Express.js:**

Express has a built-in mechanism for defining and using middleware functions through methods like app.use()

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


### **3. Static File Serving**

Static file serving ka matlab hai  files ko serve karna jaise HTML, CSS, JavaScript, aur images without making any changes in the files.


#### **Node.js**

Node.js requires more manual setup to serve static files.

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

#### **Express.js**

**Express.js** makes it simple to serve static files.

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

### **4. Template Engines**

Templating engines wo tools hote hain jo hume templating krne me help krte hai.

### **Templating Kya Hai**

**Templating** ka matlab hota hai ek aise mechanism ka use karna jo HTML code ko dynamic data ke sath generate kar sake. Yeh web development mein common hai, jahan static HTML pages ke bajaye dynamic content generate kiya jata hai based on user inputs ya server-side data.

#### **Node.js**
**Node.js** templating engines ko use karna kaafi complex bana deta hai.

   ```javascript
   const http = require('http');
   const fs = require('fs');
   const path = require('path');
   const pug = require('pug');

   const server = http.createServer((req, res) => {
     if (req.method === 'GET' && req.url === '/') {
       fs.readFile(path.join(__dirname, 'views', 'index.pug'), 'utf8', (err, data) => {
         if (err) {
           res.writeHead(500, { 'Content-Type': 'text/plain' });
           res.end('Internal Server Error');
           return;
         }
         const compiledFunction = pug.compile(data);
         const html = compiledFunction({ title: 'Welcome', message: 'Hello, World!' });
         res.writeHead(200, { 'Content-Type': 'text/html' });
         res.end(html);
       });
     } else {
       res.writeHead(404, { 'Content-Type': 'text/plain' });
       res.end('Not Found');
     }
   });

   server.listen(3000, () => {
     console.log('Server running on port 3000');
   });
   ```

### **Express.js**

**Express.js** templating engines ko use karna kaafi asaan bana deta hai compared to plain **Node.js**

   ```javascript
   const express = require('express');
   const path = require('path');
   const app = express();

   // Set the template engine to Pug
   app.set('view engine', 'pug');
   app.set('views', path.join(__dirname, 'views'));

   app.get('/', (req, res) => {
     res.render('index', { title: 'Welcome', message: 'Hello, World!' });
   });

   app.listen(3000, () => {
     console.log('Server is running on port 3000');
   });
   ```


## **Summary**
Express.js significantly simplifies web application development tasks like routing, middleware integration,static files serving, and rendering templates, compared to plain Node.js. It allows for cleaner, more modular, and maintainable code.









