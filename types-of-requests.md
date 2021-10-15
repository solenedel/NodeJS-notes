# Types of requests

The same route (ex. /blogs) will often have different types of requests:
- the get request will get that specific page
- the post request will allow a user to post a new blog

It's totally fine for the same route to have different requests, as long as they have different types. The types of requests are handled differently by the server. 

We want a route that is specific to a single blog item, which will look like:
`localhost:3000/blogs/:id`

The `:id` changes depending on the blog we want to get. This route would have a get request to view the blog, and a delete request to delete it, and a put request to update it.


## GET requests
- typically used to get resources from the server
- the resources could be JSON data from a database, or a web page, etc.


## POST requests
- create new data in the database
- ex. when we submit a web form

When we click on the submit button of the blog form, we want to send a post request. 
In the HTML, specify the action attribute (the route we want to post to) and the method (the type of request): 
```
<form action="/blogs" method="POST">
```

To send the data inside the form, add name attributes to each input field. They should be the same as the id attribute. 
```
<input type="text" id="title" name="title" required>
<input type="text" id="snippet" name="snippet" required>
<input type="text" id="body" name="body" required>
```

Let's handle the request on the server side. We need a new piece of middleware that will parse the data into a format we can use. That data will be attached to the request (req) object:

Below the express.static line:
`app.use(express.urlencoded( { extended: true }))`

This lets us access `req.body` which contains all the information we need from the web form. 
 
Below the app.get() route, create a post request:
```
app.post('/blogs', (req, res) => {
  console.log(req.body);
});
```

In the server-side console, req.body looks like: 
`{ title: 'abc', snippet: 'def', body: 'ghi' }`

**NOTE:** the 'body' key in the object is related to the body input field when a user creates a new blog. It has nothing to do with `req.body`, which is the body property on the request object. 

If we were not using the urlencoded middleware, req.body would be undefined.

In order to save the incoming data to the database, we need to create a new instance of Blog. 

```
app.post('/blogs', (req, res) => {
  const blog = new Blog(req.body);
  blog.save()
    .then((result) => {
      res.redirect('/blogs');
    })
    .catch((err) => {
      console.log(err);
    });
});
```

We should now see a newly posted blog as a document in the database. 


## Route parameters

A **route paramater** is the part of a route that is variable and may change. For example, in this route: `localhost:3000/blogs/:id` the `:id` part is the route parameter. In the browser, examples of this route could look like: 
- `localhost:3000/blogs/15`
- `localhost:3000/blogs/8`
- `localhost:3000/blogs/hello`

We need a way to extract the route parameter from a URL so that we can use it. 

To demonstrate this, we want the title of each blog in the /blogs page to link to that specific blog. Each blog has a unique id (as seen in each document in the collection). That id can be accessed in `blog._id`. 

```
<a href=`/blogs/${blog._id}`>
```

Handle the get request for each blog: 
```
app.get('/blogs/:id', (req, res) => {
  const id = req.params.id;
  Blog.findById(id)
    .then((result) => {
      ⚠️ (RENDER VIEW - EJS) ⚠️
    })
    .catch((err) => {
      console.log(err);
    });
}
```

The colon specifies that id is a route parameter, so that Express does not literally look for a route called /blogs/id. in `req.params.id`, the `id` part should correspond to the route parameter (but without the preceding colon).

The `req.params` property is an object containing properties mapped to the named route parameters. For example, if you have the route /blogs/:id, then the 'id' property is available as req.params.id.


## DELETE requests
- deleting data from the database

⚠️
(deleting from the front-end - solution uses vanilla JS and EJS.)
(Video 10 of tutorial, last section)


Deleting on the server side:

```
app.delete('/blogs/:id', (req, res) => {
  const id = req.params.id;
  Blog.findByIdAndDelete(id)
    .then((result) => {
      res.redirect('/blogs');
    })
    .catch((err) => {
      console.log(err);
    });
}
```


## PUT requests
- update existing data in the database
