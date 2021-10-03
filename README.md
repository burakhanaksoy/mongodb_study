# mongodb_study

 <p align="center" float="left">
<img width=400px height=280px src="https://user-images.githubusercontent.com/31994778/133131317-dc119c1b-2c57-4cc6-adbe-4aefb7a0843e.jpeg">
<img width=300px height=280px src="https://user-images.githubusercontent.com/31994778/133131163-e9b2eb4d-e7ef-460e-9dd6-c1f4ac4e653f.png">
  </p>

<b>Table Of Contents</b> |
------------ | 
[What is MongoDB?](#introduction)
[Robo3T & Mongo Shell?](#used-tools)
[First Interaction with MongoDB](#first-interaction)
[Structure of Mongodb](#structure)
[Common Functions](#functions)
[Cursor Object](#cursor-object)
[Projection](#projection)
[Using $gt $lt](#basic-operators)
[Chapter 1 Summary](#summary-1)
[MongoDB Compass](#compass)
[Working With Arrays](#arrays)
[Accessing Fields of a Nested Document](#accessing-fields)
[Understanding Ordered Insert](#ordered-insert)
[Understanding writeConcern](#write-concern)
[Atomicity](#atomicity)
[Deep Dive into Read Operations](#read-operations)
[Operators](#operators)
[Working With Cursors](#cursors)
[Deep Dive into Update Operations](#update-operations)
[Understanding Upsert](#upsert)
[Summary of Chapter 2](#summary-2)
[Aggregation Framework](#aggregation-framework)


<p id="introduction">
  <h2>What is MongoDB?</h2>
  <b><i>"MongoDB is a powerfull, schemaless database. No modeling, no relations."</b></i>
  
  I'm not an expert on relational DBs, but in my opinion, MongoDB's power lies in expressive querying support, being schemaless and JSON(BSON) supported format for reading, inserting, updating, and deleting data.
  
  <a target="_blank" href="https://www.mongodb.com/">
  Click for more!
  </a>
  <br>
  
  MongoDB's use peaked in the following years and as of 2020, it's the 5th most used DB. [REF](https://analyticsindiamag.com/10-most-used-databases-by-developers-in-2020/)
  </p>

---

<p id="used-tools">
 <h2>Robo3T & Mongo Shell?</h2>
 
 During my study, I will be using Robo3T and Mongo Shell.
 
 <p align="center">
 <img width="600" alt="Screen Shot 2021-09-13 at 8 57 31 PM" src="https://user-images.githubusercontent.com/31994778/133133275-375d737a-1df9-47a2-8d97-46d0fe1bf229.png">
 </p>
 
 <p align="center">
  An image of Robo3T
 </p>
 
 <p align="center">
 <img width="503" alt="Screen Shot 2021-09-13 at 8 58 27 PM" src="https://user-images.githubusercontent.com/31994778/133133415-3850719b-7d63-497c-af7b-739310dbb77d.png">
 </p>
 
 <p align="center">
 An image of Mongo Shell
 </p>
 
 Robo3T is just an interface for observing our data in the DB. As an alternative, we can also use MongoDB Compass, but I like Robo3T more.
 
 Mongo Shell is a MongoDB client.
 </p>
 
 ---
 
 <p id="first-interaction">
 Now, I am going to use MongoDB's `insertOne` method to insert a document.
 
 On the shell, I execute `db.test_coll.insertOne({"name":"Burak"})`
 
 This will return the following:
 ```
 test> db.test_coll.insertOne({"name":"Burak"})
{
  acknowledged: true,
  insertedId: ObjectId("613f923b70d0e7fb53a69c77")
}
 ```
 
 And, to double check, we can display the newly inserted data via Robo3T.
 
 <p align="center">
  <img width="400" alt="Screen Shot 2021-09-13 at 9 03 47 PM" src="https://user-images.githubusercontent.com/31994778/133134167-3b8695f8-c12c-49a8-a090-4f99dedfa06c.png">
  </p>
 
 This was our first interaction with MongoDB and Robo3T.
 
 I can also use the shell to view my data.
 
 ```
 test> db.test_coll.find({}).pretty()
[
  {
    _id: ObjectId("613f90fb70d0e7fb53a69c76"),
    test: 'My First Document'
  },
  { _id: ObjectId("613f923b70d0e7fb53a69c77"), name: 'Burak' }
]
```

Also, we can views our dbs through `show dbs` command


 <img width="157" alt="Screen Shot 2021-09-14 at 7 51 38 AM" src="https://user-images.githubusercontent.com/31994778/133196968-382e2d13-babe-4d8d-aee8-bf0ad29816df.png">

 Also, we can switch to a different db with `use` command
  
  <img width="252" alt="Screen Shot 2021-09-14 at 7 53 38 AM" src="https://user-images.githubusercontent.com/31994778/133197142-9b5b4647-23f6-4cd3-80ab-7f200a15689d.png">

 </p>
 
 </p>
 
 ---
 
 <p id="structure">
   <h2>Structure of Mongodb</h2>
   
   MongoDb has a structure of DB > Collection > Document. What I mean by this can be displayed as follows:
   
   <p align="center">
     <img width="890" alt="Screen Shot 2021-09-13 at 9 08 03 PM" src="https://user-images.githubusercontent.com/31994778/133134923-a2c4b553-b092-443a-a368-fb79b6070d2f.png">
  </p>
  
  Document is the smallest unit that we have. Collections contain documents with similar nature. DB is the biggest unit and it contains collections.
  
  <b>Format is always JSON.</b> When we retrieve, update, remove, and create data, we will always use JSON format.
 </p>
 
 ---
 
 <p id="functions">
 <h2>Common Functions</h2>
 
 Let's start learning the common functions.
 
 <h3>insertOne</h3>
 
 `insertOne` is used for inserting a single document on the collection. We did an example of this previously.
 
 ---
 
 <h3>insertMany</h3>
 
 Like `insertOne`, `insertMany` is used for inserting multiple documents to our collection.
 
 ```json
 [
    {
        "name":"Burak",
        "last_name":"Aksoy"
    },
    {
        "name":"Ahmet",
        "last_name":"Ceviz"
    }
]
```

We can insert both of these data through `insertMany` command.

```
db.test_coll.insertMany(
... [
... {
.....         "name":"Burak",
.....         "last_name":"Aksoy"
.....     },
...     {
.....         "name":"Ahmet",
.....         "last_name":"Ceviz"
.....     }]
... )
```

And we receive

```
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("613f964870d0e7fb53a69c7a"),
    '1': ObjectId("613f964870d0e7fb53a69c7b")
  }
}
```

We can take a look at our documents with `db.test_coll.find({}).pretty()`

pretty() is just simply for printing in a prettier format.

```
db.test_coll.find({}).pretty()
[
  {
    _id: ObjectId("613f90fb70d0e7fb53a69c76"),
    test: 'My First Document'
  },
  { _id: ObjectId("613f923b70d0e7fb53a69c77"), name: 'Burak' },
  {
    _id: ObjectId("613f964870d0e7fb53a69c7a"),
    name: 'Burak',
    last_name: 'Aksoy'
  },
  {
    _id: ObjectId("613f964870d0e7fb53a69c7b"),
    name: 'Ahmet',
    last_name: 'Ceviz'
  }
]
```

<b>As you can see, these documents have no common schema.</b>

---

<h3>findOne</h3>

`findOne` works with filtering. This is `db.some_coll.findOne(<filter>)`

<b>`find` may find many documents matching the filtering, but `findOne` finds the first document that matches the filter, even though there are other documents matching the filter.</b>

<p align="center">
 <img width="655" alt="Screen Shot 2021-09-13 at 9 25 12 PM" src="https://user-images.githubusercontent.com/31994778/133137032-90264d9c-3900-4e2e-9181-304bf066cedd.png">
 </p>
 
 ---
 
 <h3>remove</h3>
 
 used as `db.some_coll.remove(<filter>)`
 
 ```
 db.test_coll.remove({})
{ acknowledged: true, deletedCount: 4 }
```

Now our db is empty 😑

<b>Remember that command({}) will do a collective operation. Here, `{}` somewhat means "all".</b>
 
 ---
 
 <h3>updateMany</h3>
 
 To show update, I will be working on a new db.
<p align="center"> 
 <img width="503" alt="Screen Shot 2021-09-14 at 7 59 18 AM" src="https://user-images.githubusercontent.com/31994778/133197586-e1e6b6d6-e8b8-4e84-b15d-2540f30fd87e.png">
</p>

Let's find the ones with occupation : null, and change it to student.

```
db.students_coll.updateMany({"occupation":null},{"$set":{"occupation":"student"}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 2,
  modifiedCount: 2,
  upsertedCount: 0
}
```

We changed the matching ones successfully.

<p align="center">
 <img width="850" alt="Screen Shot 2021-09-14 at 8 03 22 AM" src="https://user-images.githubusercontent.com/31994778/133197999-25569a70-5b3d-4f5c-a31b-b94a0d543bae.png">
 </p>
 
<b>`$set` is a MongoDB operator used for setting a field of a document.</b>

---

<h3>updateOne</h3>

`updateOne` is to `updateMany` as `findOne` is to `findMany`. :)

Only updates the first matching document.

<img width="600" alt="Screen Shot 2021-09-14 at 8 18 59 PM" src="https://user-images.githubusercontent.com/31994778/133304079-0ba746ee-b056-4d0e-b08d-821c89f38d09.png">
 
---

<p id="cursor-object">
 
 <b>Cursor object in MongoDB is just like a generator in Python.</b>
 
 For large documents, when we use `find()` command, MongoDB returns us a cursor object, which holds the data that we want in a space efficient manner.
 
 What I mean by space efficient is that cursor object does contain all the data, but in a format that resembles to a generator in Python.
 
 For example, I have lots of data in my db... Say, millions of data. Returning all the data at once will scream <b>PERFORMANCE ISSUE!!</b>.
 
 Let's do a small example.
 
 <img width="529" alt="Screen Shot 2021-09-14 at 8 35 23 PM" src="https://user-images.githubusercontent.com/31994778/133306400-dd405fbe-72a7-4370-aa25-e288dba199b0.png">

 That's a lot of documents.. Now, let's use `find()`.
 
 <img width="496" alt="Screen Shot 2021-09-14 at 8 36 27 PM" src="https://user-images.githubusercontent.com/31994778/133306755-06bad7ec-aa47-4cb7-97f5-f112076d99cb.png">
 
 As you can see here, MongoDB doesn't return all the data at once. Instead, it returns a cursor object which we waits for `it` to show more data. This is just like `next` in Python's generator.
 
 Imagine returning all that data at once, it would take a long time...
 
 When we exhaust `it`, shell tells us the following:
 
 <img width="157" alt="Screen Shot 2021-09-14 at 8 41 57 PM" src="https://user-images.githubusercontent.com/31994778/133307268-efe3a306-f9dd-48b4-bcef-ab6736c11894.png">

 
 We can see all the data through using `toArray()` command.
 
 `db.students_coll.find().toArray()`
 
 And also, we can make operations on the data using `forEach()` command.
 
<img width="729" alt="Screen Shot 2021-09-14 at 8 48 15 PM" src="https://user-images.githubusercontent.com/31994778/133308176-cd8481bb-85ba-4118-b2a7-3aca38da6111.png">
 
<img width="574" alt="Screen Shot 2021-09-14 at 8 48 24 PM" src="https://user-images.githubusercontent.com/31994778/133308187-abb64cae-959b-4852-9c4a-f3cda3ff9bb7.png">

 As a summary:
 
 - Shell gives us the first 20 documents by default.
 
 - After 20 documents, `it` should be used to view more since `find()` command returns a cursor object.
 
 - We cannot use `.pretty()` with `findOne()` since pretty() is a method of the cursor object. But since findOne already returns the data itself, and since data doesn't have pretty() method, using pretty() with findOne fails.
 
 <img width="609" alt="Screen Shot 2021-09-14 at 8 54 27 PM" src="https://user-images.githubusercontent.com/31994778/133308970-70a3bc51-aed3-4889-a3c9-a76dd82fd7f1.png">

 <b>For create, update, and delete methods, cursor object doesn't exist, because they don't fetch data.</b>
 
 </p>
 
 ---
 
 <p id="projection">
 
 <b>Projection is to go when we don't want to display unwanted fields.</b>
 
 Here's what I got in my db.
 
 <img width="483" alt="Screen Shot 2021-09-14 at 9 44 07 PM" src="https://user-images.githubusercontent.com/31994778/133315710-20621890-38dc-432c-a950-61cd8e626440.png">

 Let's say that I only want to see name, last_name and address information. To do that, we use projection.
 
 `db.students_coll.find({},{"_id":0, "name":1, "last_name":1, "address":1 })`
 
 <img width="490" alt="Screen Shot 2021-09-14 at 9 46 53 PM" src="https://user-images.githubusercontent.com/31994778/133316145-11228e3d-4c26-48d7-8bd6-bebf57fa8020.png">

 wanted fields take `1`, unwanted fields take `0`.
 
 </p>
 
 ---
 
 <p id="Working With Arrays">
 MongoDB documents can also hold array data.
 
 Say that we have a document like this
 
 ```json
 {
        "name": "Burakhan",
        "last_name": "Aksoy",
        "age": 26,
        "occupation": "Junior Python Developer",
        "address": {
            "city": "Istanbul",
            "country": "Turkey"
        },
        "hobbies": [
            "music",
            "games"
        ]
    }
 ```
 
 <img width="640" alt="Screen Shot 2021-09-14 at 10 06 49 PM" src="https://user-images.githubusercontent.com/31994778/133318847-154c119b-1e5a-45e1-94ac-bc4f256cf818.png">

 Now, I can reach the related document in MongoDB by making a search on "hobbies" field.
 
 ```
 db.users.findOne({"hobbies":"movies"})
{
  _id: ObjectId("6140f27470d0e7fb53a69ce2"),
  name: 'Max',
  last_name: 'Garner',
  age: 32,
  occupation: 'Sous Chef',
  address: { city: 'New York', country: 'U.S.A' },
  hobbies: [ 'cooking', 'movies' ]
}
```
 
 And since I use findOne here, I can directly access the field with document.fieldName.
 
 ```
 db.users.findOne({"hobbies":"movies"}).name
Max
```
 
 We can also access a field of embedded document as follows:
 
 `db.coll_name.find({"field.insideField":"value"})`
 
 <img width="503" alt="Screen Shot 2021-09-14 at 10 17 02 PM" src="https://user-images.githubusercontent.com/31994778/133320202-0a803f98-d072-4e00-872d-7ab0d5fab038.png">

 <b>Remember that we get the actual data with findOne, and we get cursor object with find.</b>
 
 </p>
 
 ---
 
 <p id ="basic-operators">
 <h3>Using $gt $lt</h3>
 
 We can use $gt and $lt, along with $gte and $lte to make comparison based search in MongoDB.
 
 <img width="505" alt="Screen Shot 2021-09-14 at 10 27 19 PM" src="https://user-images.githubusercontent.com/31994778/133321462-90b4517e-16c2-4621-9cd6-782a01363f2e.png">

<img width="510" alt="Screen Shot 2021-09-14 at 10 27 32 PM" src="https://user-images.githubusercontent.com/31994778/133321470-29fc16a8-81a9-4a16-a01b-11446a354086.png">

 </p>
 
 ---
 
 <p id="summary-1">
 <h3>Chapter 1 Summary</h3>
 
 <p align="center">
 <img width="917" alt="Screen Shot 2021-09-14 at 10 24 34 PM" src="https://user-images.githubusercontent.com/31994778/133321745-b78d4c8a-4496-4567-8e86-315a85cc815b.png">
</p>
 </p>
 
 ---
 
 
<h2>Chapter 2</h2>

<p id="compass">
<h3>MongoDB Compass</h3>
</p>

Actually, MongoDB Compass is more powerful and developer friendly than Robo3T.

<p align="center">
 <img width="900" alt="Screen Shot 2021-09-16 at 7 18 54 PM" src="https://user-images.githubusercontent.com/31994778/133648646-0b84e499-3e10-4264-b0ea-73f99ef9cc3f.png">
 </p>
 
 One of the advantages of Compass over Robo3T is the aggregation support.
 
 <p align="center">
<img width="900" alt="Screen Shot 2021-09-16 at 7 21 57 PM" src="https://user-images.githubusercontent.com/31994778/133649249-35363acc-955e-4289-a198-4c911f8a7575.png">
 </p>
 
 We can do anything that we do in Robo3T.
 
 <p align="center">
 <img width="710" alt="Screen Shot 2021-09-16 at 7 24 41 PM" src="https://user-images.githubusercontent.com/31994778/133649497-f4978c1e-96d6-43bb-9a7d-da3ee1ad3a5c.png">
</p>

---

<p id="arrays">
<h3>Working with Arrays</h3>

MongoDB documents can contain arrays. Arrays can contain heterogenous data types, like a string and integer at the same time, or homogenous, such as all elements of an array are strings. Arrays can even contain objects.

For example:

<img width="412" alt="Screen Shot 2021-09-16 at 8 07 21 PM" src="https://user-images.githubusercontent.com/31994778/133655469-1764946f-6187-4685-9249-3ac7d5dbea43.png">

<img width="366" alt="Screen Shot 2021-09-16 at 8 08 07 PM" src="https://user-images.githubusercontent.com/31994778/133655485-f4a37f51-df8c-490a-944d-e7d24978b0e4.png">

<img width="371" alt="Screen Shot 2021-09-16 at 8 08 26 PM" src="https://user-images.githubusercontent.com/31994778/133655501-6c12d1f4-8ff1-4f0f-a134-273a9f4274ec.png">

We can also store objects inside an array:

<img width="444" alt="Screen Shot 2021-09-16 at 8 16 29 PM" src="https://user-images.githubusercontent.com/31994778/133656360-2c11e972-80a0-4a4c-8476-dcb14b1825b4.png">

and the query for that was

`db.users.updateOne({"hobbies":"technology"}, {"$addToSet":{"hobbies":{"name":"skiing", "beenDoingYears":11, "team":"solo"}}})`


</p>

---

<p id="accessing-fields">
 <h3>Accessing Fields of a Nested Document</h3>
</p>

Now, let's say that we have a document like this:

```
{ _id: ObjectId("6143798193587d2fb830c1bf"),
  name: 'Burak',
  lastName: 'Aksoy',
  favoriteHobby: { name: 'skiing', beenDoingYears: 11, team: 'solo' } }
```

We can easily use findOne on `favoriteHobby`

<img width="450" alt="Screen Shot 2021-09-16 at 9 12 10 PM" src="https://user-images.githubusercontent.com/31994778/133663485-77fedbe3-f208-4b15-8d22-a593c9907c9e.png">

As we can see, we accessed name field inside favoriteHobby object and use `findOne` command.

Let's add a new field to our embedded `favoriteHobby` object.

`db.users.updateOne({"name":"Burak"}, {"$set":{"favoriteHobby.field":"snow"}})`

<img width="384" alt="Screen Shot 2021-09-16 at 9 23 25 PM" src="https://user-images.githubusercontent.com/31994778/133664929-cbf14255-d444-4412-a2fd-6a7c3833fb59.png">

Now, let's remove `field` field from `favoriteHobby` object.

`db.users.updateOne({"name":"Burak"}, {"$unset":{"favoriteHobby.field":""}})`

<img width="581" alt="Screen Shot 2021-09-16 at 9 25 12 PM" src="https://user-images.githubusercontent.com/31994778/133665139-c350f23e-2f7a-4018-b498-6a67f3bc8ec0.png">

<b>Here, we used $set for adding a new field to our object, and $unset to remove a field.</b>

It doesn't matter how many levels of nesting we have, we can use `.` for accessing fields in each level.

Let's say our document looks like this at the moment:

<img width="502" alt="Screen Shot 2021-09-16 at 9 29 22 PM" src="https://user-images.githubusercontent.com/31994778/133665721-6c3c69e0-f85c-486a-bf0f-79d06df2602a.png">

We can add a new field to innermost nested object in the same way we did before.

`db.users.updateOne({"name":"Burak"},{"$set":{"favoriteHobby.anotherNestedObject.anotherObject.lastField":"Finally!"}})`

And here's our document:

<img width="690" alt="Screen Shot 2021-09-16 at 9 31 55 PM" src="https://user-images.githubusercontent.com/31994778/133666028-44fa8ea8-32df-4868-8797-04398bef66c3.png">

This goes on and on :)

---

<p id="ordered-insert">
 <h3>Understanding Ordered Insert</h3>
 </p>
 
 <b>By default, when MongoDB insertMany inserts documents one-by-one and stops adding the next document if an error occurs in the document being added.</b>
 
 Let's understand this with an example:
 
 Let's add two documents to our new DB `Hobbies`.
 
 <img width="853" alt="Screen Shot 2021-09-16 at 10 07 15 PM" src="https://user-images.githubusercontent.com/31994778/133670789-04bdb8bd-6529-414c-8b75-f44f51351065.png">
 
 Then, let's try to add more documents.
 
```
[
   {
      "_id":"football",
      "name":"football"
   },
   {
      "_id":"technology",
      "name":"technology"
   },
   {
      "_id":"cars",
      "name":"cars"
   }
]
```

To add these documents, we can use

`db.hobby_coll.insertMany([{"_id":"football", "name":"football"},{"_id":"technology", "name":"technology"}, {"_id":"cars", "name":"cars"}])`

However, this fails...

```
db.hobby_coll.insertMany([{"_id":"football", "name":"football"},{"_id":"technology", "name":"technology"}, {"_id":"cars", "name":"cars"}])
MongoBulkWriteError: E11000 duplicate key error collection: Hobbies.hobby_coll index: _id_ dup key: { _id: "technology" }
```

MongoDB tried to insert all these documents, it inserted football successfully but when it tried to insert technology duplicate key error occured.

- 1. MongoDB stopped adding documents after technology.
- 2. Documents before technology were added successfully.

How can we change this default behavior?

We can use {"ordered":false}

`db.hobby_coll.insertMany([{"_id":"football", "name":"football"},{"_id":"technology", "name":"technology"}, {"_id":"cars", "name":"cars"}], {"ordered":false})`

<img width="351" alt="Screen Shot 2021-09-16 at 10 16 31 PM" src="https://user-images.githubusercontent.com/31994778/133671967-6fc65c93-d2ca-4306-a267-12437768f899.png">

 <img width="501" alt="Screen Shot 2021-09-16 at 11 30 32 PM" src="https://user-images.githubusercontent.com/31994778/133681862-6cffbed3-9d4f-4496-81de-13ebbf829087.png">

---

<p id="write-concern">
 <h3>Understanding writeConcern</h3>
 </p>
 
 <img width="1024" alt="Screen Shot 2021-09-16 at 10 32 35 PM" src="https://user-images.githubusercontent.com/31994778/133676787-772856cd-6f62-44ac-b603-195117be4a53.png">

When we insert a new document to our collection through a client, i.e., MongoDB shell, this request goes through MongoDB server, as always. 

Then, server directs the incoming request to Storage Engine for writing on the disk.

<b>writeConcern by default makes sure that this document is added to the disk before it goes on inserting another document.</b>

This is a very safe approach, because we want to make sure our data is added on the DB before adding another data. However, writeConcern doesn't ensure that data is added at times when DB connection has a problem. For those times, we want to use `journal:true`.

Journal is the `To do` part of of Storage Engine. If used as `journal:true`, the data will be added to the DB even if connection error occurs in the DB.

Let's make an example:

<img width="600" alt="Screen Shot 2021-09-16 at 11 03 32 PM" src="https://user-images.githubusercontent.com/31994778/133678388-6edc34be-d90f-4fbc-9d0d-a21516268b7b.png">

The difference between these two operations is that the MongoDB server doesn't acknowledge the insert operation of the second document, meaning that we can't make sure if the second document is added successfully or not unless we check the data in DB. Indeed, not very safe.

Now, let's insert another document with journal:true.

```
db.students.insertOne({"name":"Mehmet", "age":26}, {writeConcern:{"w":0, "j":true}})
{ acknowledged: false,
  insertedId: ObjectId("6143a43693587d2fb830c1c4") }
```

Here, MongoDB will insert this document eventually, even if the connection is shaky and/or disrupted. Might take longer timewise than not using "j":true, but much safer.

<b>By default, writeConcern's "w":1 and "j":undefined or "j":false</b>

Let's insertMany with writeConcern:{"w":0}.

```
db.students.insertMany([{"name":"Burak"}, {"name":"Ahmet"}, {"name":"Berna"}, {"name":"James"}], {writeConcern:{w:0}})
{ acknowledged: false,
  insertedIds: 
   { '0': ObjectId("6143a52093587d2fb830c1c5"),
     '1': ObjectId("6143a52093587d2fb830c1c6"),
     '2': ObjectId("6143a52093587d2fb830c1c7"),
     '3': ObjectId("6143a52093587d2fb830c1c8") } }
```

Here, since acknowledged:false, we can't know instantly whether these documents were added successfully unless we check the db.

<b>writeConcern can also be applied to update and delete operations.</b>

<img width="401" alt="Screen Shot 2021-09-16 at 11 30 59 PM" src="https://user-images.githubusercontent.com/31994778/133681800-afaf9e27-8fc7-42a6-a951-f25a7a4a6711.png">


---

<p id="atomicity">
 <h3>Atomicity</h3>
 </p>

<img width="997" alt="Screen Shot 2021-09-16 at 11 16 11 PM" src="https://user-images.githubusercontent.com/31994778/133680180-793f6798-3106-4006-aa04-096dc28bcb98.png">

In MongoDBs create operations, atomicity applies in document level. What does that mean?

This means that when we create documents in our DB, with many nested fields or not, MongoDB <b>will either successfully insert it or, if anything goes wrong, won't insert that document at all.</b> So we have two options, either success or failure.

This also means that when we have a very large document to insert, it is impossible for MongoDB to insert some fields of that document and not insert other fields.

This is the atomicity of MongoDB document insertion.

---

<p id="read-operations">
 <h2>Deep Dive into Read Operations</h2>
 </p>

Up to now, we dealt with insert operations, i.e., insertOne, insertMany, and how to customize these inserts with some parameters, i.e., writeConcern and {"ordered":false}.

<b>Insert operations is only useful as long as we fetch the data we inserted.</b>

So, in this part, let us deep dive into reading operations in MongoDB.

<p align="center">
<img width="819" alt="Screen Shot 2021-09-18 at 7 54 55 PM" src="https://user-images.githubusercontent.com/31994778/133896406-d2f7dad5-567c-4cea-b186-84223c57be56.png">
</p

<p id="operators">
 <h3>Operators</h3>
 </p>

<p align="center">
<img width="650" alt="Screen Shot 2021-09-18 at 8 01 48 PM" src="https://user-images.githubusercontent.com/31994778/133896599-2f27d9ae-69a6-4996-8946-61f8a35addf2.png">
</p>

Now, we have a DB like this:

<img width="455" alt="Screen Shot 2021-09-18 at 8 15 17 PM" src="https://user-images.githubusercontent.com/31994778/133896986-735445bf-c9a1-4f70-8462-270e1be4bb56.png">

<h3>Comparison Operators</h3>

Now, let's use some comparison operators.

<h4>$lt</h4>

I want to find customers whose age is less than 35

```
db.customers.find({"age":{"$lt":35}}, {"_id":0})
{ name: 'Burakhan',
  last_name: 'Aksoy',
  age: 26,
  occupation: 'Junior Python Developer',
  address: { city: 'Istanbul', country: 'Turkey' },
  hobbies: [ 'music', 'games' ] }
{ name: 'Max',
  last_name: 'Garner',
  age: 32,
  occupation: 'Sous Chef',
  address: { city: 'New York', country: 'U.S.A' },
  hobbies: [ 'cooking', 'movies' ] }
```

<h4>$lte</h4>

I want to find customers whose age is less than or equal to 44

```
db.customers.find({"age":{"$lte":44}}, {"_id":0})
{ name: 'Burakhan',
  last_name: 'Aksoy',
  age: 26,
  occupation: 'Junior Python Developer',
  address: { city: 'Istanbul', country: 'Turkey' },
  hobbies: [ 'music', 'games' ] }
{ name: 'Max',
  last_name: 'Garner',
  age: 32,
  occupation: 'Sous Chef',
  address: { city: 'New York', country: 'U.S.A' },
  hobbies: [ 'cooking', 'movies' ] }
{ name: 'Alex',
  last_name: 'Schafer',
  age: 44,
  occupation: 'Project Manager',
  address: { city: 'Texas', country: 'U.S.A' },
  hobbies: [ 'cars', 'wildlife' ] }
  ```
  
  We can use `$gt` and `$gte` in the same way.
  
  <h4>$ne</h4>
  
  To filter documents whose field is not equal to the given value.
  
  <img width="444" alt="Screen Shot 2021-09-18 at 8 29 15 PM" src="https://user-images.githubusercontent.com/31994778/133897389-299779f3-b71b-4941-978a-c6edf4b7353e.png">
  
  ---
  
  <h4>$in</h4>
  
  Matches any of the values specified in an array.
  
  ```
  db.customers.find({hobbies:{"$in":["cooking"]}})
{ _id: ObjectId("61461e6c93587d2fb830c1ca"),
  name: 'Max',
  last_name: 'Garner',
  age: 32,
  occupation: 'Sous Chef',
  address: { city: 'New York', country: 'U.S.A' },
  hobbies: [ 'cooking', 'movies' ] }
{ _id: ObjectId("61461e6c93587d2fb830c1cc"),
  name: 'Homer',
  last_name: 'Simpson',
  age: 54,
  occupation: 'Nuclear Plant Worker',
  address: { city: 'Chicago', country: 'U.S.A' },
  hobbies: [ 'cooking', 'movies' ] }
  ```
  
  ```
  db.customers.find({"age":{"$in":[32, 44]}})
{ _id: ObjectId("61461e6c93587d2fb830c1ca"),
  name: 'Max',
  last_name: 'Garner',
  age: 32,
  occupation: 'Sous Chef',
  address: { city: 'New York', country: 'U.S.A' },
  hobbies: [ 'cooking', 'movies' ] }
{ _id: ObjectId("61461e6c93587d2fb830c1cb"),
  name: 'Alex',
  last_name: 'Schafer',
  age: 44,
  occupation: 'Project Manager',
  address: { city: 'Texas', country: 'U.S.A' },
  hobbies: [ 'cars', 'wildlife' ] }
  ```
  
  ---
  
  <h4>$nin</h4>
  
  ```
  { _id: ObjectId("61461e6c93587d2fb830c1c9"),
  name: 'Burakhan',
  last_name: 'Aksoy',
  age: 26,
  occupation: 'Junior Python Developer',
  address: { city: 'Istanbul', country: 'Turkey' },
  hobbies: [ 'music', 'games' ] }
{ _id: ObjectId("61461e6c93587d2fb830c1cb"),
  name: 'Alex',
  last_name: 'Schafer',
  age: 44,
  occupation: 'Project Manager',
  address: { city: 'Texas', country: 'U.S.A' },
  hobbies: [ 'cars', 'wildlife' ] }
  ```
  
  ---
  
  <h4>$or</h4>
  
  Joins query clauses with a logical OR returns all documents that match the conditions of either clause.
  
  ```
  db.customers.find({"$or":[{"hobbies":{"$in":["cooking"]}}, {"hobbies":{"$in":["wildlife"]}}]})
  { name: 'Max',
  last_name: 'Garner',
  hobbies: [ 'cooking', 'movies' ] }
  { name: 'Alex',
  last_name: 'Schafer',
  hobbies: [ 'cars', 'wildlife' ] }
  { name: 'Homer',
  last_name: 'Simpson',
  hobbies: [ 'cooking', 'movies' ] }
  ```
  
  This will fetch the customers that have hobbies either cooking or wildlife, or both.
  
  ---
  
  <h4>$and</h4>
  
  Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.
  
  ```
  db.customers.find({"$and":[{"hobbies":{"$in":["cars"]}}, {"age":{"$lt":45}}]})
{ name: 'Alex',
  last_name: 'Schafer',
  age: 44,
  occupation: 'Project Manager',
  address: { city: 'Texas', country: 'U.S.A' },
  hobbies: [ 'cars', 'wildlife' ] }
  ```
  
  Returns documents that has <b>cars</b> in hobbies array <b>AND</b> <b>age</b> less than 44.
  
  ---
  
  <h4>$not</h4>
  
  Inverts the effect of a query expression and returns documents that do not match the query expression.
  
  ```
  db.customers.find({"$and":[{"hobbies":{"$not":{"$in":["cars"]}}}, {"age":{"$lt":45}}]})
  ```
  
  <img width="438" alt="Screen Shot 2021-09-18 at 9 36 06 PM" src="https://user-images.githubusercontent.com/31994778/133905130-9ac67545-4e1b-48ef-9e21-a70958447d92.png">

Finding documents whose array doesn't contain `"cars"` and age is less than 45.

<b>$not basically negates the operator that succeeds.</b>

`{"hobbies":{"$not":{"$in":["cars"]}}} == {"hobbies":{"$nin":["cars"]}}`

---

<h4>$exists</h4>

Matches documents that have the specified field.

`db.customers.find({"last_name":{"$exists":false}})`

This query returns documents that doesn't have `last_name` field.

---

<h4>$regex</h4>

Selects documents where values match a specified regular expression.

```json
{ <field>: { $regex: /pattern/, $options: '<options>' } }
{ <field>: { $regex: 'pattern', $options: '<options>' } }
{ <field>: { $regex: /pattern/<options> } }
```

```
db.customers.find({"occupation":{"$regex":/(.)r\Z/}})
{ _id: ObjectId("61461e6c93587d2fb830c1c9"),
  name: 'Burakhan',
  last_name: 'Aksoy',
  age: 26,
  occupation: 'Junior Python Developer',
  address: { city: 'Istanbul', country: 'Turkey' },
  hobbies: [ 'music', 'games' ] }
{ _id: ObjectId("61461e6c93587d2fb830c1cb"),
  name: 'Alex',
  last_name: 'Schafer',
  age: 44,
  occupation: 'Project Manager',
  address: { city: 'Texas', country: 'U.S.A' },
  hobbies: [ 'cars', 'wildlife' ] }
{ _id: ObjectId("61461e6c93587d2fb830c1cc"),
  name: 'Homer',
  last_name: 'Simpson',
  age: 54,
  occupation: 'Nuclear Plant Worker',
  address: { city: 'Chicago', country: 'U.S.A' },
  hobbies: [ 'cooking', 'movies' ] }
```

Here, `/(.)r\Z/` is used for finding the ones with occupation field's last two characters ending with `(.)r`, meaning it has to end with `r` but the letter before `r` can be anything.

Let's say that we have a document like this:

```
{
   "name":"Bob",
   "last_name":"Bates",
   "age":54,
   "occupation":"Cyroproctor",
   "address":{
      "city":"New Delhi",
      "country":"India"
   },
   "hobbies":[
      "cooking",
      "movies"
   ]
}
```

The following query will find occupations ending with [any character but `o`] and `r`.

```
db.customers.find({"occupation":{"$regex":/[^o]r\Z/}})
```

We can also search for array items with ignore case.

`db.customers.find({"hobbies":{"$regex":/Cooking/}})`

This returns no document.

But if we search like this:

```
db.customers.find({"hobbies":{"$regex":/COoKIng/, "$options":"i"}})
{ _id: ObjectId("61461e6c93587d2fb830c1ca"),
  name: 'Max',
  last_name: 'Garner',
  age: 32,
  occupation: 'Sous Chef',
  address: { city: 'New York', country: 'U.S.A' },
  hobbies: [ 'cooking', 'movies' ] }
{ _id: ObjectId("61461e6c93587d2fb830c1cc"),
  name: 'Homer',
  last_name: 'Simpson',
  age: 54,
  occupation: 'Nuclear Plant Worker',
  address: { city: 'Chicago', country: 'U.S.A' },
  hobbies: [ 'cooking', 'movies' ] }
{ _id: ObjectId("614636b693587d2fb830c1cd"),
  name: 'Bob',
  last_name: 'Bates',
  age: 54,
  occupation: 'Cyroproctor',
  address: { city: 'New Delhi', country: 'India' },
  hobbies: [ 'cooking', 'movies' ] }
  ```
  
  Here, `"$options":"i"` is for ignoring lower-upper case characters.

---

<h4>Using operators in nested fields</h4>

Let's say we want to find documents whose "address" field's "city" does not start with "new" ignore case.

`db.customers.find({"address.city":{"$regex":/^(?!new)/, "$options":"i"}})`

<img width="614" alt="Screen Shot 2021-09-18 at 10 33 16 PM" src="https://user-images.githubusercontent.com/31994778/133906502-4f46b0f0-754d-4508-9c15-1a6d5e0eb25e.png">

Let's find the ones with country different than U.S.A

```
db.customers.find({"address.country":{"$ne":"U.S.A"}}, {"_id":0})
{ name: 'Burakhan',
  last_name: 'Aksoy',
  age: 26,
  occupation: 'Junior Python Developer',
  address: { city: 'Istanbul', country: 'Turkey' },
  hobbies: [ 'music', 'games' ] }
{ name: 'Bob',
  last_name: 'Bates',
  age: 54,
  occupation: 'Cyroproctor',
  address: { city: 'New Delhi', country: 'India' },
  hobbies: [ 'cooking', 'movies' ] }
```
  
  Let's find the ones with city field not in "New York" or "New Delhi"
  
  `db.customers.find({"address.city":{"$nin":["New York", "New Delhi"]}}, {"_id":0})`
  
  <img width="419" alt="Screen Shot 2021-09-19 at 9 57 40 AM" src="https://user-images.githubusercontent.com/31994778/133918559-dc8acc64-aaaf-46cc-9af8-84b58cce88ba.png">
  
---

<h4>$expr</h4>

<b>Allows the use of aggregation expressions within the query language.</b>

<b>Syntax</b>

```
{ $expr: { <expression> } }
```
Let's add new `account` field for our customers.

<img width="646" alt="Screen Shot 2021-09-19 at 10 25 35 AM" src="https://user-images.githubusercontent.com/31994778/133919143-f2ba0cdb-5762-4c0d-baea-eaa16646beec.png">

What happens if we want to find out customers with debt greater than balance? We need to compare two fields of the same document.

For that, we can use `$expr`

<img width="700" alt="Screen Shot 2021-09-19 at 10 33 33 AM" src="https://user-images.githubusercontent.com/31994778/133919328-4b2304d4-ab54-479c-b6dd-8e750a158c98.png">


Now, let's say that every customer in our bank has right to have a credit of 100 units.

Let's find the customers who has debt > balance + credit.

`db.customers.find({"$expr":{"$gt":["$account.debt", {"$add":["$account.balance", NumberInt("40")]}]}}, {"_id":0, "address":0, "hobbies":0})`

This query does the following:

1- Adds account.balance with 40.

2- Finds documents with account.debt is greater than the 40 added account.balance.

3- Projects out _id, address, hobbies fields from fetched documents.

<img width="397" alt="Screen Shot 2021-09-19 at 10 47 02 AM" src="https://user-images.githubusercontent.com/31994778/133919677-70dd44de-3e77-4806-9f1d-e3bcc318235a.png">

But, if we added 100 to account.balance

<img width="364" alt="Screen Shot 2021-09-19 at 10 47 24 AM" src="https://user-images.githubusercontent.com/31994778/133919684-b7271d5e-c444-4fd0-8c86-8d0d6d07d77b.png">

Lastly, let's use $cond operator to find customers with balance is greater than debt. However, we will make it so that balance and credit will be summed up and compared if balance is less than debt, else balance is compared with debt directly without summing with credit.

<b>Pseudo Code</b>
```
if balance < debt:
    balance += credit
    # Compare new balance with debt
else:
    # Compare balance with debt
```

```
db.customers.find({"$expr":{"$gt":["$account.debt", {"$cond":{"if":{"$gt":["$account.balance", "$account.debt"]}, then:"$account.balance", else:{"$add":["$account.balance", "$account.credit"]}}}]}})
```

<img width="480" alt="Screen Shot 2021-09-19 at 11 28 49 AM" src="https://user-images.githubusercontent.com/31994778/133920798-2f38bb75-3eb3-475d-a5c1-8dee53ffc689.png">

This is a bit contrived and complex query, it has aggregation inside, which makes things more complex, but still, very useful.

---


<h3>Querying Arrays</h3>

Say that we have documents like the following in our DB:

<img width="546" alt="Screen Shot 2021-09-19 at 12 17 21 PM" src="https://user-images.githubusercontent.com/31994778/133922135-9204bae1-7e64-430f-8472-08548c90b850.png">

Let's say I want to find every document with hobbies array has an object with name "cooking".

<img width="765" alt="Screen Shot 2021-09-19 at 12 21 11 PM" src="https://user-images.githubusercontent.com/31994778/133922220-a0abf5ff-3c6a-4181-b8fe-5ef7f8db65b4.png">

Here, we reach the JSON object's fields inside array with `arrayField.objectField` syntax.

<b>Here, although `name` is not a direct field of hobbies array, MongoDB can understand that we want to search `name` field of objects inside hobbies array.</b>

---

<h4>$size</h4>

The $size operator matches any array with the number of elements specified by the argument.

Say that I want to find the document which has 3 documents in hobbies array.

```
db.customers.find({"hobbies":{"$size":3}})
{ _id: ObjectId("6147042c1f3ef37225babbe3"),
  name: 'Janeth',
  last_name: 'Thompson',
  age: 22,
  occupation: 'student',
  address: { city: 'New Mexico', country: 'U.S.A' },
  account: { balance: 300, debt: 550, credit: 200 },
  hobbies: 
   [ { name: 'languages', perWeek: 5 },
     { name: 'running', perWeek: 3 },
     { name: 'movies', perWeek: 2 } ] }
```

---

<h4>$all</h4>

The $all operator selects the documents where the value of a field is an array that contains all the specified elements.

```
db.customers.find({"hobbies.perWeek":{"$all":[1]}}).count()
3
db.customers.find({"hobbies.perWeek":{"$all":[1, 6]}}).count()
1
```

This means that there's only one document whose hobbies array contains objects with perWeek field `1` and `6`.

There are 3 documents whose hobbies array contains objects with perWeek field `6`.

Another example:

<img width="598" alt="Screen Shot 2021-09-25 at 10 20 30 AM" src="https://user-images.githubusercontent.com/31994778/134762783-25ab99a9-a45b-447a-a533-319e8f303277.png">

---

<h4>$elemMatch</h4>

So, let's say that we these documents in our db...

<img width="407" alt="Screen Shot 2021-09-19 at 11 07 41 PM" src="https://user-images.githubusercontent.com/31994778/133941581-87dca04d-c5cf-4635-88c8-2d550544dd0e.png">

And, say that we want to find the documents with hobbie name cooking and perWeek is 3.

Try

`db.customers.find({"$and":[{"hobbies.name":"cooking"}, {"hobbies.perWeek":3}]})`

Returns

<img width="522" alt="Screen Shot 2021-09-19 at 11 18 22 PM" src="https://user-images.githubusercontent.com/31994778/133941851-d959ddbe-c095-4eb0-85ad-b3ae65f9ab6c.png">

What's wrong with this query is that it returns hobbies with name "cooking" and hobbies with perWeek 3. <b>We need to tell MongoDB that we want these be applied to the same document.</b>

For that, use $elemMatch

<b>The $elemMatch operator matches documents that contain an array field with at least one element that matches all the specified query criteria.</b>

```
{ <field>: { $elemMatch: { <query1>, <query2>, ... } } }
```

<img width="650" alt="Screen Shot 2021-09-19 at 11 30 43 PM" src="https://user-images.githubusercontent.com/31994778/133942101-e05ebd8f-29f6-4168-b7b7-b7df5e31c92b.png">

Voila!

---

<p id="cursors">
<h2>Working With Cursors</h2>
</p>

We know that cursor object is to MongoDB generator is to Python.

<h3>sort</h3>

We can do sorting in cursor objects. For example:

`db.customers.find({}).sort({"account.balance":1}).pretty()`

This query basically sorts every document returned by the cursor object in terms of ascending account balance. 

<img width="502" alt="Screen Shot 2021-09-25 at 10 31 32 AM" src="https://user-images.githubusercontent.com/31994778/134763121-6c08d347-94ff-400f-806a-dc94d86420e8.png">

Here, if we use "age":1 in sorting, the last two documents would be switched in place, since "Bob Bates", is older than "Kendrick Jenkins".

`db.customers.find({}, {"_id":0, "address":0, "hobbies":0}).sort({"account.balance":1, "age":1}).pretty()`

<img width="486" alt="Screen Shot 2021-09-25 at 10 33 27 AM" src="https://user-images.githubusercontent.com/31994778/134763226-602d1509-409e-4e0a-9d96-cd77de99906e.png">

---

<h3>skip</h3>

Used mainly for pagination.

For example, we have pages in our website and every page should have two documents...

But before that, let's check this out..

<img width="923" alt="Screen Shot 2021-09-20 at 10 17 44 PM" src="https://user-images.githubusercontent.com/31994778/134061329-65e69d6c-d798-4cee-9ad4-6a0e9f4e093a.png">

Here, we do normal sorting and documents start from Janeth, Homer, Max, and so on.

Now, skip(2)...

<img width="1006" alt="Screen Shot 2021-09-20 at 10 18 53 PM" src="https://user-images.githubusercontent.com/31994778/134061456-4d849b7e-40cc-444c-a352-6d4211130189.png">

Starts from Max, because we skipped the first two.

Now, going back to pagination, let's talk about limit() first.

---

<h3>limit</h3>

Used for limiting the amount of documents fetched.

<img width="1072" alt="Screen Shot 2021-09-20 at 10 21 23 PM" src="https://user-images.githubusercontent.com/31994778/134061739-ef154e39-c28d-4484-9dfe-b40c0a265c7b.png">

This is a good example for a web page that has 2 documents for every page. The next page must have skip(4).limit(2).

Of course, for a pagination with 7 documents, when we come to page 4, only one document will be returned even if limit(2).

<img width="1076" alt="Screen Shot 2021-09-20 at 10 23 18 PM" src="https://user-images.githubusercontent.com/31994778/134062133-a51051b4-541c-443d-b6ca-73c7080ca969.png">

---

<p id="update-operations">
<h2>Deep Dive into Update Operations</h2>
</p>

<h3>Updating Multiple Fields with $set</h3>

Say that I have a DB like this

<img width="449" alt="Screen Shot 2021-09-25 at 10 54 05 AM" src="https://user-images.githubusercontent.com/31994778/134763808-e567cbb1-361d-45f0-a390-a915b63f2b76.png">

Let's add two fields to all the documents.

`db.student_info.updateMany({},{"$set":{"nationality":"Turkish", "school":"TEDU"}})`

Then, all the matching documents will have two new fields, `nationality` and `school`, respectively.

<img width="338" alt="Screen Shot 2021-09-25 at 10 56 32 AM" src="https://user-images.githubusercontent.com/31994778/134763890-123bdb01-6cb4-4e12-8959-c2bf63d90db4.png">

---

<h3>Incrementing and Decrementing Values</h3>

<h4>$inc</h4>

<b>Increments the value of the field by the specified amount.</b> [ref](https://docs.mongodb.com/manual/reference/operator/update-field/)

<b><i>"$inc is an atomic operation within a single document."</i></b>

Syntax:

`{ $inc: { <field1>: <amount1>, <field2>: <amount2>, ... } }`

Use

`db.student_info.updateOne({"_id":ObjectId("614ed57293587d2fb830c1cf")}, {"$inc":{"age":1}})`

This will increase age of "Burakhan Aksoy" by one.

```js
{ name: 'Burakhan',
  last_name: 'Aksoy',
  age: 27,
  nationality: 'Turkish',
  school: 'TEDU' 
  }
```

As an extra, let's say we want to update / create a field named "info" which is an object, we can do as follows:

`db.student_info.find({"name":"Burakhan"}).forEach((doc) => {db.student_info.updateOne({"name":doc.name}, {"$set":{"info.age":doc.age}})})`

And, when we do find again

```js
db.student_info.find({"name":"Burakhan"})
{ _id: ObjectId("614ed57293587d2fb830c1cf"),
  name: 'Burakhan',
  last_name: 'Aksoy',
  age: 27,
  nationality: 'Turkish',
  school: 'TEDU',
  info: { age: 27 } }
```

As we can see, "info" object added to our document with "age" value being equal to the current age field of the document.

We can also decrement by using $inc.

`db.student_info.updateOne({"name":"Burakhan"}, {"$inc":{"age":-1}})`

<img width="409" alt="Screen Shot 2021-09-25 at 1 22 24 PM" src="https://user-images.githubusercontent.com/31994778/134768123-a4c54527-d32d-40f6-bf27-32483390f901.png">

---

<h3>$min</h3>

<b>"Only updates the field if the specified value is less than the existing field value.</b> [ref](https://docs.mongodb.com/manual/reference/operator/update/min/#mongodb-update-up.-min)

The $min updates the value of the field to a specified value if the specified value is less than the current value of the field.

Let's say we have such documents in our student_info collection.

<img width="431" alt="Screen Shot 2021-09-25 at 1 35 22 PM" src="https://user-images.githubusercontent.com/31994778/134768456-00734ecb-4e7f-45d7-a8ad-ad61f7d9f94c.png">

Here, let's find out senior students by looking status field and set required minCredit to 20 for graduation.

`db.student_info.updateMany({"status":"senior"}, {"$min":{"graduationRequirements.minCredit":20}})`

<img width="400" alt="Screen Shot 2021-09-25 at 2 00 53 PM" src="https://user-images.githubusercontent.com/31994778/134769119-e7250be2-ca8b-499f-b1a9-9d4c52bd1e8d.png">

---

Let's do this with aggregation just for the kick of it.

```js
db.student_info.aggregate(
[
   {
      "$match":{
         "status":"senior",
         "graduationRequirements.minCredit":{
            "$gt":20
         }
      }
   },
   {
      "$set":{
         "graduationRequirements.minCredit":20
      }
   },
   {
      "$project":{
         "_id":0,
         "nationality":0
      }
   }
])
```

---

<h3>$max</h3>

<b>The $max operator updates the value of the field to a specified value if the specified value is greater than the current value of the field.</b>[ref](https://docs.mongodb.com/manual/reference/operator/update/max/)

---

<h3>$mul</h3>

<b>Multiply the value of a field by a number. To specify a $mul expression, use the following prototype:</b>

`{ $mul: { <field1>: <number1>, ... } }`

[ref](https://docs.mongodb.com/manual/reference/operator/update/mul/)

---

<h3>Getting Rid of Fields</h3>

We use $unset for removing fields from the document.

<img width="398" alt="Screen Shot 2021-09-25 at 3 47 45 PM" src="https://user-images.githubusercontent.com/31994778/134772181-fd078ca8-34f6-4512-9edc-4fcb84205ee0.png">

Let's remove "nationality" field.

We use `db.student_info.updateMany({}, {"$unset":{"nationality":""}})`

```
{ acknowledged: true,
  insertedId: null,
  matchedCount: 5,
  modifiedCount: 5,
  upsertedCount: 0 }
```

Then

<img width="407" alt="Screen Shot 2021-09-25 at 3 49 04 PM" src="https://user-images.githubusercontent.com/31994778/134772198-c9abab9e-60b5-4031-b58a-929dcbf54676.png">

---

<h3>Renaming a Field</h3>

To rename a field, we use `$rename`.

<b>The $rename operator updates the name of a field.</b> [ref](https://docs.mongodb.com/manual/reference/operator/update/rename/)

So, let us rename `last_name` field to `lastName`.

`db.student_info.updateMany({}, {"$rename":{"last_name":"lastName"}})`

<img width="397" alt="Screen Shot 2021-09-25 at 5 06 10 PM" src="https://user-images.githubusercontent.com/31994778/134774356-e5de3b31-dacf-4b92-a726-352b04308636.png">

---

<div id="upsert">
<h2>Understanding Upsert</h2>
</div>

Upsert can be thought as "<i>update <b>OR</b> insert"</i>.

By default, MongoDB uses upsert=false.

Let's see this on an example.

`db.student_info.updateOne({"name":"spongebob"}, {"$set":{"age":21, "status":"junior", "school":"METU"}})`

Now, we don't have a document with name:"spongebob", and when we try to update that document, we will have the following response:

```js
{ acknowledged: true,
  insertedId: null,
  matchedCount: 0,
  modifiedCount: 0,
  upsertedCount: 0 }
```

Here, matchedCount and modifiedCount are both 0. The former should be 0 of course, since there's no such document. The latter though, is related to upsert.

if we set upsert:true, then MongoDB will create (insert) that document for us, hence the <b>insert</b> in up<b>sert</b>.

Let's try again with upsert:true.

`db.student_info.updateOne({"name":"spongebob"}, {"$set":{"age":21, "status":"junior", "school":"METU"}}, {"upsert":true})`

<img width="445" alt="Screen Shot 2021-09-25 at 5 23 06 PM" src="https://user-images.githubusercontent.com/31994778/134774862-b80ca634-79bc-4eb2-979d-9f3d24d1aada.png">

<b>As we can see, when upsert is set to true, the document is inserted.</b>

<img width="395" alt="Screen Shot 2021-09-25 at 5 26 03 PM" src="https://user-images.githubusercontent.com/31994778/134774923-d8492c39-6a42-4f52-bed3-e99bb6e3ec58.png">

---

<h3>Updating Matched Array Elements</h3>

We have studied this on `$elemMatch` part. However, we used that with `find()` method. Now, though, we will use $elemMatch with `updateMany()` and `updateOne()` methods.

Switching back to our `bank` db. We have documents like this.

<img width="473" alt="Screen Shot 2021-09-26 at 8 13 31 AM" src="https://user-images.githubusercontent.com/31994778/134794625-232eda93-b66b-46ab-a030-08a7e312ca98.png">

Now, we want to find documents with hobbies named cooking and perWeek gte 3 and update that specific hobby.

`db.customers.find({"hobbies":{"$elemMatch":{"name":"cooking", "perWeek":{"$gte":3}}}})`

With this query, the documents we have are:

<img width="499" alt="Screen Shot 2021-09-26 at 8 19 36 AM" src="https://user-images.githubusercontent.com/31994778/134794740-bb438fc8-8ffa-4d81-978f-b318e217ca04.png">

Now, let's do the update.

`db.customers.updateMany({"hobbies":{"$elemMatch":{"name":"cooking", "perWeek":{"$gte":3}}}}, {"$set":{"hobbies.$.highFrequency":true}})`

```js
{ acknowledged: true,
  insertedId: null,
  matchedCount: 2,
  modifiedCount: 2,
  upsertedCount: 0 }
```

Update is successful. Now let's talk about the use of `$` here in `{"$set":{"hobbies.$.highFrequency":true}}`.

hobbies.<b>$</b>.highFrequency tells us the following:

<b><i>"I will find hobbies array's that specific object you searched for and only update that object."</i></b>

When we use find again, we will have:

<img width="762" alt="Screen Shot 2021-09-26 at 8 28 51 AM" src="https://user-images.githubusercontent.com/31994778/134794903-e0443358-e393-4dbe-9bf7-4e852c1b6fdc.png">

---

<h3>Updating All Array Elements</h3>

Previously, we used $ sign in hobbies.$.highFrequency to update only a specific element inside hobbies array.

What happens when we want to update all array elements that matches our query?

Say that we want add a new field `isConsistent:true` to all elements inside hobbies array with perWeek field gte 2.

Can we use something like this?

`db.customers.updateMany({"hobbies":{"$elemMatch":{"perWeek":{"$gte":2}}}}, {"$set":{"hobbies.$.isConsistent":true}})`

only projecting hobbies, we get:

<img width="543" alt="Screen Shot 2021-09-26 at 8 53 10 AM" src="https://user-images.githubusercontent.com/31994778/134795388-8c03e689-a897-426c-9cfe-16b488814679.png">

How about the fields that are underlined? They have perWeek gte 2?

This is because hobbies.$.isConsistent only updates the first element.

For updating all elements in an array, we need to use `$[]`.

`db.customers.updateMany({"hobbies":{"$elemMatch":{"perWeek":{"$gte":2}}}}, {"$set":{"hobbies.$[].isConsistent":true}})`

<img width="579" alt="Screen Shot 2021-09-26 at 9 24 50 AM" src="https://user-images.githubusercontent.com/31994778/134796186-76be9a72-ad8f-4b05-a8a2-9fd295272c7d.png">

But this updates all of the elements in the array, even if perWeek is not gte 2.

For that, let's take a look at the following part.

---

<h3>Update All Documents That Match arrayFilters in an Array</h3>

`db.customers.updateMany({},{"$set":{"hobbies.$[elem].isConsistent":true}}, {multi:true, arrayFilters:[{"elem.perWeek":{"$gt":2}}]})`

```js
{ acknowledged: true,
  insertedId: null,
  matchedCount: 4,
  modifiedCount: 4,
  upsertedCount: 0 }
```

<img width="538" alt="Screen Shot 2021-09-26 at 11 14 38 AM" src="https://user-images.githubusercontent.com/31994778/134799591-9cc93301-4cac-4778-833a-3e7dc1a4d60a.png">

Only added isConsistent:true to elements of hobbies array with perWeek > 2.

For usage of arrayFilters, please take a look at [here](https://docs.mongodb.com/manual/reference/operator/update/positional-filtered/#mongodb-update-up.---identifier--).

<img width="463" alt="Screen Shot 2021-09-26 at 11 17 17 AM" src="https://user-images.githubusercontent.com/31994778/134799675-70fb2681-c71b-4953-8f23-941e13495631.png">

---

<h3>Sorting Elements in Array</h3>

Our documents' hobbies array contains elements with perWeek field.

<img width="542" alt="Screen Shot 2021-09-26 at 11 32 19 AM" src="https://user-images.githubusercontent.com/31994778/134800172-0d8b34f7-f9a9-4d67-a71f-144dce2795e1.png">

As we can see here, these are not sorted. I mean for some documents, perweek greater element comes before perweek smaller, and vice-versa.

For example:

<img width="558" alt="Screen Shot 2021-09-26 at 11 35 52 AM" src="https://user-images.githubusercontent.com/31994778/134800288-7739751c-dc21-4c1f-820f-ccfb0a91e394.png">

For this document, perWeek smaller comes before perWeek greater.

Let's use `db.customers.updateMany({}, {"$push":{"hobbies":{"$each":[], "$sort":{"perWeek":1}}}})`

This will sort our documents' hobbies field based on perWeek.

<img width="563" alt="Screen Shot 2021-09-26 at 11 50 50 AM" src="https://user-images.githubusercontent.com/31994778/134800747-94ef9e7d-e164-45f9-a630-9f36f200b263.png">

Here, please check the [ref](https://docs.mongodb.com/manual/reference/operator/update/sort/):

<img width="787" alt="Screen Shot 2021-09-26 at 11 56 28 AM" src="https://user-images.githubusercontent.com/31994778/134800985-d1867ab9-f558-4f83-9be0-9332379dec24.png">

<img width="780" alt="Screen Shot 2021-09-26 at 11 58 45 AM" src="https://user-images.githubusercontent.com/31994778/134801048-8a88a403-2ab5-40b1-a5b9-7dbf795ace81.png">

So, we have to use $sort with $each and after $push.

<b>$push must be used here and empty array is used with $each if no elements will be added to the array.</b>

Of course, for adding elements to an array we'll do the same thing but $each won't be empty array.

For example:

`db.customers.updateOne({"name":"Burakhan"}, {"$push":{"hobbies":{"$each":[{"name":"reading tech article", "perWeek":2}], "$sort":{"perWeek":1}}}})`

<img width="539" alt="Screen Shot 2021-09-26 at 12 10 40 PM" src="https://user-images.githubusercontent.com/31994778/134801387-5490d638-6aa4-43cd-9c27-e0d1a983ed73.png">

---

<h3>Removing Elements from Array</h3>

We use `$pull`.

`db.customers.updateOne({"name":"Burakhan"}, {"$pull":{"hobbies":{"name":"reading tech article"}}})`

```js
db.customers.find({"name":"Burakhan"})
{ _id: ObjectId("6150285893587d2fb830c1da"),
  name: 'Burakhan',
  last_name: 'Aksoy',
  age: 26,
  account: { balance: 500, debt: 300, credit: 200 },
  address: { city: 'Istanbul', country: 'Turkey' },
  occupation: 'Junior Python Developer',
  hobbies: 
   [ { name: 'cycling', perWeek: 3, isConsistent: true },
     { name: 'cooking', perWeek: 5, isConsistent: true } ] }
```

For more info, click [here](https://docs.mongodb.com/manual/reference/operator/update/pull/).

---

<h3>$addToSet vs $push</h3>

Both of these operators are used for adding a new element into an array.

However, <b>$addToSet doesn't add duplicate elements.</b>

<h4>Using $push</h4>

Let's update this document:

<img width="544" alt="Screen Shot 2021-09-26 at 2 16 29 PM" src="https://user-images.githubusercontent.com/31994778/134805418-469c7e60-d58a-421f-bc57-8ccc6c14e547.png">

```js
db.customers.updateOne({"name":"Burakhan"}, {"$push":{"hobbies":{"name":"cycling", "perWeek":3, "isConsistent":true}}})
{ acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0 }
```

<h4>Using $addToSet</h4>

```js
db.customers.updateOne({"name":"Burakhan"}, {"$addToSet":{"hobbies":{"name":"cycling", "perWeek":3, "isConsistent":true}}})
{ acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 0,
  upsertedCount: 0 }
```

As we can see, for $push, modifiedCount:1, for $addToSet, modifiedCount:0.

---

<div id="summary-2">
<h3>Summary of Chapter 2</h3>
 </div>
 
 <img width="967" alt="Screen Shot 2021-09-26 at 2 24 44 PM" src="https://user-images.githubusercontent.com/31994778/134805658-44bdc0de-b7ea-494a-b4ac-fe1565f33965.png">

---

<div id="aggregation-framework">
<h2>Aggregation Framework</h2>
 </div>
 
 <b><i>"Retrieving data efficiently, in a structured way"</b></i> [ref](https://pro.academind.com/p/mongodb-the-complete-developer-s-guide-2020)
 
 <img width="634" alt="Screen Shot 2021-09-26 at 11 03 12 PM" src="https://user-images.githubusercontent.com/31994778/134822453-6c354700-47b1-4bbe-a496-b4aa5a4aeef1.png">
 
 
