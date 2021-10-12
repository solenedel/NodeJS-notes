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


## Modules and require

To access code in a file A from another file B, we need to: 

1. **require** file A inside file B: `const xyz = require('./fileA')`
2. **export** the relevant code from file A: `module.exports = foo`

What if we want to export multiple things from file A? 

`module.exports = { foo, bar }`

The require statement stays the same. The value of `xyz` will now be an object containing both foo and bar. To access them individually, we would use `xyz.foo` and `xyz.bar` .

If we only want to import a specific thing, we can use object destructuring as follows:

`const { foo } = require('./fileA')`

In this case, the destructured `{ foo }` must have the exact **same name** as the corresponding `foo` in the file where it is initially declared. Using this method, we can difertly refer to `foo` in file B rather than `xyz.foo`.

Likewise, we can selectively import `foo` and `bar` like: 

`const { foo, bar } = require('./fileA')`


## Node core modules

Node has several built-in modules that can be required directly in any file.

Examples:

- The operating system module: `const os = require('os')` 
- The file system module: `const fs = require('fs')`

The file system modules lets us read, write, create and delete files. This feature cannot be done without node in JS. 

### **reading files with readFile**

```
fs.readFile('./docs/blog.txt', (err, data) => {
  if (err) console.log(err);
  console.log(data.toString());
});
```
- first argument: relative file path
- second argument: callback function
- we must convert the data into a string because it is initially a buffer.
- readFile is asynchronous and does not block the code that follows it.

### **writing files with writeFile**

The code below replaces the original text in the file with the specified text:
```
fs.writeFile('./docs/blog.txt', 'hello, world', () => {
  console.log('file was written');
});
```

- first argument: relative file path
- second argument: text we want to write to the file
- third argument: callback function
- writeFile is asynchronous.

If we try to write to a file that doesn't exist yet, it will create that file for us and insert the specified text. 

### **creating and deleting directories**

We can create a directory (again, asynchronous): 

```
fs.mkdir('./assets', (err) => {
  if (err) console.log(err);
  console.log('folder created'); 
});
```
If we run this code when the directory already exists, we get an error. To check if a directory already exists before creating it, we can use `fs.existsSync` which is synchronous, but will take a very short time to run. 

``` 
if (!fs.existsSync('./assets')) {
  // create the directory
} else {
  // if the directory already exists, delete it
  fs.rmdir('./assets', (err) => {
    if (err) console.log(err);
    console.log('folder deleted');
  });
}
```
### **deleting files with unlink**

```
if (fs.existsSync('./docs/deleteme.txt')) {
  fs.unlink('./docs/deleteme.txt', (err) => {
    if (err) console.log(err);
    console.log('file deleted);
  });
}
```


### streams and buffers

The above works well for small files. If we are dealing with very large files, we should use streams to read and write data. With **streams** , we can start using the data in a file before it has finished loading. (Using the methods above, we would have to finish until the whole file is loaded before doing anything, which can be a long time if the file is very large).

As a file is loaded, small pieces of data are packaged into buffers and progressively sent along a stream, to the browser. This is the exact logic applied to **streaming videos**. We can start watching a video before it has been fully loaded, and the video is progressively loaded. 

## ** reading files using a read stream **

``` 
const readStream = fs.createReadStream('./docs/blog3.txt');

readStream.on('data', (chunk) => {
  console.log(chunk.toString());
});
```
readStream listens for a **data event** and every time we get a new chunk of data, it fires the callback function and get access to that chunk.

`createReadStream` can take an optional second argument, an **options object** within which we can specify things such as the encoding. 

`const readStream = fs.createReadStream('./docs/blog3.txt', { encoding: 'utf8' });`
This will automatically encode the data into readable format, removing the need for `toString()` later on in the code. 


## ** writing files using a write stream **


``` 
const writeStream = fs.createWriteStream('./docs/blog4.txt');

readStream.on('data', (chunk) => {
  console.log(chunk);

  writeStream.write(chunk);
});
```

We can do the same as above in a more simple way by using **piping**.

`readStream.pipe(writeStream)`

This achieves the same as the previous code snippet.

