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


