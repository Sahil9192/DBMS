### 1. **Insert Documents into a Collection (Create Operation)**

**Query:**

```javascript
db.books.insertOne({
    title: "The Great Gatsby",
    author: "F. Scott Fitzgerald",
    publishedYear: 1925,
    genres: ["Fiction", "Classic"],
    price: 10.99,
    stock: 50
});
```

**Description:**  
This query inserts a single document into the `books` collection. The document includes information about the book such as title, author, publication year, genres, price, and stock. The `insertOne()` method is used to add this document.

### 2. **Insert Multiple Documents (Create Operation)**

**Question:**  
How do you insert multiple documents at once into the `books` collection?

**Query:**

```javascript
db.books.insertMany([
    {
        title: "To Kill a Mockingbird",
        author: "Harper Lee",
        publishedYear: 1960,
        genres: ["Fiction", "Classic"],
        price: 12.99,
        stock: 40
    },
    {
        title: "1984",
        author: "George Orwell",
        publishedYear: 1949,
        genres: ["Dystopian", "Science Fiction"],
        price: 15.99,
        stock: 30
    }
]);
```

**Description:**  
This query inserts multiple documents into the `books` collection at once using the `insertMany()` method. Each document represents a different book with relevant details.

### 3. **Find Documents (Read Operation)**

**Question:**  
How do you find all books written by a specific author, such as "George Orwell"?

**Query:**

```javascript
db.books.find({ author: "George Orwell" });
```

**Description:**  
This query retrieves all documents from the `books` collection where the `author` field matches "George Orwell". The `find()` method is used to search for documents that meet the specified criteria.

### 4. **Find Documents with Logical Operators**

**Question:**  
How do you find books that are either categorized as "Fiction" or "Dystopian"?

**Query:**

```javascript
db.books.find({
    $or: [
        { genres: "Fiction" },
        { genres: "Dystopian" }
    ]
});
```

**Description:**  
This query uses the `$or` logical operator to find documents where the `genres` field contains either "Fiction" or "Dystopian". The `find()` method with the `$or` operator allows you to search for documents that meet one or more conditions.

### 5. **Update a Document (Update Operation)**

**Question:**  
How do you update the price of the book "1984" to 17.99?

**Query:**

```javascript
db.books.updateOne(
    { title: "1984" },
    { $set: { price: 17.99 } }
);
```

**Description:**  
This query updates the `price` field of the document where the `title` is "1984" to 17.99. The `updateOne()` method updates a single document, and the `$set` operator specifies the fields to be updated.

### 6. **Update Multiple Documents (Update Operation)**

**Question:**  
How do you increase the stock of all books in the "Fiction" genre by 10?

**Query:**

```javascript
db.books.updateMany(
    { genres: "Fiction" },
    { $inc: { stock: 10 } }
);
```

**Description:**  
This query increases the `stock` field by 10 for all documents in the `books` collection where the `genres` field contains "Fiction". The `updateMany()` method is used to update multiple documents, and the `$inc` operator increments the specified field.

### 7. **Delete a Document (Delete Operation)**

**Question:**  
How do you delete a book from the collection where the title is "The Great Gatsby"?

**Query:**

```javascript
db.books.deleteOne({ title: "The Great Gatsby" });
```

**Description:**  
This query deletes the document where the `title` is "The Great Gatsby". The `deleteOne()` method removes a single document that matches the specified criteria.

### 8. **Delete Multiple Documents (Delete Operation)**

**Question:**  
How do you delete all books that have a stock of 0?

**Query:**

```javascript
db.books.deleteMany({ stock: 0 });
```

**Description:**  
This query deletes all documents from the `books` collection where the `stock` field is 0. The `deleteMany()` method removes multiple documents that match the criteria.

### 9. **Save Method**

**Question:**  
How do you insert a new document or update an existing one using the `save()` method?

**Query:**

```javascript
db.books.save({
    _id: ObjectId("60c72b2f9b1e8a3b4c8b4567"), // Existing document ID
    title: "Animal Farm",
    author: "George Orwell",
    publishedYear: 1945,
    genres: ["Political Satire", "Allegory"],
    price: 8.99,
    stock: 20
});
```

**Description:**  
The `save()` method is used to insert a new document or update an existing one if the `_id` field is provided. In this example, the document with the specified `_id` will be updated with the new values, or if the `_id` does not exist, a new document will be inserted.

### 10. **Use of `find()` with Logical Operators**

**Question:**  
How do you find books published between 1950 and 2000?

**Query:**

```javascript
db.books.find({
    publishedYear: { $gte: 1950, $lte: 2000 }
});
```

**Description:**  
This query retrieves all documents from the `books` collection where the `publishedYear` is between 1950 and 2000, inclusive. The `$gte` (greater than or equal to) and `$lte` (less than or equal to) operators are used in combination to specify the range.

