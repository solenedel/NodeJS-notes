# Clients and servers

All computers connected to the internet have a unique **ip address** to identify them. Some special computers are **hosts** - they host websites on the internet. If we want to connect to a server on that host computer, we need to know it's ip address. Websites have **domain names** to mask the ip address of the host computer. 

When we type the domain name in the browser, it will look up the ip address of the computer hosting the website and connect to the server on the host computer with that ip address. The server will probably respond by sending back an HTML page. This client-server communication is done via **HTTP**. 

In node, we write code to manually create a server and listen for requests coming in from the browser. 



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

Our server we set up also needs its own port number to communicate through. A common port for local web development is 3000. This port should be fine to use as long as it doesn't clash with another program running on that port. 

While we are running a server with node, the terminal should not display any prompt or accept new commands. This means the server is running and listening for requests. To stop the server: CTRL + C . Server-side logs will not show in the browser's console, as it is only made for client-side logs.

To see our application in the browser, we type `localhost:3000` in the address bar. At this point we will see that a request has been made but there is no response sent back from the server to the browser. 


## Sending a response to the browser 




