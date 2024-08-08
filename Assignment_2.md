### Sample Database Schema

1. **Authors Table:**

```sql
CREATE TABLE Authors (
    AuthorID INT PRIMARY KEY,
    AuthorName VARCHAR(100) NOT NULL,
    BirthYear INT,
    Country VARCHAR(50)
);
```

2. **Books Table:**

```sql
CREATE TABLE Books (
    BookID INT AUTO_INCREMENT PRIMARY KEY,
    Title VARCHAR(200) NOT NULL,
    AuthorID INT,
    PublishedYear INT,
    Price DECIMAL(10, 2),
    CONSTRAINT fk_author FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID)
);
```

3. **Customers Table:**

```sql
CREATE TABLE Customers (
    CustomerID INT AUTO_INCREMENT PRIMARY KEY,
    CustomerName VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    PhoneNumber VARCHAR(15),
    Address VARCHAR(200)
);
```

4. **Orders Table:**

```sql
CREATE TABLE Orders (
    OrderID INT AUTO_INCREMENT PRIMARY KEY,
    CustomerID INT,
    OrderDate DATE NOT NULL,
    TotalAmount DECIMAL(10, 2),
    CONSTRAINT fk_customer FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```

5. **OrderDetails Table:**

```sql
CREATE TABLE OrderDetails (
    OrderDetailID INT AUTO_INCREMENT PRIMARY KEY,
    OrderID INT,
    BookID INT,
    Quantity INT NOT NULL,
    Price DECIMAL(10, 2) NOT NULL,
    CONSTRAINT fk_order FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    CONSTRAINT fk_book FOREIGN KEY (BookID) REFERENCES Books(BookID)
);
```

### SQL Queries

1. **Inner Join:**

Retrieve all orders along with the customer details.

```sql
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate, Orders.TotalAmount
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

2. **Left Join:**

Get all books and their authors, including books that do not have an author (if applicable).

```sql
SELECT Books.Title, Authors.AuthorName
FROM Books
LEFT JOIN Authors ON Books.AuthorID = Authors.AuthorID;
```

3. **Right Join:**

Retrieve all authors and the books they have written, including authors who have not written any books.

```sql
SELECT Authors.AuthorName, Books.Title
FROM Authors
RIGHT JOIN Books ON Authors.AuthorID = Books.AuthorID;
```

4. **Full Outer Join:**

MySQL does not support `FULL OUTER JOIN` directly, but you can simulate it using `UNION` of `LEFT JOIN` and `RIGHT JOIN`.

```sql
SELECT Authors.AuthorName, Books.Title
FROM Authors
LEFT JOIN Books ON Authors.AuthorID = Books.AuthorID

UNION

SELECT Authors.AuthorName, Books.Title
FROM Authors
RIGHT JOIN Books ON Authors.AuthorID = Books.AuthorID;
```

5. **Self Join:**

Find books by the same author, showing pairs of books written by the same author.

```sql
SELECT b1.Title AS Book1, b2.Title AS Book2, a.AuthorName
FROM Books b1
INNER JOIN Books b2 ON b1.AuthorID = b2.AuthorID AND b1.BookID <> b2.BookID
INNER JOIN Authors a ON b1.AuthorID = a.AuthorID;
```

6. **Sub-Query (Scalar Subquery):**

Find the customer who has made the highest total amount of purchases.

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

7. **Sub-Query (Correlated Subquery):**

List all books that have been ordered more than the average quantity ordered for all books.

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

8. **View Creation:**

Create a view to show the total number of books ordered and their total price.

```sql
CREATE VIEW BookOrderSummary AS
SELECT Books.Title, SUM(OrderDetails.Quantity) AS TotalQuantity, SUM(OrderDetails.Price * OrderDetails.Quantity) AS TotalPrice
FROM OrderDetails
INNER JOIN Books ON OrderDetails.BookID = Books.BookID
GROUP BY Books.Title;
```

9. **Query Using a View:**

Select all records from the view created above.

```sql
SELECT * FROM BookOrderSummary;
```

10. **Sub-Query with `EXISTS`:**

Find customers who have placed at least one order.

```sql
SELECT CustomerName
FROM Customers
WHERE EXISTS (
    SELECT 1
    FROM Orders
    WHERE Orders.CustomerID = Customers.CustomerID
);
```

These queries cover various types of joins (inner, left, right, full outer), sub-queries (scalar, correlated), and views, demonstrating a range of SQL capabilities for querying and managing data.
