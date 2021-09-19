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


