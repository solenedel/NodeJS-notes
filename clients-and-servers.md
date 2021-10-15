# Clients and servers

All computers connected to the internet have a unique **ip address** to identify them. Some special computers are **hosts** - they host websites on the internet. If we want to connect to a server on that host computer, we need to know it's ip address. Websites have **domain names** to mask the ip address of the host computer. 

When we type the domain name in the browser, it will look up the ip address of the computer hosting the website and connect to the server on the host computer with that ip address. The server will probably respond by sending back an HTML page. This client-server communication is done via **HTTP**. 

In node, we write code to manually create a server and listen for requests coming in from the browser. 

**💡 NOTE: The techniques in this document use core node.js (without Express or any other frameworks). To skip to using Express, see the Express notes.**


## creating a server using node

Require the HTTP module: 

`const http = require('http');`

Create a server using createServer:

```
http.createServer( (req, res) => {
  console.log('a request was made');
});
```
The callback function runs every time a request comes in to the server. 

NOTE: if we wanted to use that instance of the server (ex. for websockets), we store the server in a variable:
`const server = http.createServer()`

At this point, the server is not yet actively listening for requests. To start listening, we us the listen() method. 

```
server.listen(3000, 'localhost', () => {
  console.log('server is listening for requests on port 3000');
});
```

- argument 1: port number the server runs on
- argument 2: host name (default value is localhost)
- argument 3: callback that runs when the server starts listening for requests


## localhost and port numbers

**Localhost** is like a domain name we would use on the web, but it takes us to a specific **loop back ip address** which is **127.0.0.1**. It points directly back to your own computer, which is acting as a host for your website.

A **port number** represents a specific gateway or channel in the computer that a certain bit of software or server should communicate through. The different software (applications) on a computer that connect to the internet all run on different ports to keep information separate. A port is a bit like a door in the computer for communication between a software and the internet. 

Our server we set up also needs its own port number to communicate through. A common port for local web development is **3000**. This port should be fine to use as long as it doesn't clash with another program running on that port. 

While we are running a server with node, the terminal should not display any prompt or accept new commands. This means the server is running and listening for requests. To stop the server: **CTRL + C** . We need to stop and restart the server every time there is a change to the server file. 

Server-side logs will not show in the browser's console, which is only for client-side logs.

Also note that js scripts linked in the body of an html page will not run on the server-side, but on the client-side.

To see our application in the browser, we type `localhost:3000` in the address bar. At this point we will see that a request has been made but there is no response sent back from the server to the browser. 


## Sending a response to the browser 


### **the request object**

The request object from createServer is an object with a huge amount of information. Notable ones are the url (`req.url`) and the type of request, or HTTP method (`req.method`). If we log these to the console and go to `localhost:3000` we can see that the url is **/** and the method is **GET**.

/ is the **root** of the website- the point in the url that comes just after `localhost:3000`. It is basically the 'main page' of a website. If we went to `localhost:3000/about`, the url would be /about. 

### **the response object**

The response object has several methods to help with sending a response. 

- `setHeader` lets us give information (ex. content type) about the response
- `write` lets us specify what content to send back
- `end` marke the end of the response

Inside the `createServer` callback function: 
```
  res.setHeader('Content-Type', 'text/html');
  res.write('<p>Hello, world</p>'); // first line of html
  res.write('<p>Hello again</p>'); // second line of html
  res.end();
```

No need to include the `<body>` tag, it is automatically created, as is the `<head>` tag.
This is not a realistic way to send html for a web page, instead we should have our html 
pages already created in the project directory. We can use the file system again to send these html files to the brower in the response.

## returning html pages

We can create a folder called views which will contain all the html pages we need to send back from different requests. Let's create an index.html file inside views. 

```
// set header content type
res.setHeader('Content-Type', 'text/html');

// send an html file
fs.readFile('./views/index.html', (err, data) => {
  if (err) {
    console.log(err);
    res.end();
  } else {
    res.write(data);
    res.end();
    // shortcut: if only sending one thing to res.write, we can omit the write method and directly use res.end(data);
  }
});
```

## basic routing

At this point, whatever route we go to: /, /about, /blogs ... we always get back the same index.html page. If a user goes to /about, we want to send the about.html page. If a user goes to a page that doesn't exist, we want to send back a 404 page. 

```
const server = http.createServer((req, res) => {
  res.setHeader('Content-Type', 'text/html');

  let path = './views/';  // all html pages are in the views folder

  switch(req.url) {
    case '/': 
      path += 'index.html';
      break;
    case '/about': 
      path += 'about.html';
      break;
    default:
      path += '404.html';
      break;
  }


  fs.readFile(path, (err, data) => {
    if (err) {
      console.log(err);
      res.end();
    } else {
      res.end(data);
    }
  }

});
```




## Status codes

The **status code** describes the type of response sent to the browser, and how successful the request was. 

### common status codes
- 200 - OK (successful, no issues)
- 301 - Resource was moved permanently
- 404 - the resource was not found
- 500 - internal server error


### status code ranges
- 100 range - informational responses (for the browser)
- 200 range - successful responses
- 300 range - redirections
- 400 range - user or client errors (front-end)
- 500 range - server errors (back-end)

We can set the status codes of responses manually using `res.statusCode`.

```
switch(req.url) {
    case '/': 
      path += 'index.html';
      res.statusCode === 200;
      break;
    case '/about': 
      path += 'about.html';
      res.statusCode === 200;
      break;
    default:
      path += '404.html';
      res.statusCode === 404;
      break;
  }
  ```

In the browser's dev tools, go to the Network tab and navigate between pages to see the status codes show up in the table. 

## Redirects

Let's say we changed the url of /about-me to /about. Now, if other websites are linking to the old /about-me, we need to redirect the user to go to /about. 

```
  case '/about': 
      path += 'about.html';
      res.statusCode === 200;
      break;
  case '/about-me': 
      res.statusCode === 301; // permanent redirect
      res.setHeader('Location', '/about');
      res.end();
      break;
```

As our websites grow in complexity and number of pages, the methods above for routing and redirection become messy and hard to maintain. The third-party package **Express** helps us manage these things much more simply. 


