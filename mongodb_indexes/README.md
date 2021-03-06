# Mongodb indexes
In this section we are going to talk about indexes in mongodb. We are going to review two kind of indexes "single field indexes" and "compound indexes"

## Table of Contents
* [Mongodb Single Field Indexes](#mongodb_single_field_indexes)
* [Mongodb Compound Indexes](#mongodb_compound_indexes)


## Mongodb Single Field Indexes
In order to understand better how an index works we will introduce also de command "explain" to see the execution plan of a query.

```sh
db.characters.find({"name":"R2D2"});
#output
{ "_id" : ObjectId("61eeb4d8f4f662cd82b5446f"), "name" : "R2D2", "type" : "Robot" }

#now we are going to use the explain
db.characters.explain().find({"name":"R2D2"});
#output
{
	"explainVersion" : "1",
	"queryPlanner" : {
		"namespace" : "starwars.characters",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"name" : {
				"$eq" : "R2D2"
			}
		},
		"queryHash" : "01AEE5EC",
		"planCacheKey" : "C04BB947",
		"maxIndexedOrSolutionsReached" : false,
		"maxIndexedAndSolutionsReached" : false,
		"maxScansToExplodeReached" : false,
		"winningPlan" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"name" : {
					"$eq" : "R2D2"
				}
			},
			"direction" : "forward"
		},
		"rejectedPlans" : [ ]
	},
	"command" : {
		"find" : "characters",
		"filter" : {
			"name" : "R2D2"
		},
		"$db" : "starwars"
	},
	"serverInfo" : {
		"host" : "d9316188277b",
		"port" : 27017,
		"version" : "5.0.5",
		"gitVersion" : "d65fd89df3fc039b5c55933c0f71d647a54510ae"
	},
	"serverParameters" : {
		"internalQueryFacetBufferSizeBytes" : 104857600,
		"internalQueryFacetMaxOutputDocSizeBytes" : 104857600,
		"internalLookupStageIntermediateDocumentMaxSizeBytes" : 104857600,
		"internalDocumentSourceGroupMaxMemoryBytes" : 104857600,
		"internalQueryMaxBlockingSortMemoryUsageBytes" : 104857600,
		"internalQueryProhibitBlockingMergeOnMongoS" : 0,
		"internalQueryMaxAddToSetBytes" : 104857600,
		"internalDocumentSourceSetWindowFieldsMaxMemoryBytes" : 104857600
	},
	"ok" : 1
}

#create single index
db.characters.createIndex({"name":1});
#output
{
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"createdCollectionAutomatically" : false,
	"ok" : 1
}

#what happen if we do the explain again
db.characters.explain().find({"name":"R2D2"});
#output
{
	"explainVersion" : "1",
	"queryPlanner" : {
		"namespace" : "starwars.characters",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"name" : {
				"$eq" : "R2D2"
			}
		},
		"queryHash" : "01AEE5EC",
		"planCacheKey" : "5AF8B629",
		"maxIndexedOrSolutionsReached" : false,
		"maxIndexedAndSolutionsReached" : false,
		"maxScansToExplodeReached" : false,
		"winningPlan" : {
			"stage" : "FETCH",
			"inputStage" : {
				"stage" : "IXSCAN",
				"keyPattern" : {
					"name" : 1
				},
				"indexName" : "name_1",
				"isMultiKey" : false,
				"multiKeyPaths" : {
					"name" : [ ]
				},
				"isUnique" : false,
				"isSparse" : false,
				"isPartial" : false,
				"indexVersion" : 2,
				"direction" : "forward",
				"indexBounds" : {
					"name" : [
						"[\"R2D2\", \"R2D2\"]"
					]
				}
			}
		},
		"rejectedPlans" : [ ]
	},
	"command" : {
		"find" : "characters",
		"filter" : {
			"name" : "R2D2"
		},
		"$db" : "starwars"
	},
	"serverInfo" : {
		"host" : "d9316188277b",
		"port" : 27017,
		"version" : "5.0.5",
		"gitVersion" : "d65fd89df3fc039b5c55933c0f71d647a54510ae"
	},
	"serverParameters" : {
		"internalQueryFacetBufferSizeBytes" : 104857600,
		"internalQueryFacetMaxOutputDocSizeBytes" : 104857600,
		"internalLookupStageIntermediateDocumentMaxSizeBytes" : 104857600,
		"internalDocumentSourceGroupMaxMemoryBytes" : 104857600,
		"internalQueryMaxBlockingSortMemoryUsageBytes" : 104857600,
		"internalQueryProhibitBlockingMergeOnMongoS" : 0,
		"internalQueryMaxAddToSetBytes" : 104857600,
		"internalDocumentSourceSetWindowFieldsMaxMemoryBytes" : 104857600
	},
	"ok" : 1
}
```



## Mongodb Compound Indexes
In this section we will learn how to create a compound index

```sh
db.starships.find({"name":"X-wing","side":"Republic"});
#output
{ "_id" : ObjectId("61ef0d67f4f662cd82b54498"), "name" : "X-wing", "side" : "Republic", "size" : { "width" : 11.7, "height" : 2.4, "length" : 13.4, "weight" : 10000 }, "weapons" : [ "laser", "missiles" ], "crew" : 1 }

#now we are going to use the explain
db.starships.explain().find({"name":"X-wing","side":"Republic"});
#output
{
	"explainVersion" : "1",
	"queryPlanner" : {
		"namespace" : "starwars.starships",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"$and" : [
				{
					"name" : {
						"$eq" : "X-wing"
					}
				},
				{
					"side" : {
						"$eq" : "Republic"
					}
				}
			]
		},
		"queryHash" : "0C1248AB",
		"planCacheKey" : "DAB24D40",
		"maxIndexedOrSolutionsReached" : false,
		"maxIndexedAndSolutionsReached" : false,
		"maxScansToExplodeReached" : false,
		"winningPlan" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"$and" : [
					{
						"name" : {
							"$eq" : "X-wing"
						}
					},
					{
						"side" : {
							"$eq" : "Republic"
						}
					}
				]
			},
			"direction" : "forward"
		},
		"rejectedPlans" : [ ]
	},
	"command" : {
		"find" : "starships",
		"filter" : {
			"name" : "X-wing",
			"side" : "Republic"
		},
		"$db" : "starwars"
	},
	"serverInfo" : {
		"host" : "d9316188277b",
		"port" : 27017,
		"version" : "5.0.5",
		"gitVersion" : "d65fd89df3fc039b5c55933c0f71d647a54510ae"
	},
	"serverParameters" : {
		"internalQueryFacetBufferSizeBytes" : 104857600,
		"internalQueryFacetMaxOutputDocSizeBytes" : 104857600,
		"internalLookupStageIntermediateDocumentMaxSizeBytes" : 104857600,
		"internalDocumentSourceGroupMaxMemoryBytes" : 104857600,
		"internalQueryMaxBlockingSortMemoryUsageBytes" : 104857600,
		"internalQueryProhibitBlockingMergeOnMongoS" : 0,
		"internalQueryMaxAddToSetBytes" : 104857600,
		"internalDocumentSourceSetWindowFieldsMaxMemoryBytes" : 104857600
	},
	"ok" : 1
}

#create single index
db.starships.createIndex({"side":-1,"name":1});
#output
{
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"createdCollectionAutomatically" : false,
	"ok" : 1
}

#what happen if we do the explain again
db.starships.explain().find({"name":"X-wing","side":"Republic"});
#output
{
	"explainVersion" : "1",
	"queryPlanner" : {
		"namespace" : "starwars.starships",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"$and" : [
				{
					"name" : {
						"$eq" : "X-wing"
					}
				},
				{
					"side" : {
						"$eq" : "Republic"
					}
				}
			]
		},
		"queryHash" : "0C1248AB",
		"planCacheKey" : "23CC9BF6",
		"maxIndexedOrSolutionsReached" : false,
		"maxIndexedAndSolutionsReached" : false,
		"maxScansToExplodeReached" : false,
		"winningPlan" : {
			"stage" : "FETCH",
			"inputStage" : {
				"stage" : "IXSCAN",
				"keyPattern" : {
					"side" : -1,
					"name" : 1
				},
				"indexName" : "side_-1_name_1",
				"isMultiKey" : false,
				"multiKeyPaths" : {
					"side" : [ ],
					"name" : [ ]
				},
				"isUnique" : false,
				"isSparse" : false,
				"isPartial" : false,
				"indexVersion" : 2,
				"direction" : "forward",
				"indexBounds" : {
					"side" : [
						"[\"Republic\", \"Republic\"]"
					],
					"name" : [
						"[\"X-wing\", \"X-wing\"]"
					]
				}
			}
		},
		"rejectedPlans" : [ ]
	},
	"command" : {
		"find" : "starships",
		"filter" : {
			"name" : "X-wing",
			"side" : "Republic"
		},
		"$db" : "starwars"
	},
	"serverInfo" : {
		"host" : "d9316188277b",
		"port" : 27017,
		"version" : "5.0.5",
		"gitVersion" : "d65fd89df3fc039b5c55933c0f71d647a54510ae"
	},
	"serverParameters" : {
		"internalQueryFacetBufferSizeBytes" : 104857600,
		"internalQueryFacetMaxOutputDocSizeBytes" : 104857600,
		"internalLookupStageIntermediateDocumentMaxSizeBytes" : 104857600,
		"internalDocumentSourceGroupMaxMemoryBytes" : 104857600,
		"internalQueryMaxBlockingSortMemoryUsageBytes" : 104857600,
		"internalQueryProhibitBlockingMergeOnMongoS" : 0,
		"internalQueryMaxAddToSetBytes" : 104857600,
		"internalDocumentSourceSetWindowFieldsMaxMemoryBytes" : 104857600
	},
	"ok" : 1
}
```
