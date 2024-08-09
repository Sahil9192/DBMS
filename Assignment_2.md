### 1. **Inner Join**

**Description:**
An `INNER JOIN` returns only the rows that have matching values in both tables. In this query, we retrieve all orders along with the customer details. The `Orders` and `Customers` tables are joined on the `CustomerID` column.

```sql
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate, Orders.TotalAmount
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

### 2. **Left Join**

**Description:**
A `LEFT JOIN` returns all rows from the left table (in this case, `Books`), and the matched rows from the right table (`Authors`). If no match is found, NULL values are returned for columns from the right table. This query gets all books and their authors, including books that do not have an author.

```sql
SELECT Books.Title, Authors.AuthorName
FROM Books
LEFT JOIN Authors ON Books.AuthorID = Authors.AuthorID;
```

### 3. **Right Join**

**Description:**
A `RIGHT JOIN` returns all rows from the right table (`Authors`) and the matched rows from the left table (`Books`). If there is no match, NULL values are returned for columns from the left table. This query retrieves all authors and the books they have written, including authors who have not written any books.

```sql
SELECT Authors.AuthorName, Books.Title
FROM Authors
RIGHT JOIN Books ON Authors.AuthorID = Books.AuthorID;
```

### 4. **Full Outer Join**

**Description:**
MySQL does not directly support `FULL OUTER JOIN`, but we can simulate it using a combination of `LEFT JOIN` and `RIGHT JOIN` with `UNION`. This query returns all authors and books, showing pairs where the author and book are linked, and including all authors and books even if there is no match.

```sql
SELECT Authors.AuthorName, Books.Title
FROM Authors
LEFT JOIN Books ON Authors.AuthorID = Books.AuthorID

UNION

SELECT Authors.AuthorName, Books.Title
FROM Authors
RIGHT JOIN Books ON Authors.AuthorID = Books.AuthorID;
```

### 5. **Self Join**

**Description:**
A `SELF JOIN` is used to join a table with itself. This can be useful when you need to compare rows within the same table. In this query, we find pairs of books written by the same author, showing the titles of two different books by the same author.

```sql
SELECT b1.Title AS Book1, b2.Title AS Book2, a.AuthorName
FROM Books b1
INNER JOIN Books b2 ON b1.AuthorID = b2.AuthorID AND b1.BookID <> b2.BookID
INNER JOIN Authors a ON b1.AuthorID = a.AuthorID;
```

### 6. **Scalar Sub-Query**

**Description:**
A scalar sub-query is a sub-query that returns a single value. In this query, we find the customer who has made the highest total amount of purchases by first calculating the total purchase amount for each customer in a sub-query.

```sql
SELECT CustomerName
FROM Customers
WHERE CustomerID = (
    SELECT CustomerID
    FROM Orders
    GROUP BY CustomerID
    ORDER BY SUM(TotalAmount) DESC
    LIMIT 1
);
```

### 7. **Correlated Sub-Query**

**Description:**
A correlated sub-query is a sub-query that references columns from the outer query. In this query, we list all books that have been ordered more than the average quantity ordered for all books. The sub-query calculates the average total quantity ordered for each book.

```sql
SELECT Title
FROM Books
WHERE BookID IN (
    SELECT BookID
    FROM OrderDetails
    GROUP BY BookID
    HAVING SUM(Quantity) > (
        SELECT AVG(total_quantity)
        FROM (
            SELECT SUM(Quantity) AS total_quantity
            FROM OrderDetails
            GROUP BY BookID
        ) subquery
    )
);
```

### 8. **View Creation**

**Description:**
A `VIEW` is a virtual table based on the result of a SELECT query. Views can simplify complex queries by encapsulating them as reusable objects. In this example, we create a view to show the total number of books ordered and their total price.

```sql
CREATE VIEW BookOrderSummary AS
SELECT Books.Title, SUM(OrderDetails.Quantity) AS TotalQuantity, SUM(OrderDetails.Price * OrderDetails.Quantity) AS TotalPrice
FROM OrderDetails
INNER JOIN Books ON OrderDetails.BookID = Books.BookID
GROUP BY Books.Title;
```

### 9. **Query Using a View**

**Description:**
Once a view is created, it can be queried just like a regular table. This query selects all records from the `BookOrderSummary` view created earlier.

```sql
SELECT * FROM BookOrderSummary;
```

### 10. **Sub-Query with `EXISTS`**

**Description:**
The `EXISTS` keyword is used to test for the existence of any record in a sub-query. This query finds customers who have placed at least one order by checking for the existence of orders linked to each customer.

```sql
SELECT CustomerName
FROM Customers
WHERE EXISTS (
    SELECT 1
    FROM Orders
    WHERE Orders.CustomerID = Customers.CustomerID
);
```
