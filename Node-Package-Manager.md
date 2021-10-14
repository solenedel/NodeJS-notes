# NPM: Node Package Manager

If we want to use node packages that are not available in core node.js, we need to install them using npm. Search for diferent packages in npmjs.com.

## Installing packages globally

Installing globally means we aren't just installing it for our current project, but anywhere on our computer. 

Installing globally:  `npm install -g packagename` or `npm i -g packagename`


## Installing packages locally

To install packages that are local to our project, we need the `package.json` file. This file keeps track of any packages we install locally, as well as project details and scripts. 

Creating a `package.json` file: `npm init`

We can hit enter to use default values to the questions prompted in the terminal.

This command will also create a `package-lock.json` file, which keeps track of the different dependency versions of the project. We never need to edit anything inside this file. 

The `package.json` file keeps track of all project dependencies - the third-party packages we install locally. 

Installing a package locally: `npm i --save packagename`

The `--save` flag ensures that the package is saved into the project's dependencies. In newer versions of node, we can omit `--save` and the package will still be saved to the dependencies.


### **Nodemon**

Nodemon is a package that helps us create a live-reload server to speed up development without having to stop and restart the server. 

To run the server with nodemon: `nodemon server` (if the file we run is server.js)

**TO CONFIRM:**
We don't need to require Nodemon at the top of a file in order to run the server with it. 
OR: we only need to require locally installed packages??

### **Lodash**

Lodash provides various different methods.

After installing it, we need to require the library at the top of each file that uses it. 

`const _ = require('lodash')` 

It's common practice is to denote lodash with an underscore _ .

## the node_modules folder

When we intall a local package into the project, a folder called `node_modules` is created in the main project directory. All the different files needed for that package are contained inside node_modules. We never need to directly go into this folder. 


