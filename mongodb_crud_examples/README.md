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
docker container run --name mongo-starwars --publish 27017:27017 -d mongo
docker container exec -it mongo-starwars bash
mongo
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


## Mongodb Find
The command to select items in mongodb is find.

```sh
db.characters.find({});
db.characters.findOne({});
```