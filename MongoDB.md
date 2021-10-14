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


