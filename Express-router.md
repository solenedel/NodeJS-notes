# Express router

The **express router** is built into Express, and we can use it to manage our routes more neatly and efficiently, especially when the number of routes becomes larger. Using the express router, we can split up our routes into different files, and manages them as small groups of routes that belong together. 

For example, we have many different routes that start with /blogs. We can split them into their own route file since they belong together. In the main project directory, create a folder called `routes`. Inside it, create `blog-routes.js`. Cut the /blogs routes from `app.js` and paste them into the blog routes file. 

The new blog routes file will not work yet because we are trying to attach the route handlers to the app instance, which is not defined yet in the file. At the top of `blog-routes.js`, we need to require express and also create a new instance of a router object like: `const router = express.Router()`.




Make sure you place the blogs/create GET route ABOVE the /blogs/:id GET route in the code. Otherwise express will fire the /blogs/:id handler for requests to /blogs/create.