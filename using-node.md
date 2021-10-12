# Using node

When using node, the terminal acts as the console for us. 
In a full-stack app, the terminal will show server-side logs and the browser will show client-side logs.

- Running a single file using node: `node filename`

(We can omit the .js for JS files)

There are a few differences between the JS we use on the front end, and the one we use with node: 

## The global object and the window object

Node gives us access to the **global object** which is a bit like the **window object** in front-end JS. 

When we use methods on the window object, we can omit the `window.method` because that is implied. For example we directly use `setTimeout()` instead of `window.setTimeout()` .

Inside the browser, window is our global object but in node, the global object is just `global`. If we log global to the console, we can see properties and methods including ones on the window object like setTimeout. 

Just like with window, the global is implied so we can omit it when using a property or method on it. 

On the global object, we have access to:
-  `__dirname` which gives the absolute path to the current directory.
- `__filename` which gives the absolute path to the current file.

 Many things in the window object cannot be accessed via node's global object. For example, DOM methods like `querySelector`. Node is used on the server side so interacting with the browser is not necessary. 




