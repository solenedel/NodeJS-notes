# Middleware

**Middleware** is any code that runs on the server, between getting a request and sending the response. The use() method is generally used to run middleware code. We can have several pieces of middleware that run between a request and response. 

The functions that run in our GET handlers are also essentially middleware. The order of middleware is very important to how it runs. 

Examples & uses of middleware code: 
- logger middleware to log details of every request to the console
- authentication checking middleware for protected routes 
- middleware to parse JSON data from requests
- returning 404 pages 


## creating our own middleware

Let's create a logger middleware: 

```
app.use((req, res) => {
  console.log('a new request was made');
  console.log('host: ', req.hostname);
  console.log('path: ', req.path);
  console.log('method: ', req.method);
});
```

This code should be at the top of app.js, (just after `app.listen()`) before any other route handlers so that it fires for every request. At this point, the logs will be seen in the console when we change pages, but in the browser itself, the page is not changing and is loading eternally. This is because Express does not know how to get to the next piece of middleware after the logger middleware. 


## Using next()

The next() method lets us explicitly tell Express to move on to the next piece of middleware. We get access to next() as a third parameter to the use() method: 

```
app.use((req, res, next) => {
  // console logs
  next();
});
```

If we send a response before the next piece of middleware- even if we used next() in the previous middleware, the next middleware will not run. 


## Third party middleware

There are loads of already created middleware functions we can use (as npm packeges). For example Morgan (a logger middleware), Cookie session, etc. 

Example: using Morgan

```
const morgan = require('morgan');

app.use(morgan('dev'));
```


## Static files 

Static files include images or CSS files. We cannot automatically access these files from the browser. To allow browser access to such files, we need to specify which ones should be accessed explicitly- which files should be public. 

The static middleware comes built into Express.

```
app.use(express.static('public'));
```
Here we specify that any static files inside the folder called public will be available to the front-end.






