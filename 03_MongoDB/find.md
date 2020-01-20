### Create a new DB 
```
use books
```
check your current db:
```
> db
books
```
### Create a new collection
```
db.createCollection("countries")
```
check that the collection was added:
```
> db.getCollectionNames()
[ "countries" ]
```
### Insert new documents to "countries" collection
```
db.countries.insert(
    [
        {"name":"Israel","callingCodes":["972"],"population":8527400,"borders":["EGY","JOR","LBN","SYR"]},
        {"name":"Germany","callingCodes":["49"],"population":81770900,"borders":["AUT","BEL","CZE","DNK","FRA","LUX","NLD","POL","CHE"]},
        {"name":"Brazil","callingCodes":["55"],"population":206135893,"borders":["ARG","BOL","COL","GUF","GUY","PRY","PER","SUR","URY","VEN"]},
        {"name":"Spain","callingCodes":["34"],"population":46438422,"borders":["AND","FRA","GIB","PRT","MAR"]},
        {"name":"France","callingCodes":["33"],"population":66710000,"borders":["AND","BEL","DEU","ITA","LUX","MCO","ESP","CHE"]},
        {"name":"China","callingCodes":["86"],"population":1377422166,"borders":["AFG","BTN","MMR","HKG","IND","KAZ","PRK","KGZ","LAO","MAC","MNG","PAK","RUS","TJK","VNM"]}
    ]
)
```
the output is:
```
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 6,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
```
### Select all documents from "countries" collection
```
```
the result is:
```
```