# MongoDB Cheat Sheet

## Terminology

- **Collection**: A grouping of documents inside of a database. This is the same as a table in SQL and usually each type of data (users, posts, products) will have its own collection.

- **Document**:
  A record inside of a collection. This is the same as a row in SQL and usually there will be one document per object in the collection. A document is also essentially just a JSON object.

- **Field**: A key value pair within a document. This is the same as a column in SQL. Each document will have some number of fields that contain information such as name, address, hobbies, etc. An important difference between SQL and MongoDB is that a field can contain values such as JSON objects, and arrays instead of just strings, number, booleans, etc.

# Installation

## On MacOS
```bash
brew tap mongodb/brew
brew update
brew install mongodb-community@6.0

# for shell
brew install mongosh
```

## Toggle MongoDB Service
```bash
brew services start mongodb-community@6.0
brew services stop mongodb-community@6.0
```

**Note**: For other os follow [these instructions](https://www.mongodb.com/docs/mongodb-shell/install/#std-label-mdb-shell-install).

## Access Shell
```bash
mongosh
```

# Basic

## Show All Databases

```bash
show dbs
```

## Show Current Database

```bash
db
```

## Create Or Switch Database

```bash
use posts
```

## Drop

```bash
db.dropDatabase()
```

## Create Collection

```bash
db.createCollection('posts')
```

### Show Collections

```bash
show collections
```

# Create

## Insert Row

```bash
db.posts.insertOne({
  title: 'Post One',
  body: 'Body of post one',
  tags: ['news', 'events'],
  reactions: 2,
  views: 20,
  userId: 1,
})
```

## Insert Multiple Rows

```bash
db.posts.insertMany([
  {
    title: 'Post Two',
    body: 'Body of post two',
    category: 'Technology',
    tags: ['news', 'events'],
    reactions: 20,
    views: 30,
    userId: 1,
    date: Date()
  },
  {
    title: 'Post Three',
    body: 'Body of post three',
    category: 'News',
    tags: ['news', 'events'],
    reactions: 12,
    views: 10,
    userId: 1,
    date: Date()
  },
  {
    title: 'Post Four',
    body: 'Body of post three',
    category: 'Entertainment',
    tags: ['news', 'events'],
    reactions: 10,
    views: 3,
    userId: 1,
    date: Date()
  }
])
```

# Read

## Get All Rows

```bash
db.posts.find()
```

## Get All Rows Formatted

```bash
db.posts.find().pretty()
```

## Find Rows

```bash
db.posts.find({ userId: 1 })
```

## Find One Row

```bash
db.posts.findOne({ id: 1 })
```

## Find Specific Fields

```bash
db.posts.find({ title: 'Post One' }, {
  title: 1,
  userId: 1
})
```

## Count Rows

```bash
db.posts.find().count()
db.posts.find({ reactions: 2 }).count()
```

# Read Modifiers

## Sort Rows

```bash
# asc
db.posts.find().sort({ title: 1 }).pretty()
# desc
db.posts.find().sort({ title: -1 }).pretty()
```

## Limit Rows

```bash
db.posts.find().limit(2).pretty()
```

## Chaining

```bash
db.posts.find().limit(2).sort({ title: 1 }).pretty()
```

## Greater & Less Than

```bash
db.posts.find({ views: { $gt: 2 } })
db.posts.find({ views: { $gte: 7 } })
db.posts.find({ views: { $lt: 7 } })
db.posts.find({ views: { $lte: 7 } })
```

## Complex Filter

```bash
db.posts.find({ name: { $eq: "shahriar" } })
db.posts.find({ name: { $ne: "shahriar" } })
db.posts.find({ name: { $in: ["shahriar", "swim"] } })
db.posts.find({ name: { $nin: ["shahriar", "swim"] } })
db.posts.find({$or: [{name: "shahriar"}, {views: 3}]})
db.posts.find({name: {$not: {$eq: "shahriar"}}})
db.posts.find({name: {$exists: true}})
db.posts.find({ $expr: { $gt: ["$views", "$likes"] } })
```

# Update

## Update Row

```bash
db.posts.update({ title: 'Post Two' },
{
  title: 'Post Two',
  body: 'New body for post 2',
  date: Date()
})
```

## Update Specific Field

```bash
db.posts.update({ title: 'Post Two' },
{
  $set: {
    body: 'Body for post 2',
    userId: 2
  }
})
```

## Increment Field ($inc)

```bash
db.posts.update({ title: 'Post Two' },
{
  $inc: {
    reactions: 5
  }
})
```

## Rename Field

```bash
db.posts.update({ title: 'Post Two' },
{
  $rename: {
    reactions: 'favorites'
  }
})
```

## Rename Field

```bash
db.posts.update({ title: 'Post Two' },
{
  $rename: {
    reactions: 'favorites'
  }
})
```

## Remove a Field

```bash
db.posts.replaceOne({ title: 'Post Two' },
{
  $unset: {title: ""}
})
```

## Push into a Field

```bash
db.posts.update({ title: 'Post Two' },
{
  $push: {tags: "new tag"}
})
```

## Pull from a Field

```bash
db.posts.update({ title: 'Post Two' },
{
  $pull: {tags: "new tag"}
})
```

# Delete

## Delete Row

```bash
db.posts.deleteOne({ title: 'Post Four' })
```

## Delete Multiple Row

```bash
db.posts.deleteMany({ userId: 1 })
```
