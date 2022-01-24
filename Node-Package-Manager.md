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

Install the package globally:  `npm i -g nodemon`

To run the server with nodemon: `nodemon server` (if the file we run is server.js)

**NOTES / BUGS**
- if the Nodemon command does not work, it is likely something to do with local vs global installation. Nodemon can be run from node_modules (locally) with the command `npx nodemon`.
- Also make sure there is a start script with nodemon in package.json

### **Lodash**

Lodash provides various different methods such as:
- generating a random number with `random()`
- allowing a function to be run only **once** with `once()`

After installing it, we need to require the library at the top of each file that uses it. 

`const _ = require('lodash')` 

It's common practice is to denote lodash with an underscore _ .

To use a method from lodash: `_.methodName()`

## the node_modules folder

When we install a local package into the project, a folder called `node_modules` is created in the main project directory. All the different files needed for that package are contained inside node_modules. We never need to directly go into this folder.

The package.json file allows us to easily share project code with others. node_modules is a very large folder and should never be committed to GitHub (it should be **git ignored**). Instead, we can simply upload the package.json file to GitHub. 

Then, once the project is opened on someone else's computer, they can simply `npm install` to generate the node_modules folder locally, using the package.json as a reference for what packages and versions to install. 






