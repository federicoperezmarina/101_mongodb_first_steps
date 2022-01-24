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
	{"name":"X-wing", "size":"small", "weapons":["laser","missiles"], "crew":1},
	{"name":"Tie-fighter", "size":"small", "weapons":["laser"], "crew":1},
	{"name":"Millenium Falcon", "size":"medium", "weapons":["laser"], "crew":10},
	{"name":"Super Star Destroyer", "size":"big", "weapons":["turbolaser","laser","missiles"], "crew":1000},
	{"name":"Y-wing", "size":"small", "weapons":["missiles"], "crew":2},
	{"name":"B-wing", "size":"small", "weapons":["laser","bombs"], "crew":2},
	{"name":"Slave I", "size":"small", "weapons":["laser","missiles"], "crew":2},
	{"name":"Jedi Starfighter", "size":"small", "weapons":["laser"], "crew":1},
	{"name":"Lambda-class T-4a Shuttle", "size":"medium", "crew":10},
	{"name":"N-1 Naboo Starfighter", "size":"small", "weapons":["laser"], "crew":1},
	{"name":"Solar Sailer", "size":"small", "weapons":["laser"], "crew":1},
	{"name":"Hammerhead-class Cruiser", "size":"medium", "weapons":["turbolaser","missiles"], "crew":25}
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
```

###	Specify Conditions Using Query Operators
We are going to show how specify condition using query operator
```sh
# { <field1>: { <operator1>: <value1> }, ... }
db.characters.find({"type":{"$in":["Robot","Wookie"]}});
```

###	Specify AND Conditions
We are going to show how specify AND condition
```sh
db.characters.find({"name":"R2D2","type":"Robot"});
```

###	Specify OR Conditions
We are going to show how specify OR condition
```sh
db.characters.find({"$or":[{"name":"R2D2"},{"name":"C3PO"}]});
```

### Specify AND as well as OR Conditions
We are going to show how mix AND and OR Coditions
```sh
db.characters.find({"type":"Robot","$or":[{"name":"R2D2"},{"name":"C3PO"}]});
```