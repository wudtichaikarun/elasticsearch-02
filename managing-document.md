# Managing Documents

```
// create index pages
PUT /pages

// delete index pages
DELETE /pages

// create index products specify number of shards and replicas
PUT /products
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 2
  }
}

// create document
POST /products/_doc
{
  "name": "Coffee Maker",
  "price": 64,
  "in_stock": 10
}

// update by id
PUT /products/_doc/100
{
  "name": "Toaster",
  "price": 49,
  "in_stock": 4
}

// query ond
GET /products/_doc/100

// query many
GET /products/_search

// update one
POST /products/_update/100
{
  "doc": {
    "in_stock": 3
  }
}

// update one field not exits
POST /products/_update/100
{
  "doc": {
    "tags": ["electronics"]
  }
}
```

> script test subtracted "in_stock" before run script "in_stock" : 3 after "in_stock" : 2

before run script

```
// command
GET /products/_doc/100

// result
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "_version" : 4,
  "_seq_no" : 3,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Toaster",
    "price" : 49,
    "in_stock" : 3,
    "tags" : [
      "electronics"
    ]
  }
}

```

run script

```
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock--"
  }
}
```

after fun script

```
// command
GET /products/_doc/100

// result
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "_version" : 5,
  "_seq_no" : 4,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Toaster",
    "price" : 49,
    "in_stock" : 2,
    "tags" : [
      "electronics"
    ]
  }
}

```

> script change value "in_stock" to 10

```
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock = 10"
  }
}

// command
GET /products/_doc/100

// result
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "_version" : 6,
  "_seq_no" : 5,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Toaster",
    "price" : 49,
    "in_stock" : 10,
    "tags" : [
      "electronics"
    ]
  }
}

```

> script use params

```
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock -= params.quantity",
    "params": {
      "quantity": 4
    }
  }
}

// command
GET /products/_doc/100

// result
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "_version" : 7,
  "_seq_no" : 6,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Toaster",
    "price" : 49,
    "in_stock" : 6,
    "tags" : [
      "electronics"
    ]
  }
}

```

- upsert

> document id 101 don't exists then create the document

```
POST /products/_update/101
{
  "script": {
    "source": "ctx._source.in_stock++"
  },
  "upsert": {
    "name": "Blender",
    "price": 399,
    "in_stock": 5
  }
}

// command
GET /products/_doc/101

// result
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "101",
  "_version" : 2,
  "_seq_no" : 8,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Blender",
    "price" : 399,
    "in_stock" : 5
  }
}

```

> document id 101 already exists then update the document

```
POST /products/_update/101
{
  "script": {
    "source": "ctx._source.in_stock++"
  },
  "upsert": {
    "name": "Blender",
    "price": 399,
    "in_stock": 5
  }
}

// command
GET /products/_doc/101

// result
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "101",
  "_version" : 2,
  "_seq_no" : 8,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Blender",
    "price" : 399,
    "in_stock" : 6
  }
}

```

- replacing documents

> need to replacing field "tags"

before

```
// command
GET /products/_doc/100

// result
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "_version" : 7,
  "_seq_no" : 6,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Toaster",
    "price" : 49,
    "in_stock" : 6,
    "tags" : [
      "electronics"
    ]
  }
}
```

run

```
PUT /products/_doc/100
{
  "name": "Toaster",
  "price": 79,
  "in_stock": 6
}
```

after

```
// command
GET /products/_doc/100

// result
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "_version" : 8,
  "_seq_no" : 9,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Toaster",
    "price" : 79,
    "in_stock" : 6
  }
}

```

delete document

run

```
DELETE /products/_doc/100
```

after

```
// command
GET /products/_doc/100

// result
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "found" : false
}

```
