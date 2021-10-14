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




