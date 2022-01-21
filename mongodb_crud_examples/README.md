# Mongodb CRUD
In this chapter we are going tu see how to understand and practice the CRUD operations like create, read, update and delete

## Table of Contents
* [Mongodb Show Databases](#mongodb_show_databases)
* [Mongodb Use Database](#mongodb_use_database)
* [Mongodb Insert](#mongodb_insert)
* [Mongodb Find](#mongodb_find)
* [Mongodb Update](#mongodb_update)
* [Mongodb Delete](#mongodb_delete)

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
db.characters.insertOne({"name":"luke skywalker","type":"Jedi"});
db.characters.insertOne({"name":"Chewabacca","type":"Wookie"});

#insert many
db.characters.insertMany([{"name":"Dark Vader","type":"Sith"},{"name":"C3PO","type":"Robot"}])
db.characters.find({});
```

## Mongodb Find
The command to select items in mongodb is find.

```sh
db.characters.find({})
db.characters.findOne({});
```