# Mongodb Aggregation
In this section we are going to explain how the aggregation framework of mongodb works. The aggregation consists in a pipeline that has one or more stages that process documents.

Also all the official documentation of the aggregation framework is [here](https://docs.mongodb.com/manual/meta/aggregation-quick-reference/)

## Table of Contents
* [Mongodb Match Stage](#mongodb_match_stage)
* [Mongodb Group Stage](#mongodb_group_stage)
* [Mongodb Project Stage](#mongodb_project_stage)
* [Mongodb Sort Stage](#mongodb_sort_stage)
* [Mongodb Limit Stage](#mongodb_limit_stage)
* [Mongodb Skip Stage](#mongodb_skip_stage)

## Mongodb Match Stage
Now we are going to show how to add an stage to an aggregation.

```sh
db.starships.aggregate([
	{
		"$match": {"side":"Republic"}
	}
]);
#output
{ "_id" : ObjectId("61ef0d67f4f662cd82b5449d"), "name" : "B-wing", "side" : "Republic", "size" : { "width" : 2.9, "height" : 7.3, "length" : 16.9, "weight" : 20000 }, "weapons" : [ "laser", "bombs" ], "crew" : 2 }
{ "_id" : ObjectId("61ef0d67f4f662cd82b5449a"), "name" : "Millenium Falcon", "side" : "Republic", "size" : { "width" : 34, "height" : 7.8, "length" : 34.75, "weight" : 100000 }, "weapons" : [ "laser" ], "crew" : 10 }
{ "_id" : ObjectId("61ef0d67f4f662cd82b54498"), "name" : "X-wing", "side" : "Republic", "size" : { "width" : 11.7, "height" : 2.4, "length" : 13.4, "weight" : 10000 }, "weapons" : [ "laser", "missiles" ], "crew" : 1 }
{ "_id" : ObjectId("61ef0d67f4f662cd82b5449c"), "name" : "Y-wing", "side" : "Republic", "size" : { "width" : 8.54, "height" : 2.44, "length" : 20.03, "weight" : 20000 }, "weapons" : [ "missiles" ], "crew" : 2 }
```

This is a simple aggregation of one step.


## Mongodb Group Stage
In this new stage of a pipeline we are going to group some information. We are going to use the first step in order to filter the starships of the republic and we want to know how many crew can carry all these 4 ships.

```sh
db.starships.aggregate([
	{
		"$match": {
			"side":"Republic"
		}
	},
	{
		"$group":{
			"_id": "$side",
		  	"total_crew": {
		    	"$sum": "$crew"
		  	}
		}
	}
]);
#output
{ "_id" : "Republic", "total_crew" : 15 }
```

## Mongodb Project Stage
The project stage permits us to filter the information and this gives performance to the aggregation

```sh
db.starships.aggregate([
	{
		"$project":{
			"side" : 1,
			"crew":1
		}
	},
	{
		"$match": {
			"side":{
				"$in":["Republic","Empire"]
			}
		}
	},
	{
		"$group":{
			"side": "$side",
		  	"total_crew": {
		    	"$sum": "$crew"
		  	}
		}
	}
]);
#output
{ "_id" : "Empire", "total_crew" : 1001 }
{ "_id" : "Republic", "total_crew" : 15 }
```

## Mongodb Sort Stage
Now we are going to sort the result of the last query in order to see first the "Republic"

```sh
db.starships.aggregate([
	{
		"$project":{
			"side" : 1,
			"crew":1
		}
	},
	{
		"$match": {
			"side":{
				"$in":["Republic","Empire"]
			}
		}
	},
	{
		"$group":{
			"_id": "$side",
		  	"total_crew": {
		    	"$sum": "$crew"
		  	}
		}
	},
	{
		"$sort":{
			"_id" : -1
		}
	}
]);
#output
... ]);
{ "_id" : "Republic", "total_crew" : 15 }
{ "_id" : "Empire", "total_crew" : 1001 }
```

## Mongodb Limit Stage
The limit stage give us the possibility of getting a limited items

```sh
db.starships.aggregate([
	{
		"$project":{
			"side" : 1,
			"crew":1
		}
	},
	{
		"$match": {
			"side":{
				"$in":["Republic","Empire"]
			}
		}
	},
	{
		"$group":{
			"_id": "$side",
		  	"total_crew": {
		    	"$sum": "$crew"
		  	}
		}
	},
	{
		"$sort":{
			"_id" : -1
		}
	},
	{
		"$limit":1
	}
]);
#output
{ "_id" : "Republic", "total_crew" : 15 }
```

## Mongodb Skip Stage
The Skip stage enables us to skip a number of documents in order to select the items wanted

```sh
db.starships.aggregate([
	{
		"$project":{
			"side" : 1,
			"crew":1
		}
	},
	{
		"$match": {
			"side":{
				"$in":["Republic","Empire"]
			}
		}
	},
	{
		"$group":{
			"_id": "$side",
		  	"total_crew": {
		    	"$sum": "$crew"
		  	}
		}
	},
	{
		"$sort":{
			"_id" : -1
		}
	},
	{
		"$skip":1
	},	
	{
		"$limit":1
	}
]);
#output
{ "_id" : "Empire", "total_crew" : 1001 }
```

There are a lot of more stages operators you can find them [here](https://docs.mongodb.com/manual/reference/operator/aggregation-pipeline/#std-label-aggregation-pipeline-operator-reference)
