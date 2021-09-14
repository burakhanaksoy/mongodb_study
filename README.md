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
[Working With Arrays](#arrays)
[Using $gt $lt](#basic-operators)
[Chapter 1 Summary](#summary-1)


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
 
 
