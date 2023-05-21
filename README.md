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

 __Task 1:__ Find all users who are located in New York.

 Answer: 
 ```
 db.getCollection("practice-query").aggregate([
    {
        $match: { "address.city": "New York" }
    }
]);
```
