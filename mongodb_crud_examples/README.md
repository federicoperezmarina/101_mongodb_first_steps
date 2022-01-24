# Mongodb CRUD
In this chapter we are going tu see how to understand and practice the CRUD operations like create, read, update and delete

## Table of Contents
* [Mongodb Docker](#mongodb_docker)
* [Mongodb Show Databases](#mongodb_show_databases)
* [Mongodb Use Database](#mongodb_use_database)
* [Mongodb Insert](#mongodb_insert)
* [Mongodb Find](#mongodb_find)
* [Mongodb Update](#mongodb_update)
* [Mongodb Delete](#mongodb_delete)

## Mongodb Docker
First of all we need mongodb installed somewhere. We are going to use docker to create a container with the mongodb.

```sh
#shell commands
docker pull mongo
docker container run --name mongo-starwars --publish 27017:27017 -d mongo
docker container exec -it mongo-starwars bash
mongo

#mongodb commands
use starwars;
db.characters.insertOne({"name":"R2D2","type":"Robot"});
```

## Mongodb Show Databases
We are going to show the first command of mongodb in order to list all the databases.

```sh
show dbs;
```

## Mongodb Use Database
The next command is used to change between databases

```sh
use starwars;
```

## Mongodb Insert
In mongodb we can do an atomic insert or a bulk insert

```sh
#insert one
db.characters.insertOne({"name":"Luke skywalker","type":"Jedi"});
db.characters.insertOne({"name":"Chewabacca","type":"Wookie"});

#insert many
db.characters.insertMany([{"name":"Dark Vader","type":"Sith"},{"name":"C3PO","type":"Robot"}]);
db.characters.find({});
```

Now we are going to do a bulk insertion with node. First of all we have to download/install mongodb in node with the following command shell:
```sh
npm install mongodb
```

After this we are prepared to execute our file "02_mongodb_insert_many.js"
```sh
$ node 02_mongodb_insert_many.js

Connected successfully to server
Inserted documents => {
  acknowledged: true,
  insertedCount: 6,
  insertedIds: {
    '0': new ObjectId("61ea126e16f5b389d8f981f9"),
    '1': new ObjectId("61ea126e16f5b389d8f981fa"),
    '2': new ObjectId("61ea126e16f5b389d8f981fb"),
    '3': new ObjectId("61ea126e16f5b389d8f981fc"),
    '4': new ObjectId("61ea126e16f5b389d8f981fd"),
    '5': new ObjectId("61ea126e16f5b389d8f981fe")
  }
}
done.
```

Now we have some characters and planets in our starwars database. We want to introduce some starships more.
```sh
db.starships.insertMany([
	{"name":"X-wing", side:"Republic", "size":{"width":11.7,"height":2.4,"length":13.4, "weight":10000}, "weapons":["laser","missiles"], "crew":1},
	{"name":"Tie-fighter", side:"Empire", "size":{"width":6.7,"height":8.8,"length":7.2, "weight":7500}, "weapons":["laser"], "crew":1},
	{"name":"Millenium Falcon", side:"Republic", "size":{"width":34,"height":7.8,"length":34.75, "weight":100000}, "weapons":["laser"], "crew":10},
	{"name":"Super Star Destroyer", side:"Empire", "size":{"width":60543,"height":3975,"length":13234, "weight":1000000000}, "weapons":["turbolaser","laser","missiles"], "crew":1000},
	{"name":"Y-wing", side:"Republic", "size":{"width":8.54,"height":2.44,"length":20.03, "weight":20000}, "weapons":["missiles"], "crew":2},
	{"name":"B-wing", side:"Republic", "size":{"width":2.9,"height":7.3,"length":16.9, "weight":20000}, "weapons":["laser","bombs"], "crew":2}
]);
```

## Mongodb Find
The command to select items in mongodb is find.

```sh
db.characters.find({});
db.characters.findOne({});
```

###	Specify Equality Condition
We are going to show how specify equality condition
```sh
# { <field1>: <value1>, ... }
db.characters.find({"type":"Robot"});
#output
{ "_id" : ObjectId("61eeb4d8f4f662cd82b5446f"), "name" : "R2D2", "type" : "Robot" }
{ "_id" : ObjectId("61eeb609f4f662cd82b54472"), "name" : "C3PO", "type" : "Robot" }
```

###	Specify Conditions Using Query Operators
We are going to show how specify condition using query operator
```sh
# { <field1>: { <operator1>: <value1> }, ... }
db.characters.find({"type":{"$in":["Robot","Wookie"]}});
#output
{ "_id" : ObjectId("61eeb4d8f4f662cd82b5446f"), "name" : "R2D2", "type" : "Robot" }
{ "_id" : ObjectId("61eeb5e94923339f09a84a49"), "name" : "Chewabacca", "type" : "Wookie" }
{ "_id" : ObjectId("61eeb609f4f662cd82b54472"), "name" : "C3PO", "type" : "Robot" }
```

###	Specify AND Conditions
We are going to show how specify AND condition
```sh
db.characters.find({"name":"R2D2","type":"Robot"});
#output
{ "_id" : ObjectId("61eeb4d8f4f662cd82b5446f"), "name" : "R2D2", "type" : "Robot" }
```

###	Specify OR Conditions
We are going to show how specify OR condition
```sh
db.characters.find({"$or":[{"name":"R2D2"},{"name":"C3PO"}]});
#output
{ "_id" : ObjectId("61eeb4d8f4f662cd82b5446f"), "name" : "R2D2", "type" : "Robot" }
{ "_id" : ObjectId("61eeb609f4f662cd82b54472"), "name" : "C3PO", "type" : "Robot" }
```

### Specify AND as well as OR Conditions
We are going to show how mix AND and OR Coditions
```sh
db.characters.find({"type":"Robot","$or":[{"name":"R2D2"},{"name":"C3PO"}]});
#output
{ "_id" : ObjectId("61eeb4d8f4f662cd82b5446f"), "name" : "R2D2", "type" : "Robot" }
{ "_id" : ObjectId("61eeb609f4f662cd82b54472"), "name" : "C3PO", "type" : "Robot" }
```

### Match an Embeded/Nested Document
We are going to show how to find a nested document
```sh
db.starships.find({"size":{"width":11.7,"height":2.4,"length":13.4, "weight":10000}});
#output
{ "_id" : ObjectId("61ef0d67f4f662cd82b54498"), "name" : "X-wing", "side" : "Republic", "size" : { "width" : 11.7, "height" : 2.4, "length" : 13.4, "weight" : 10000 }, "weapons" : [ "laser", "missiles" ], "crew" : 1 }
```

### Query on Nested Field
We are going to show how to find a nested field -> ("field.nestedField")
```sh
db.starships.find({"size.width":11.7});
#output
{ "_id" : ObjectId("61ef0d67f4f662cd82b54498"), "name" : "X-wing", "side" : "Republic", "size" : { "width" : 11.7, "height" : 2.4, "length" : 13.4, "weight" : 10000 }, "weapons" : [ "laser", "missiles" ], "crew" : 1 }
```

Another example can be adding an operator to find in the nested field
```sh
db.starships.find({"size.width":{"$gt":35}});
#output
{ "_id" : ObjectId("61ef0d67f4f662cd82b5449b"), "name" : "Super Star Destroyer", "side" : "Empire", "size" : { "width" : 60543, "height" : 3975, "length" : 13234, "weight" : 1000000000 }, "weapons" : [ "turbolaser", "laser", "missiles" ], "crew" : 1000 }
```

Specify AND condition with a nested field
```sh
db.starships.find({"name":"Super Star Destroyer","size.width":{"$gt":35}});
#output
{ "_id" : ObjectId("61ef0d67f4f662cd82b5449b"), "name" : "Super Star Destroyer", "side" : "Empire", "size" : { "width" : 60543, "height" : 3975, "length" : 13234, "weight" : 1000000000 }, "weapons" : [ "turbolaser", "laser", "missiles" ], "crew" : 1000 }
```

## Query an Array
Now we are going to learn how to query in mongodb fields with arrays:

Query: starship with weapons laser
```sh
db.starships.find({"weapons":["laser"]});
#output
{ "_id" : ObjectId("61ef0d67f4f662cd82b54499"), "name" : "Tie-fighter", "side" : "Empire", "size" : { "width" : 6.7, "height" : 8.8, "length" : 7.2, "weight" : 7500 }, "weapons" : [ "laser" ], "crew" : 1 }
{ "_id" : ObjectId("61ef0d67f4f662cd82b5449a"), "name" : "Millenium Falcon", "side" : "Republic", "size" : { "width" : 34, "height" : 7.8, "length" : 34.75, "weight" : 100000 }, "weapons" : [ "laser" ], "crew" : 10 }
```

Query: starship with weapons bombs
```sh
db.starships.find({"weapons":"bombs"});
#output
{ "_id" : ObjectId("61ef0d67f4f662cd82b5449d"), "name" : "B-wing", "side" : "Republic", "size" : { "width" : 2.9, "height" : 7.3, "length" : 16.9, "weight" : 20000 }, "weapons" : [ "laser", "bombs" ], "crew" : 2 }
```

Query: starship with weapons laser in position 0
```sh
db.starships.find({"weapons.0":"laser"});
#output
{ "_id" : ObjectId("61ef0d67f4f662cd82b54498"), "name" : "X-wing", "side" : "Republic", "size" : { "width" : 11.7, "height" : 2.4, "length" : 13.4, "weight" : 10000 }, "weapons" : [ "laser", "missiles" ], "crew" : 1 }
{ "_id" : ObjectId("61ef0d67f4f662cd82b54499"), "name" : "Tie-fighter", "side" : "Empire", "size" : { "width" : 6.7, "height" : 8.8, "length" : 7.2, "weight" : 7500 }, "weapons" : [ "laser" ], "crew" : 1 }
{ "_id" : ObjectId("61ef0d67f4f662cd82b5449a"), "name" : "Millenium Falcon", "side" : "Republic", "size" : { "width" : 34, "height" : 7.8, "length" : 34.75, "weight" : 100000 }, "weapons" : [ "laser" ], "crew" : 10 }
{ "_id" : ObjectId("61ef0d67f4f662cd82b5449d"), "name" : "B-wing", "side" : "Republic", "size" : { "width" : 2.9, "height" : 7.3, "length" : 16.9, "weight" : 20000 }, "weapons" : [ "laser", "bombs" ], "crew" : 2 }
```

Query: starship with 3 weapons
```sh
db.starships.find({"weapons":{"$size":3}});
#output
{ "_id" : ObjectId("61ef0d67f4f662cd82b5449b"), "name" : "Super Star Destroyer", "side" : "Empire", "size" : { "width" : 60543, "height" : 3975, "length" : 13234, "weight" : 1000000000 }, "weapons" : [ "turbolaser", "laser", "missiles" ], "crew" : 1000 }
```

Also we can query in this way
```sh
#this query can't be executed in starwars database
db.inventory.find( { "dim_cm": { "$gt": 25 } } )
db.inventory.find( { "dim_cm": { "$gt": 15, "$lt": 20 } } )
db.inventory.find( { "dim_cm": { "$elemMatch": { "$gt": 22, "$lt": 30 } } } )
```

## Project fields in mongodb
If you only want a part of the output document you have to select the fields that you want and this is called project in mongodb.
```sh
db.starships.find({"weapons":{"$size":3}},{"_id":0,"name":1});
#output
{ "name" : "Super Star Destroyer" }
```

## Query for Null or Missing Fields
We are going to query to null elements o missing fields

Query Equality Filter:
```sh
db.planets.find({"capital_city":null});
#output
{ "_id" : ObjectId("61ef1391f4f662cd82b5449e"), "name" : "Dagobah", "capital_city" : null }
{ "_id" : ObjectId("61ef1391f4f662cd82b544a0"), "name" : "Exegol", "capital_city" : null }
{ "_id" : ObjectId("61ef1391f4f662cd82b544a3"), "name" : "Tatooine" }
```

Query Type Check
```sh
db.planets.find({"capital_city":{"$type": 10}});
#output
{ "_id" : ObjectId("61ef1391f4f662cd82b5449e"), "name" : "Dagobah", "capital_city" : null }
{ "_id" : ObjectId("61ef1391f4f662cd82b544a0"), "name" : "Exegol", "capital_city" : null }
```

Query Existence Check:
```sh
db.planets.find({"capital_city":{"$exists":false}});
#output
{ "_id" : ObjectId("61ef1391f4f662cd82b544a3"), "name" : "Tatooine" }

db.planets.find({"capital_city":{"$exists":true}});
#output
{ "_id" : ObjectId("61ef1391f4f662cd82b5449e"), "name" : "Dagobah", "capital_city" : null }
{ "_id" : ObjectId("61ef1391f4f662cd82b5449f"), "name" : "Corrusant", "capital_city" : "Galactic city" }
{ "_id" : ObjectId("61ef1391f4f662cd82b544a0"), "name" : "Exegol", "capital_city" : null }
{ "_id" : ObjectId("61ef1391f4f662cd82b544a1"), "name" : "Kamino", "capital_city" : "Tipoca city" }
{ "_id" : ObjectId("61ef1391f4f662cd82b544a2"), "name" : "Naboo", "capital_city" : "Theed" }
```

## Mongodb Update
First of all we saw the insert operation, in second place find and now we are going to explain how to update fields in a document.

We have three kind of operators:
```sh
db.collection.updateOne(<filter>, <update>, <options>)
db.collection.updateMany(<filter>, <update>, <options>)
db.collection.replaceOne(<filter>, <update>, <options>)
```

Query to update the city capital of the planet Tatooine:
```sh
db.planets.find({"name":"Tatooine"});
#output
{ "_id" : ObjectId("61ef193cf4f662cd82b544a9"), "name" : "Tatooine" }

#update
db.planets.updateOne(
	{"name":"Tatooine"},
	{
		"$set":{"capital_city":"Bestine"},
		"$currentDate":{"lastmodified":true}
	}
);
#output
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }

db.planets.find({"name":"Tatooine"});
#output
{ "_id" : ObjectId("61ef1391f4f662cd82b544a3"), "name" : "Tatooine", "capital_city" : "Bestine", "lastmodified" : ISODate("2022-01-24T21:22:41.772Z") }
```

Query to update all documents at the same time
```sh
db.planets.find({});
#output
{ "_id" : ObjectId("61ef193cf4f662cd82b544a4"), "name" : "Dagobah", "capital_city" : null }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a5"), "name" : "Corrusant", "capital_city" : "Galactic city" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a6"), "name" : "Exegol", "capital_city" : null }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a7"), "name" : "Kamino", "capital_city" : "Tipoca city" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a8"), "name" : "Naboo", "capital_city" : "Theed" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a9"), "name" : "Tatooine", "capital_city" : "Bestine", "lastmodified" : ISODate("2022-01-24T21:25:39.682Z") }

#update
db.planets.updateMany(
	{},
	{
		"$set":{"type":"Planet"},
		"$currentDate":{"lastmodified":true}
	}
);
#output
{ "acknowledged" : true, "matchedCount" : 6, "modifiedCount" : 6 }

db.planets.find({});
#output
{ "_id" : ObjectId("61ef193cf4f662cd82b544a4"), "name" : "Dagobah", "capital_city" : null, "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a5"), "name" : "Corrusant", "capital_city" : "Galactic city", "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a6"), "name" : "Exegol", "capital_city" : null, "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a7"), "name" : "Kamino", "capital_city" : "Tipoca city", "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a8"), "name" : "Naboo", "capital_city" : "Theed", "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a9"), "name" : "Tatooine", "capital_city" : "Bestine", "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }
```

## Mongodb Delete
Now we are going to explain the last CRUD command, the "delete".

We have two kind of operators:
```sh
db.collection.deleteMany()
db.collection.deleteOne()
```

Query to delete the planet "Tatooine"
```sh
db.planets.find({"name":"Tatooine"});
#output
{ "_id" : ObjectId("61ef193cf4f662cd82b544a9"), "name" : "Tatooine", "capital_city" : "Bestine", "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }

#delete Tatooine
db.planets.deleteOne({"name":"Tatooine"});
#output
{ "acknowledged" : true, "deletedCount" : 1 }

#list all planets
db.planets.find({});
#output
{ "_id" : ObjectId("61ef193cf4f662cd82b544a4"), "name" : "Dagobah", "capital_city" : null, "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a5"), "name" : "Corrusant", "capital_city" : "Galactic city", "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a6"), "name" : "Exegol", "capital_city" : null, "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a7"), "name" : "Kamino", "capital_city" : "Tipoca city", "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }
{ "_id" : ObjectId("61ef193cf4f662cd82b544a8"), "name" : "Naboo", "capital_city" : "Theed", "lastmodified" : ISODate("2022-01-24T21:27:48.002Z"), "type" : "Planet" }

#delete all planets
db.planets.deleteMany({});
{ "acknowledged" : true, "deletedCount" : 5 }
#list all planets
db.planets.find({});
#output nothing
```