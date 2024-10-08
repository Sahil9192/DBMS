### **1. MySQL Implementation**

#### **Step 1: Create the Tables**

```sql
CREATE TABLE Borrower (
    Roll_no INT PRIMARY KEY,
    Name VARCHAR(100),
    Date_of_Issue DATE,
    Name_of_Book VARCHAR(100),
    Status CHAR(1)
);

CREATE TABLE Fine (
    Roll_no INT,
    Date DATE,
    Amt DECIMAL(10, 2),
    FOREIGN KEY (Roll_no) REFERENCES Borrower(Roll_no)
);
```
- **Explanation:** These commands create the `Borrower` and `Fine` tables. The `Borrower` table records the book borrowing information, while the `Fine` table stores fines if applicable.

#### **Step 2: Create the Stored Procedure**

```sql
DELIMITER $$

CREATE PROCEDURE ProcessBookReturn(IN p_roll_no INT, IN p_name_of_book VARCHAR(100))
BEGIN
    DECLARE v_issue_date DATE;
    DECLARE v_current_date DATE DEFAULT CURDATE();
    DECLARE v_days INT;
    DECLARE v_fine_amt DECIMAL(10, 2) DEFAULT 0;

    -- Fetch the issue date from the Borrower table
    SELECT Date_of_Issue INTO v_issue_date
    FROM Borrower
    WHERE Roll_no = p_roll_no AND Name_of_Book = p_name_of_book;

    -- Calculate the number of days since the book was issued
    SET v_days = DATEDIFF(v_current_date, v_issue_date);

    -- Calculate fine based on the number of days
    IF v_days > 30 THEN
        SET v_fine_amt = (v_days - 30) * 50 + (30 * 5);
    ELSEIF v_days >= 15 THEN
        SET v_fine_amt = v_days * 5;
    END IF;

    -- Update the status of the book in the Borrower table
    UPDATE Borrower
    SET Status = 'R'
    WHERE Roll_no = p_roll_no AND Name_of_Book = p_name_of_book;

    -- If a fine is applicable, store the details in the Fine table
    IF v_fine_amt > 0 THEN
        INSERT INTO Fine (Roll_no, Date, Amt)
        VALUES (p_roll_no, v_current_date, v_fine_amt);
    END IF;

    SELECT 'Book return processed successfully.' AS message;
END$$

DELIMITER ;
```

- **Explanation:** This stored procedure `ProcessBookReturn` handles the return of books, calculates fines, updates the status of the book, and inserts fine details into the `Fine` table if necessary. It uses control structures like `IF` and exception handling via logical conditions.

#### **Step 3: Call the Stored Procedure**

```sql
CALL ProcessBookReturn(101, 'The Great Gatsby');
```

- **Explanation:** This command calls the stored procedure with specific parameters (`Roll_no` and `Name_of_Book`), triggering the book return process.

---

