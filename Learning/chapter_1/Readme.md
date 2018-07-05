# MongoDb Quick Tutorial

## Loading Documents to database stored as js file
* load ./data/loadMovieDetailsDataset.js this file
* ```load("loadMovieDetailsDataset.js")```
    
### See all database
* ```show dbs```

### switch to use given db
* ```use database_name```
* then use ```db.collection_name.whatever_you_wanna_do()```

## CRUD Operations
* C : Create
* R : Read
* U :Update
* D :Delete

## _id 
If i dont specify **_id** while insert it created automaticaly of type objectID and it is always unique.<br />
you can specify **_id** manually ,just make sure it is unique.
```
db.moviesScrap.insertOne({_id:"23432dsfdg","title":"2012","year":2012})
```
make sure for all document **_id** have same data type for convenient and making MondoDb more usable but if you dont it won't through and error.

## Insert one Document
* ```db.moviesScrap.insertOne({"title":"Avengers","year":2010})```
* it will return a Document if insert successfully
```  
    {
	"acknowledged" : true,
	"insertedId" : ObjectId("5b3e3e9642c6e9b5f049295b")
    }
```
 * true :states insertion is successful.
 * insertedID is basically value of **_id** field for the new document.
 
 # Insert many ordered and unOrdered
 ### Ordered
 * If any error occure while inserting kth document then documents after kth documents will not insert.
 * it is by default mode.
 ```buildoutcfg
db.moviesScratch.insertMany(
    [
        {
  	    "_id" : "tt0084726",
  	    "title" : "Star Trek II: The Wrath of Khan",
  	    "year" : 1982,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt0796366",
  	    "title" : "Star Trek",
  	    "year" : 2009,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt0084726",
  	    "title" : "Star Trek II: The Wrath of Khan",
  	    "year" : 1982,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt1408101",
  	    "title" : "Star Trek Into Darkness",
  	    "year" : 2013,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt0117731",
  	    "title" : "Star Trek: First Contact",
  	    "year" : 1996,
  	    "type" : "movie"
        }
    ]
);

```
here Duplicate key error
 ### Un-ordered Insert
 *  in Un-ordered insert if kth document insert failed even though it will insert all other documents even the documents comes after document that cause error. 
 * two make insertMany unordered add new document
 ```buildoutcfg
    {
        "ordered": false 
    }
```

* complete Query Example
```buildoutcfg
db.moviesScratch.insertMany(
    [
        {
	    "_id" : "tt0084726",
	    "title" : "Star Trek II: The Wrath of Khan",
	    "year" : 1982,
	    "type" : "movie"
        },
        {
	    "_id" : "tt0796366",
	    "title" : "Star Trek",
	    "year" : 2009,
	    "type" : "movie"
        },
        {
	    "_id" : "tt0084726",
	    "title" : "Star Trek II: The Wrath of Khan",
	    "year" : 1982,
	    "type" : "movie"
        },
        {
	    "_id" : "tt1408101",
	    "title" : "Star Trek Into Darkness",
	    "year" : 2013,
	    "type" : "movie"
        },
        {
	    "_id" : "tt0117731",
	    "title" : "Star Trek: First Contact",
	    "year" : 1996,
	    "type" : "movie"
        }
    ],
    {
        "ordered": false 
    }
);
```
it also throw ***BulkWriteError*** or **writeErrors** for document which does not get inserted due to error


# Query Over Scalar Data
* Scalar data is values like int string not a jason object or array

## and query
```db.movies.find({"mpaaRating":"PG-13","year":2009})```
* where rating is PG-13 **and** year is 2009


### Query on nested field
* we can use dot operator for going in hierarchy 
```db.data.find({'wind.speed.rate':1})```

## Matching a Array
* Match Exactly array 
```db.movies.find({cast:["Jeff Bridges", "Tim Robbins"]})```
* Search "Jeff Bridges" is in list of cast.
```db.movies.find({'cast':"Jeff Bridges"})```
* Search "Jeff Bridges" is at 0th index of array
```db.movies.find({'cast.0':"Jeff Bridges"}).pretty()```
* ```pretty()``` is function to formate result for better readability .

# Cursor
* Mongodb ```find()``` function return a cursor.
* MongoDB return result in batches so it is like lazy loading by default batch size is 20.

# Projection
* Projection used to limit output based on requirement like if we only want movies name for my query.
* projection will be specify in second argument of find method. If we include any field then by default all other fields are excluded except _id.

```db.movieDetails.find({'genres.1':"Western"},{"title":1})```
<br />
It only return title and _id (it is by default)
* we can avoid **_id** by explicitly by making is 0 in projection.

```db.movieDetails.find({'genres.1':"Western"},{"title":1,"_id":0})```

* If we want all field except title and _id field
```python
db.movieDetails.find({'genres.1':"Western"},{"title":0,"_id":0})
```

# Update one
* FIRST argument is filter
* second argument is update operator ```$set```, it takes a document as input.
```python
db.movieDetails.updateOne({
     "title":"The Martian"},
    {
     $set : {poster : "http://posterLinl.com"}
  })
```

* Output : for update query
```
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
```

* [There are many other update operator you can see them here **link**](https://docs.mongodb.com/manual/reference/operator/update/)
