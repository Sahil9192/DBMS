### SQL DDL Statements

**DDL (Data Definition Language)** statements are used to define and modify database structures such as tables, indexes, and views.

1. **Creating Tables:**

Tables are fundamental structures in SQL where data is stored. Here’s an example of creating tables with different constraints.

```sql
-- Create Authors Table
CREATE TABLE Authors (
    AuthorID INT PRIMARY KEY,
    AuthorName VARCHAR(100) NOT NULL,
    BirthYear INT,
    Country VARCHAR(50)
);

-- Create Books Table with a foreign key constraint
CREATE TABLE Books (
    BookID INT PRIMARY KEY,
    Title VARCHAR(200) NOT NULL,
    AuthorID INT,
    PublishedYear INT,
    Price DECIMAL(10, 2),
    CONSTRAINT fk_author FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID)
);
```

2. **Creating a View:**

A **view** is a virtual table based on the result-set of an SQL query.

```sql
-- Create a view to display detailed book information
CREATE VIEW BookDetails AS
SELECT 
    Books.BookID,
    Books.Title,
    Authors.AuthorName,
    Books.PublishedYear,
    Books.Price
FROM 
    Books
JOIN Authors ON Books.AuthorID = Authors.AuthorID;
```

3. **Creating an Index:**

An **index** improves the speed of data retrieval operations on a table at the cost of additional writes and storage space.

```sql
-- Create an index on the Title column in the Books table
CREATE INDEX idx_book_title ON Books(Title);
```

4. **Creating a Sequence:**

A **sequence** generates a sequence of numeric values according to specified rules.

```sql
-- Create a sequence for generating BookID
CREATE SEQUENCE seq_book_id
START WITH 1
INCREMENT BY 1
NOCACHE;
```

5. **Creating a Synonym:**

A **synonym** is an alternative name for database objects.

```sql
-- Create a synonym for the Authors table
CREATE SYNONYM Syn_Authors FOR Authors;
```

### SQL DML Statements

**DML (Data Manipulation Language)** statements are used for managing data within schema objects. It primarily deals with operations like SELECT, INSERT, UPDATE, and DELETE.

1. **Inserting Data:**

The **INSERT** statement is used to add new rows to a table.

```sql
-- Insert data into Authors table
INSERT INTO Authors (AuthorID, AuthorName, BirthYear, Country) VALUES (1, 'George Orwell', 1903, 'United Kingdom');
INSERT INTO Authors (AuthorID, AuthorName, BirthYear, Country) VALUES (2, 'Harper Lee', 1926, 'United States');

-- Insert data into Books table
INSERT INTO Books (BookID, Title, AuthorID, PublishedYear, Price) VALUES (1, '1984', 1, 1949, 15.99);
INSERT INTO Books (BookID, Title, AuthorID, PublishedYear, Price) VALUES (2, 'To Kill a Mockingbird', 2, 1960, 10.99);
```

2. **Selecting Data:**

The **SELECT** statement is used to fetch data from a database.

```sql
-- Select all books
SELECT * FROM Books;

-- Select books with price greater than 12
SELECT * FROM Books WHERE Price > 12;
```

3. **Updating Data:**

The **UPDATE** statement is used to modify existing records in a table.

```sql
-- Update the price of a book
UPDATE Books
SET Price = 12.99
WHERE BookID = 2;
```

4. **Deleting Data:**

The **DELETE** statement is used to remove existing records from a table.

```sql
-- Delete a book
DELETE FROM Books
WHERE BookID = 1;
```

5. **Using Operators and Functions:**

Operators and functions can be used in SQL queries for more complex operations.

```sql
-- Select books published before 1950
SELECT * FROM Books WHERE PublishedYear < 1950;

-- Select the total number of books
SELECT COUNT(*) AS TotalBooks FROM Books;

-- Select the average price of books
SELECT AVG(Price) AS AveragePrice FROM Books;

-- Select all books with 'Kill' in the title
SELECT * FROM Books WHERE Title LIKE '%Kill%';

-- Select the author who has written the most books
SELECT AuthorID, COUNT(*) AS BookCount FROM Books GROUP BY AuthorID ORDER BY BookCount DESC LIMIT 1;
```

