# MongoDB and Mongoose

There are generally two types of databases to choose from: 

1. **SQL databases**: use tables, rows and columns to store records of data
2. **NoSQL databases**: use collections and documents.

## MongoDB

**MongoDB** is a NoSQL database. A NoSQL database is split up into **collections**, which are a bit like tables in SQL databases. Each collection is used to store a particular type of data, in the form of **documents**. We can have as many collctions as we want - but each collection would only store one type of document. For example, the User Collection would only store User documents.

A document is a bit like a record (row of a table) in a SQL database. Each document represents a single item of data. For example, a Blog document inside a Blog Collection, would represent a single blog. 

The documents are stored in a format that looks a lot like JSON or JS objects. An example of what could be contained in one Blog document: 

``` 
{
  "_id": ObjectId(12345), 
  "title": "Opening Party",
  "body": "Lorem Ipsum..."
}
```
The id of a document is unique and automatically generated.

From our code, we could connect to a collection and then save/read/update/delete documents inside it.  


## Setting up MongoDB with Atlas

There are several ways to use MongoDB. We can either install it locally on our computer, or we can use a cload database that is already hosted, and easier to manage. We will use the cloud service called **MongoDB Atlas** (https://www.mongodb.com/cloud/atlas).

Once logged in, create a free cluster (choose the default options for provider and region) and give it a name. The cluster will take a few minutes to be created. 

Once created, click on **browse collections** > **add my own data** and fill in the name of the database and collection. 

Then, go to the **Database Access** tab on the left and **add a new database user**. Fill in the name and password for the new user. Give read and write access to any database. Only valid users that we declare can access the database. 

Go back to the Database tab and for the cluster, click on **connect**. 

1. Setup connection security
  - select "add your current IP Address"

2. Choose a connection method
  - select "connect to your application"

3. Connect
  - copy the connection string

Back to our app.js, at the top of the file, create a new variable:

`const dbURI = [paste the connection string];`

We will connect to MongoDB using this connection string. We will need to replace the dummy values in the connection string with the user's credentials, and the name of the database (to replace `test`).

We could connect using the plain MongoDB API package, and make queries to the database with the same package. However this can get a bit verbose, so we will use **Mongoose** to connect and interact with the database. 

Mongoose is an **ODM (Object Document Mapping)** library. That means it wraps the standard MongoDB API and provides a much easier way to communicate with the database. It does this by allowing us to create simple data models which have DB query methods to create, update and delete database documents. 

## Schemas and Models

In Mongoose, we make a **schema** to describe the structure of a data type / document. The schema describes the properties and property types they should have. 

Examples:

**User Schema**
- name (string), required
- age (number)
- bio (string), required

**Blog Schema**
- title (string), required
- body (string), required
(it will also have an automatically generated id)

The structure of documents stored in the respective collections of the database will follow their schema. After creating the schema, we create a **model** based on a specific schema. The model is what allows us to communicate with a particular database collection. For example, the Blog Model will have methods we can use to save, update, delete or read data from the Blogs Collection. 

## Connecting to the DB with Mongoose

First, install Mongoose `npm i mongoose`. 

```
const mongoose = require('mongoose');

// const dpURI = connection string

mongoose.connect(dbURI);
```

The connect() method takes the connection string as an argument. We can pass a second argument, an options object, to stop the deprecatin warnings (optional). 
`mongoose.connect(dbURI, { useNewUrlParser: true, useUnifiedTopology: true });`





