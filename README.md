# j-db-api
j-db-api is a Restful webservices built on top of [j-db core engine] (https://github.com/m-sparrow/j-db)

## Syntax

###### Create DB
Create new DB using the below
```
POST http://localhost:8000/jdb/db/create

Request Body:
{
  "db": "my-db-1"
}

#dbName - String    #Ex: MyDB

```

###### Fetch list of DBs
This method returns the list of databases.
```
GET http://localhost:8000/jdb/db/list

Request Body:
--
```

###### Create Table
This methos is used to create a new table.
```
POST http://localhost:8000/jdb/table/create

Request Body:
{
  "db": "my-db-1", 
  "table": "my-table-1", 
  "schema": {
     "pk": "pk",
     "sk": {"0": "sk1", "1": "sk2"}
  }
}

#dbName - String        # Ex: MyDB
#tableName - String     # Ex: MyTable
#schema - JSON
```
The value of "pk" must be a String while "sk" must be a map with keys as Integers starting from 0 .... n and values as String.

###### Fetch list of tables
This method returns the list of tables in the given DB
```
GET http://localhost:8000/jdb/table/list?db=my-db-1

Request Body:
--
#dbName - String        # Ex: MyDB
```

###### Scan table keys
This method is used to scan the list of partition keys and secondary partition keys in the given table.
```
GET http://localhost:8000/jdb/table/scan-keys
Request Body:
{
  "db": "my-db-1", 
  "table": "my-table-1", 
  "keys": "id"
}

#dbName - String        # Ex: MyDB
#tableName - String     # Ex: MyTable
#keys - String          # Ex: pk1?sk1?....?skn

```
keys acts a cursor delimited with "?". This method returns "pk" or the list of "sk"s as per the given cursor.

###### Fetch Table metadata
This method is used to get the metadata of the given table.
```
GET http://localhost:8000/jdb/table/metadata

Request Body:

#dbName - String        # Ex: MyDB
#tableName - String     # Ex: MyTable
```

###### Table - Put Item
This method is used to create new Item in the table.
```
http://localhost:8000/jdb/table/put-item

Request Body:
{
  "db": "my-db-1", 
  "table": "my-table-1", 
  "options": {
     "keys":{
        "id" : "pk", 
        "sk2":"sk2-2-value", 
        "sk1" : "sk1-1-value"
     }, 
     "item":{
       "Key1":"Value1", 
       "Key2": "Value2"
    }
  }
}

#dbName - String        # Ex: MyDB
#tableName - String     # Ex: MyTable
#options - JSON
```
Keys (Partition Key and all Secondary Partition Keys) are mandatory. Item is mandatory and can be any valid JSON document.

###### Table - Get Item
This method is used to fetch an Item from the table.
```
GET http://localhost:8000/jdb/table/get-item

Request Body:
{
  "db": "my-db-1", 
  "table": "my-table-1", 
  "options": {
     "keys":{
     	"id" : "pk", 
	"sk2":"sk2-2-value", 
	"sk1" : "sk1-1-value"
     }
  }
}

#dbName - String        # Ex: MyDB
#tableName - String     # Ex: MyTable
#options - JSON
```
Keys (Partition Key and all Secondary Partition Keys) are mandatory.

###### Table - Update Item
This method is used to update an existing Item in the table.
```
POST http://localhost:8000/jdb/table/update-item

Request Body:
{
  "db": "my-db-1",
  "table": "my-table-1",
  "options": {
      "keys": {
          "id": "id1",
          "sk2": "sk2-2-value",
          "sk1": "sk1-1-value"
      },
      "path": "attr1?attr2?attr3#2?attr4",
      "obj": "value-to-be-updated"
  }
}
```
Keys (Partition Key and all Secondary Partition Keys) are mandatory. Path and Obj is also mandatory. "?" is used as a delimiter to traverse through the JSON object while "#" is used to specify the element index if it is an array.

###### Table - Delete Item
This method is used to delete an Item in the table.
```
POST http://localhost:8000/jdb/table/delete-item

Request Body:
{
  "db": "my-db-1",
  "table": "my-table-1",
  "options": {
      "keys": {
         "id": "id1",
         "sk2": "sk2-2-value",
         "sk1": "sk1-1-value"
      }
  }
}

#dbName - String        # Ex: MyDB
#tableName - String     # Ex: MyTable
#options - JSON
```
Keys (Partition Key and all Secondary Partition Keys) are mandatory.

###### Table - Add an Element to an existing Item
This method is used a new attribute or array value to an existing Item in the table.
```
POST http://localhost:8000/jdb/table/add-item-element

Request Body:
{
    "db": "my-db-1",
    "table": "my-table-1",
    "options": {
        "keys": {
           "id": "id1",
           "sk2": "sk2-2-value",
           "sk1": "sk1-1-value"
        },
        "path": "attr1?attr2?attr3#2?attr4",
        "tag": "tag",
        "obj": "value-to-be-added"
    }
}
#dbName - String        # Ex: MyDB
#tableName - String     # Ex: MyTable
#options - JSON
```
Keys (Partition Key and all Secondary Partition Keys) are mandatory. Path and Obj is also mandatory. "?" is used as a delimiter to traverse through the JSON object while "#" is used to specify the element index if it is an array. Tag is mandatory if the new element is an object (key-value) and optional if the new element is added to an array.

###### Table - Remove an Element from an exiting Item
This method is used to remove an attribute or array value from an existing Item in the table.
```
POST http://localhost:8000/jdb/table/remove-item-element

Request Body:
{
    "db": "my-db-1",
    "table": "my-table-1",
    "options": {
        "keys": {
           "id": "id1",
           "sk2": "sk2-2-value",
           "sk1": "sk1-1-value"
        },
        "path": "attr1?attr2?attr3#2?attr4"
    }
}

#dbName - String        # Ex: MyDB
#tableName - String     # Ex: MyTable
#options - JSON
```
Keys (Partition Key and all Secondary Partition Keys) are mandatory. Path is also mandatory. "?" is used as a delimiter to traverse through the JSON object while "#" is used to specify the element index if it is an array.
