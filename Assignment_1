### DDL Statements

1. **Creating Tables**

```sql
-- Create Authors Table
CREATE TABLE Authors (
    AuthorID INT PRIMARY KEY,
    AuthorName VARCHAR(100) NOT NULL,
    BirthYear INT,
    Country VARCHAR(50)
);

-- Create Books Table
CREATE TABLE Books (
    BookID INT PRIMARY KEY,
    Title VARCHAR(200) NOT NULL,
    AuthorID INT,
    PublishedYear INT,
    Price DECIMAL(10, 2),
    CONSTRAINT fk_author FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID)
);

-- Create Customers Table
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    PhoneNumber VARCHAR(15),
    Address VARCHAR(200)
);

-- Create Orders Table
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    OrderDate DATE NOT NULL,
    TotalAmount DECIMAL(10, 2),
    CONSTRAINT fk_customer FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

-- Create OrderDetails Table
CREATE TABLE OrderDetails (
    OrderDetailID INT PRIMARY KEY,
    OrderID INT,
    BookID INT,
    Quantity INT NOT NULL,
    Price DECIMAL(10, 2) NOT NULL,
    CONSTRAINT fk_order FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    CONSTRAINT fk_book FOREIGN KEY (BookID) REFERENCES Books(BookID)
);

```

1. **Creating a View**

```sql
-- Create a view to display detailed order information
CREATE VIEW OrderDetailsView AS
SELECT
    Orders.OrderID,
    Customers.CustomerName,
    Books.Title,
    OrderDetails.Quantity,
    OrderDetails.Price,
    Orders.OrderDate
FROM
    OrderDetails
JOIN Orders ON OrderDetails.OrderID = Orders.OrderID
JOIN Customers ON Orders.CustomerID = Customers.CustomerID
JOIN Books ON OrderDetails.BookID = Books.BookID;

```

1. **Creating an Index**

```sql
-- Create an index on the Title column in the Books table for faster searches
CREATE INDEX idx_book_title ON Books(Title);

```

1. **Creating a Sequence**

```sql
-- Create a sequence for generating OrderID
CREATE SEQUENCE seq_order_id
START WITH 1
INCREMENT BY 1
NOCACHE;

```

1. **Creating a Synonym**

```sql
-- Create a synonym for the Customers table
CREATE SYNONYM Syn_Customers FOR Customers;

```

### DML Statements

Now let's write at least 10 SQL queries to demonstrate the use of DML statements.

1. **Inserting Data**

```sql
-- Insert into Authors
INSERT INTO Authors (AuthorID, AuthorName, BirthYear, Country) VALUES (1, 'George Orwell', 1903, 'United Kingdom');
INSERT INTO Authors (AuthorID, AuthorName, BirthYear, Country) VALUES (2, 'Harper Lee', 1926, 'United States');

-- Insert into Books
INSERT INTO Books (BookID, Title, AuthorID, PublishedYear, Price) VALUES (1, '1984', 1, 1949, 15.99);
INSERT INTO Books (BookID, Title, AuthorID, PublishedYear, Price) VALUES (2, 'To Kill a Mockingbird', 2, 1960, 10.99);

-- Insert into Customers
INSERT INTO Customers (CustomerID, CustomerName, Email, PhoneNumber, Address) VALUES (1, 'John Doe', 'john.doe@example.com', '1234567890', '123 Main St');

-- Insert into Orders
INSERT INTO Orders (OrderID, CustomerID, OrderDate, TotalAmount) VALUES (seq_order_id.NEXTVAL, 1, '2023-08-01', 26.98);

-- Insert into OrderDetails
INSERT INTO OrderDetails (OrderDetailID, OrderID, BookID, Quantity, Price) VALUES (1, 1, 1, 1, 15.99);
INSERT INTO OrderDetails (OrderDetailID, OrderID, BookID, Quantity, Price) VALUES (2, 1, 2, 1, 10.99);

```

1. **Selecting Data**

```sql
-- Select all books
SELECT * FROM Books;

-- Select all customers who have placed orders
SELECT DISTINCT Customers.CustomerName
FROM Customers
JOIN Orders ON Customers.CustomerID = Orders.CustomerID;

-- Select orders with details
SELECT * FROM OrderDetailsView;

```

1. **Updating Data**

```sql
-- Update the price of a book
UPDATE Books
SET Price = 12.99
WHERE BookID = 2;

```

1. **Deleting Data**

```sql
-- Delete an order
DELETE FROM Orders
WHERE OrderID = 1;

```

1. **Using Operators and Functions**

```sql
-- Select books published before 1950
SELECT * FROM Books
WHERE PublishedYear < 1950;

-- Select the total number of orders
SELECT COUNT(*) AS TotalOrders FROM Orders;

-- Select the average price of books
SELECT AVG(Price) AS AveragePrice FROM Books;

-- Select all books with 'Kill' in the title
SELECT * FROM Books
WHERE Title LIKE '%Kill%';

-- Select the customer who has the longest name
SELECT CustomerName FROM Customers
ORDER BY LENGTH(CustomerName) DESC
LIMIT 1;

```

These queries and statements demonstrate a variety of SQL concepts, including the creation and use of different SQL objects, constraints, and various DML operations.
