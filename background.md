# What is node JS ?

Front end code runs in the browser, while back end code runs in the server. JS could historically only run in the browser. Now, node lets us run code on the server side.

Computers cannot directly understand JS or compile it into machine code, since it is one more level of abstraction compared to other languages like C++. We can't run JS directly on a computer, but it can run inside a browser using the V8 engine which is written in C++ and compiles JS into machine code. 

Outside the browser there is no V8 engine, this is where node comes in. Node.js is a program (also written in C++) that wraps the V8 engine. node can run directly on our computer or server. 

Node adds more functionality to JS: 
- reading & writing files on a computer
- connecting to a database
- act as a server for content

Normally, JS can't do the above things because it was designed only as a client-side language. 

However, when using node to run JS outside the browser we lose features such as interacting with the DOM. This is not something we need on the server side anyway. 

Node has become an alternative to other server-side languages like Python, Ruby, etc. Meaning we can now use JavaScript for the full stack. 

Advantages of using node as the back end (over other languages):
- since we use node by writing JS code, we can easily share code between the front and back end. 
- node has a huge amount of third-party packages.
