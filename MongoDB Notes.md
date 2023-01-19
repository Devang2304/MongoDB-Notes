# MongoDB Notes

> MongoDB Crash Course 2022 < TODO: Add Video Link

## Table of Contents
- [Check `monosh` Version](#check-monosh-version)
- [Start the Mongo Shell](#start-the-mongo-shell)
- [Show Current Database](#show-current-database)
- [Show All Databases](#show-all-databases)
- [Create Or Switch Database](#create-or-switch-database)
- [Drop Database](#drop-database)
- [Create Collection](#create-collection)
- [Show Collections](#show-collections)
- [Insert Document](#insert-document)
- [Insert Multiple Documents](#insert-multiple-documents)
- [Find All Documents](#find-all-documents)
- [Find Documents with Query](#find-documents-with-query)
- [Sort Documents](#sort-documents)
- [Count Documents](#count-documents)
- [Limit Documents](#limit-documents)
- [Chaining](#chaining)
- [Find One Document](#find-one-document)
- [Update Document](#update-document)
- [Update Document or Insert if not Found](#update-document-or-insert-if-not-found)
- [Increment Field (`$inc`)](#increment-field-inc)
- [Update Multiple Documents](#update-multiple-documents)
- [Rename Field](#rename-field)
- [Delete a Document](#delete-a-document)
- [Delete Multiple Documents](#delete-multiple-documents)
- [Greater & Less Than](#greater--less-than)
- [Complex-Filter-Objects]()
- [Check for equality](#check-for-equality)
- [Check for inequality](#check-for-inequality)
- [Check for > & >=]()
- [Check for more than one](#check-for-more-than-one)
- [Check if a value is none of many values](#check-if-a-value-is-none-of-many-values)
- [Check that multiple conditions are all true](#check-that-multiple-conditions-are-all-true)
- [Check that one of multiple conditions is true](#check-that-one-of-multiple-conditions-is-true)
- [Negate the filter inside of $not](#negate-the-filter-inside-of-not)
- [Check if a field exists](#check-if-a-field-exists)


## Check `monosh` Version
Open a connection to your local MongoDB instance. All other commands will be runw ithin this mongosh connection.
```js
mongosh --version
```

## Start the Mongo Shell

```js
mongosh "YOUR_CONNECTION_STRING" --username YOUR_USER_NAME
```

## Show Current Database
Show current database name

```js
db
```

## Show All Databases
Show all databases in the current MongoDB instance

```js
show dbs
```

## Create Or Switch Database
Creates the database if does not exist else switch to the existing database

```js
use blog
```

## Drop Database
Delete the current database

```js
db.dropDatabase()
```

## Create Collection

```js
db.createCollection('posts')
```

## Show Collections
Show all collections in the current database

```js
show collections
```

## Insert Document
Create a new document inside the specified collection

```js
db.posts.insertOne({
  title: 'Post 1',
  body: 'Body of post.',
  category: 'News',
  likes: 1,
  tags: ['news', 'events'],
  date: Date()
})
```

## Insert Multiple Documents
Create a new document inside the specified collection

```js
db.posts.insertMany([
  {
    title: 'Post 2',
    body: 'Body of post.',
    category: 'Event',
    likes: 2,
    tags: ['news', 'events'],
    date: Date()
  },
  {
    title: 'Post 3',
    body: 'Body of post.',
    category: 'Tech',
    likes: 3,
    tags: ['news', 'events'],
    date: Date()
  },
  {
    title: 'Post 4',
    body: 'Body of post.',
    category: 'Event',
    likes: 4,
    tags: ['news', 'events'],
    date: Date()
  },
  {
    title: 'Post 5',
    body: 'Body of post.',
    category: 'News',
    likes: 5,
    tags: ['news', 'events'],
    date: Date()
  }
])
```

## Find All Documents

```js
db.posts.find()
```

## Find Documents with Query
Find all documents that match the filter obejct 

```js
db.posts.find({ category: 'News' })
```

## Sort Documents
Sort the results of a find by the given fields
Get all users sorted by name in alphabetical order and then if nay names are the same sort by aga in reverse order

### Ascending

```js
db.posts.find().sort({ title: 1 })
```

### Descending

```js
db.posts.find().sort({ title: -1 })
```

## Count Documents

```js
db.posts.find().count()
db.posts.find({ category: 'news' }).count()
```

## Limit Documents

```js
db.posts.find().limit(2)
```

## Chaining

```js
db.posts.find().limit(2).sort({ title: 1 })
```

## Find One Document

```js
db.posts.findOne({ likes: { $gt: 3 } })
```

## Update Document

```js
db.posts.updateOne({ title: 'Post 1' },
{
  $set: {
    category: 'Tech'
  }
})
```

## Update Document or Insert if not Found

```js
db.posts.updateOne({ title: 'Post 6' },
{
  $set: {
    title: 'Post 6',
    body: 'Body of post.',
    category: 'News'
  }
},
{
  upsert: true
})
```

## Increment Field (`$inc`)

```js
db.posts.updateOne({ title: 'Post 1' },
{
  $inc: {
    likes: 2
  }
})
```

## Update Multiple Documents

```js
db.posts.updateMany({}, {
  $inc: {
    likes: 1
  }
})
```

## Rename Field

```js
db.posts.updateOne({ title: 'Post 2' },
{
  $rename: {
    likes: 'views'
  }
})
```

## Delete a Document

```js
db.posts.deleteOne({ title: 'Post 6' })
```

## Delete Multiple Documents

```js
db.posts.deleteMany({ category: 'Tech' })
```

## Greater & Less Than

```js
db.posts.find({ views: { $gt: 2 } })
db.posts.find({ views: { $gte: 7 } })
db.posts.find({ views: { $lt: 7 } })
db.posts.find({ views: { $lte: 7 } })
```

## Complex Filter Object

### Check for equality
Get all users with the name  DEVANG

```js
db.users.find({name: {$eq :"DEVANG"}})
```

### Check for inequality
Get all the users with a name other than DEVANG

```js
db.users.find((name: {&ne :"DEVANG"}))
```

### Check for > & >=
Get all uses with an age greater than 12
Get all users with an age greater than or equal to 15

```js
$gt/$gte
```
```js
db.users.find({age: {$gt:12}})
db.users.find({age: {$gte:15}})
```

### Check for < & <=
Get all uses with an age less than 12
Get all users with an age less than or equal to 15

```js
$lt/$lte
```
```js
db.users.find({age: {$lt:12}})
db.users.find({age: {$lte:15}})
```
### Check for more than one
Get all users with a name of Swastik or Shubham

```js
$in
```
```js
db.users.find({age: {$in:["Shubham","Swastik"]}})
```

### Check if a value is none of many values
Get all users that do not have the name Swastik or Shubham

```js
$nin
```
```js
db.users.find({ name: { $nin: ["Shubham", "Swastik"] } })
```

### Check that multiple conditions are all true
Get all users that have an age of 19 and the name Shubham
This is an alternative way to do the same thing. Generally you do not need $and

```js
$and
```
```js
db.users.find({ $and: [{ age: 19 }, { name: "Swastik" }] })
db.users.find({ age: 19, name: "Shubham" })
```

### Check that one of multiple conditions is true
Get all users with a name of Swastik or an age of 19

```js
$or
```
```js
db.users.find({ $or: [{ age: 19 }, { name: "Swastik" }] })
```

### Negate the filter inside of $not
Get all users with a name other than Devang

```js
db.users.find({ name: { $not: { $eq: "Devang" } } })
```

### Check if a field exists
Get all users that have a name field

```js
db.users.find({ name: { $exists: true } })
```