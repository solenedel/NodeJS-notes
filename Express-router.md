# Express router

The **express router** is built into Express, and we can use it to manage our routes more neatly and efficiently, especially when the number of routes becomes larger. Using the express router, we can split up our routes into different files, and manages them as small groups of routes that belong together. 

For example, we have many different routes that start with /blogs. We can split them into their own route file since they belong together. In the main project directory, create a folder called `routes`. Inside it, create `blogRoutes.js`. Cut the /blogs routes from `app.js` and paste them into the blog routes file. 

The new blog routes file will not work yet because we are trying to attach the route handlers to the app instance, which is not defined yet in the file. At the top of `blogRoutes.js`, we need to require express and also create a new instance of a router object like: `const router = express.Router()`.

Now instead of attatching the route handlers to app, we attatch them to router. 

```
router.get('/blogs', (req, res) => {
  // code
});

router.post('/blogs', (req, res) => {
   // code
});
```

An instance of the router is like a mini app, but it must be used inside an app, alone it can't be used. 

At the bottom of the file, export the router:
`module.exports = router`

Import the router in `app.js`:
`const blogRouter = require('./routes/blogRoutes')`

Also, the Blog model needs to be required in the blogRoutes file, but no longer in app.js. 

To use the blogRoutes in our main express app (app.js), we just need to have, in the place of the original blog routes: 
`app.use('/blogs', blogRoutes)`


## The order of routes

The order in which we define our routes is important. 

For example:
```
router.get('/blogs/:id', (req, res) => {
  // code
});

router.get('/blogs/create', (req, res) => {
  // code
});
```

When we go to /blogs/create in the browser, how does Express know that create is not actually a blog id? Because the code runs from top to bottom, in order to make sure we reach the correct /blogs/create page, the route needs to be defined above /blogs/:id. Otherwise, it would look for a blog with an id of create instead of giving us the create page. 

As a general rule, the more specific routes should go below the less specific routes. For example if we had a `/blogs/user/:id` route, it should be below the `/blogs/user` route.


## Scoping the router

We can scope the blogs router so that it only applies when the user goes to /blogs: `app.use('/blogs', blogRoutes)`

If we use this, we must remove the '/blogs' part of each route handler like so:
```
router.get('/', (req, res) => {
  // code
});

router.get('/create', (req, res) => {
  // code
});
```
Because we scoped the router, it will automatically add /blogs to the start of the url being handled. Scoping makes the code more reusable because if we change the url structure in the future, we will only need to change the name in `app.use('/NAME', blogRoutes)`.




