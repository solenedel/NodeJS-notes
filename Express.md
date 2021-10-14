# Express

Express is a framework that helps easily manage routing, requests, server-side logic and resonses much more elegantly and simply. 

Installing Express: `npm i express`

## creating an Express app

In the project directory, ceate a file that is typically called `app.js`. Require Express in that file. 

```
const express = require('express');

// create an instance of an Express app
const app = express(); 

// listen for requests
app.listen(3000);   

```

`app.listen()` returns an instance of the server. We could store this in a constant if we want to reuse it later (ex. for websockets).

## Responding to requests

For example: responding to a GET request
```
app.get('/', (req, res) => {

  res.send('<p>Home page</p>');

});
```

The res.send() method infers and automatically sets the content-type header, as well as the status code.

## Routing and HTML pages

```
app.get('/', (req, res) => {
  res.sendFile('./views/index.html', { root: __dirname });
});

app.get('/about', (req, res) => {
  res.sendFile('./views/about.html', { root: __dirname });
});
```

By default, sendFile looks for an absolute path (from the root of the computer). To give a relative path, we must specify the root we want in an options object as the second argument. 
The root is the main project directory. 

## Redirecting with Express

```
app.get('/about-us', (req, res) => {
  res.redirect('/about');
});
```

404 Page:

The app.use() method is used to create middleware in Express. 

The use() method will fire for every request that comes in, but only if the request reaches that point in the code. When we send a request for a page, Express runs through the server code in app.js from top to bottom and will look at each GET handler. If it finds a match with the page requested, it will fire the callback function and will stop looking at the subsequent route handlers. If it doesn't match, it carries on down the file. 

```
app.use((req, res) => {
  res.sendFile('./views/404.html', { root: __dirname });
});
```

Our GET route for the **404 page must be at the bottom** of our code, because if it reaches that point it means there has not yet been a match. Because the 404 handler uses app.use(), it is not tied to a specific url and will fire as long as that point in the code has been reached. However, if the 404 route was higher in the code than other routes, it would potentially "block" the routes that follows it.

In the snippet above, Express does not know that the 404 route corresponds to a 404 error. We must manually set the status code to 404. 

```
app.use((req, res) => {
  res.status(404).sendFile('./views/404.html', { root: __dirname });
});
```





