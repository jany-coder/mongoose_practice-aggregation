# mongoose Practice Aggregation

# Example data:

```
[
   {
     "name": "John Doe",
     "email": "johndoe@example.com",
     "age": 28,
     "address": {
       "street": "123 Main St",
       "city": "New York",
       "state": "NY",
       "zipcode": "10001"
     },
     "favorites": {
       "color": "blue",
       "food": "pizza",
       "movie": "The Shawshank Redemption"
     },
     "friends": [
       {
         "name": "Jane Smith",
         "email": "janesmith@example.com"
       },
       {
         "name": "Mike Johnson",
         "email": "mikejohnson@example.com"
       }
     ]
   },
   {
     "name": "Alice Williams",
     "email": "alicewilliams@example.com",
     "age": 35,
     "address": {
       "street": "456 Elm St",
       "city": "San Francisco",
       "state": "CA",
       "zipcode": "94101"
     },
     "favorites": {
       "color": "green",
       "food": "sushi",
       "movie": "The Godfather"
     },
     "friends": [
       {
         "name": "Bob Anderson",
         "email": "bobanderson@example.com"
       },
       {
         "name": "Emily Davis",
         "email": "emilydavis@example.com"
       }
     ]
   }
 ]
```

**Task 1:** Find all users who are located in New York.



**find()**

```
db.collection.find({ "address.city": "New York" })
```

**aggregate([])**

```
db.getCollection("practice-query").aggregate([
   {
       $match: { "address.city": "New York" }
   }
]);
```

**Task 2:** Find the user(s) with the email **"johndoe@example.com"** and retrieve their favorite movie.



**find()**

```
db.getCollection("practice-query").find({"email": "johndoe@example.com"}, {"favorites.movie" : 1})
```

**aggregate([])**

```
db.getCollection("practice-query").aggregate([
    {
        $match: { "email": "johndoe@example.com"}
    },
    {
        $project: {
            "favorites.movie" : 1,
        }
    }
]);
```

**Task 3:** Find all users with "pizza" as their favorite food and sort them according to age.



**aggregate([])**

```
db.getCollection("practice-query").aggregate([
    {
        $match: { "favorites.food": "pizza" }
    },
    {
        $sort : {"age" : 1}
    }
]);
```

**Task 4:** Find all users over 30 whose favorite color is "green".



**aggregate([])**

```
db.getCollection("practice-query").aggregate([
    {
        $match: {
            $and: [
                {"age" : {$gt : 30}},
                {"favorites.color" : "green"}
            ]
        }
    }
]);
```

**Task 5:** Count the number of users whose favorite movie is "The Shawshank Redemption".



**aggregate([])**

```
db.getCollection("practice-query").aggregate([
   {
       $match: {"favorites.movie" : "The Shawshank Redemption"}
   },
   {
       $count: "count"
   }
]);
```

**Task 6:** Update the zipcode of the user with the email **"johndoe@example.com"** to "10002".



**updateOne()**

```
db.getCollection("practice-query").updateOne(
    { "email": "johndoe@example.com" },
    { $set: { "address.zipcode": "10002" } }
)
```

**Task 7:** Delete the user with the email **"alicewilliams@example.com"** from the user data.



**deleteOne()**

```
db.getCollection("practice-query").deleteOne(
  { "email": "alicewilliams@example.com" }
)
```

**Task 8:** Group users by their favorite movie and retrieve the average age in each movie group.



**aggregate([])**

```
db.getCollection("practice-query").aggregate([
    {
        $group: {
            _id: "favorites.movie",
            averageAge: { $avg: "$age" }
        }
    }
]);
```

**Task 9:** Calculate the average age of users with a favorite " pizza " food.



**aggregate([])**

```
db.getCollection("practice-query").aggregate([
    {
        $match: { "favorites.food": "pizza" }
    },
    {
        $group: {
            _id: null,
            averageAge: { $avg: "$age" }
        }
    },
    {
        $project: {
            _id: 0,
            averageAge: {$toInt: "$averageAge"}
        }
    }
]);
```

<------------------------------------------------------------------------------>

```
// orders
[
  {
    "_id": 1,
    "order_number": "ORD-001",
    "customer_id": 1,
    "total_amount": 100.0
  },
  {
    "_id": 2,
    "order_number": "ORD-002",
    "customer_id": 2,
    "total_amount": 150.0
  },
  {
    "_id": 3,
    "order_number": "ORD-003",
    "customer_id": 1,
    "total_amount": 200.0
  }
]

// customers
[
  {
    "_id": 1,
    "name": "Alice Williams",
    "email": "alice@example.com"
  },
  {
    "_id": 2,
    "name": "Bob Anderson",
    "email": "bob@example.com"
  },
  {
    "_id": 3,
    "name": "Emily Davis",
    "email": "emily@example.com"
  }
]
```

**Task 10:** Perform a lookup aggregation to retrieve the orders data along with the customer details for each order.



```
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customer_id",
      foreignField: "_id",
      as: "customer"
    }
  }
])

```

# More Tasks on Aggregation

**Task 1:** Group users by their favorite color and retrieve the count of users in each color group.



```
db.getCollection("practice-query").aggregate([
    {
        $group: {
            _id: "$favorites.color",
            count: { $sum: 1 }
        }
    }
])
```

**Task 2:** Find the user(s) with the highest age.



```
db.getCollection("practice-query").aggregate([
    { $sort: { "age": -1 } },
    { $limit: 1 }
])
```

**Task 3:** Find the most common favorite food among all users.



```
    {
        $group: {
            _id: "$favorites.food",
            count: { $sum: 1 }
        }
    },
    {
        $sort: { count: -1 }
    },
    {
        $limit: 1
    },
    {
        $project: {
            _id: 0,
            favoriteFood: "$_id",
            count: 1
        }
}
```

**Task 4:** Calculate the total count of friends across all users.

```
 db.getCollection("practice-query").aggregate([
    { $unwind: "$friends" },
    {
        $group: {
            _id: null,
            count: { "$sum": 1 }
        }
    }
])
```

**Task 5:** Find the user(s) with the longest name.

```
db.getCollection("practice-query").aggregate([
    {
        $project: {
            nameLength: { $strLenCP: "$name" },
        }
    },
    { $sort: { nameLength: -1 } },
    { $limit: 1 }
])
```

__Task 6:__ Calculate each state's total number of users in the address field.

```
db.getCollection("practice-query").aggregate([
    {
        $group: {
            _id: "$address.state",
            count: {$sum: 1 }
        }
    }
])
```

__Task 7:__ Find the user(s) with the highest number of friends.

```
db.getCollection("practice-query").aggregate([
    { $unwind: "$friends" },
    {
        $group: {
            _id: "$name",
            count: { $sum: 1 }
        }
    },
    { $sort: { count: -1 } },
    { $limit: 1 }
])
```
