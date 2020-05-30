# learning_mongo
Exercise files for the Learning Mongo course on Lynda

Default Mongo Port: port 27017
Doesn't require a schema

## Bash
`mongoimport --help` 
`mongoimport --collection {{ collection }} --db {{ database }} --file {{ file }} --jsonArray`

## Commands
- db                                  - shows you db you're currently connected to
- use                                 - to switch to another database
- show dbs                            - to see all dbs
- show collections                    - to see all collections
- db.cars.insert({ "nInserted": 1 })  - inserts item into the 'cars' collection
- db.numbers.insert({ "number": n })  - insert number into numbers collection
- db.numbers.count()                  - numbers of documents in a collection
- db.numbers.find({ "number": 1 })    - finds a document matching the filter
- print("Hello World")                - to print 
- db.tours.drop()                     - to remove a collection
- db.tours.remove({})                 - removes all documents from a collection
- db.collection.stats() / db.stats()  - gives you information with regards to performance and memory

### Chain Commands
- .explain()                          - explains how a query was conducted
- .explain("executionStats")          - explains execution
- .pretty()                           - to format chain function
- .sort({tourPrice: -1})              - to sort chain function that sorts in descending order, use 1 for ascending order
- .sort({score: {$meta: "textScore"}})- sorts by the relevance score
- .limit(1)                           - limits the query result to 1
- .skip()                             - to pick other document than the first one returned

### Commands for results ina range
- db.tours.find({tourPrice:{$lte:1000,$gte:800}})                 - between 1000 and 800 inclusive
- db.tours.find({tourDescription:{$regex:/backpack/i}})           - serach using regular expression (i => ignore case)
- db.tours.find({tourDescription:/backpack/i},{tourName:1,_id:0}) - shortened version of above

### REST API CRUD Operation - Create, Read, Update and Delete

```
Create => Insert
Read => Find or FindOne
Update => Update
Delete => Delete
```

db.tours.insert({ "tourName":"The Wines of Santa Cruz", "tourLength":3, "tourDescription":"Discover Santa Cruz's wineries", "tourPrice":500, "tourTags":["wine","Santa Cruz"] } )

db.tours.find({"tourTags":"wine"})

db.tours.update({"tourName":"The Wines of Santa Cruz"},
... { $set: {"tourRegion":"Central Coast"}})

db.tours.update({"tourName":"The Wines of Santa Cruz"},
... { $addToSet: {"tourTags":"boardwalk"}})

db.tours.remove({"tourName":"The Wines of Santa Cruz"})

db.tours.find(
... {"tourPrice":{$lte:500},
... "tourLength":{$lte:3}}
... ) // lte = less than or equal to

### Indexing

- db.numbers.createIndex({number:1})                  - creating an index
- db.tours.createIndex({tourPrice:1, tourlength:1})   - create a compound index
- db.tours.createIndex({"tourName":1},{unique:true}); - creates an index on property forcing it to be unique
- db.tours.createIndex({tourDescription:"text",tourBlub:"text"})

You can create up 64 indexes in Mongo

### Queries

- db.tours.find({}, {tourName:1})                      - returns results with just the tourName property and _id
- db.tours.find({}, {tourName:1,_id:0})                - returns results with just the tourName property without the _id
- db.tours.find({}, {tourName:1,tourPrice:1,_id:0})    - returns results with just the tourName and the tourPrice property without the _id
- db.tours.find({$text:{$search:"wine"}})              - looks in the text and searches for wine
- db.tours.find({}, {score:{$meta:"textScore"}})       - adds a new field to the results to get the relevance score

### Aggregation 

- db.tours.aggregate([{$group: {_id: '$tourPackage', count: {$sum: 1}}}]);                                - aggregate the count of a properties / field of the database
- db.tours.aggregate([{$group: {_id: '$tourOrganizer.organizerName', count: {$sum: 1}}}]);                - aggregate the count of a properties / field of the database with dot notation to access property of object
- db.tours.aggregate([{$group: {_id: '$tourPackage', average: {$avg: '$tourPrice'}}}]);                   - aggregate the average of the tour price field
- db.tours.aggregate([{$group: {_id: '$tourPackage', average: {$avg: '$tourPrice'}}}, {$out: 'prices'}]); - outputs the results into a new collection

### Model Schema

Mongoose - a library to integrate with the database under a model driven interface
         - models the data being held in mongo to make queries as efficient as possible to help meet business use cases

### Sharding

- Partition your database
- More storage and greater capacity
- Multiple CPUs and memory
- Operates as a single database for clients
- Minor
- Minor performance hit

### Replication

- Uptime and failover
- Replica set vs. master-slave
- Automatic failover