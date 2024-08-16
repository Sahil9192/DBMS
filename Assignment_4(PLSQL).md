
---

### **Option 1: PL/SQL Code Block**

#### **Problem Statement:**
- You are required to write a PL/SQL code block that handles the return of books by students, calculates fines based on the number of days the book is overdue, updates the status of the book, and stores fine details in a table if applicable.

#### **Tables:**

1. **Borrower** table:
    - `Roll_no`: The student's roll number.
    - `Name`: The student's name.
    - `Date_of_Issue`: The date the book was issued.
    - `Name_of_Book`: The name of the issued book.
    - `Status`: The current status of the book (Issued "I" or Returned "R").

2. **Fine** table:
    - `Roll_no`: The student's roll number.
    - `Date`: The date the fine was applied.
    - `Amt`: The fine amount.

#### **PL/SQL Code Block:**

```sql
DECLARE
    v_roll_no      Borrower.Roll_no%TYPE;
    v_name_of_book Borrower.Name_of_Book%TYPE;
    v_issue_date   Borrower.Date_of_Issue%TYPE;
    v_current_date DATE := SYSDATE;
    v_days         NUMBER;
    v_fine_amt     NUMBER := 0;
    fine_exception EXCEPTION;
BEGIN
    -- Accept input from the user
    v_roll_no := '&Roll_no';
    v_name_of_book := '&Name_of_Book';

    -- Fetch the issue date from the Borrower table
    SELECT Date_of_Issue INTO v_issue_date
    FROM Borrower
    WHERE Roll_no = v_roll_no AND Name_of_Book = v_name_of_book;

    -- Calculate the number of days since the book was issued
    v_days := v_current_date - v_issue_date;

    -- Calculate fine based on the number of days
    IF v_days > 30 THEN
        v_fine_amt := (v_days - 30) * 50 + (30 * 5);
    ELSIF v_days >= 15 THEN
        v_fine_amt := v_days * 5;
    END IF;

    -- Update the status of the book in the Borrower table
    UPDATE Borrower
    SET Status = 'R'
    WHERE Roll_no = v_roll_no AND Name_of_Book = v_name_of_book;

    -- If a fine is applicable, store the details in the Fine table
    IF v_fine_amt > 0 THEN
        INSERT INTO Fine (Roll_no, Date, Amt)
        VALUES (v_roll_no, v_current_date, v_fine_amt);
    END IF;

    DBMS_OUTPUT.PUT_LINE('Book return processed successfully.');

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No such record found for Roll_no: ' || v_roll_no || ' and Name_of_Book: ' || v_name_of_book);
    WHEN OTHERS THEN
        RAISE fine_exception;
    WHEN fine_exception THEN
        DBMS_OUTPUT.PUT_LINE('An error occurred while processing the fine.');
END;
/
```

#### **Description:**
- **Control Structure:** The `IF-ELSIF` structure is used to calculate the fine based on the number of days overdue.
- **Exception Handling:** The `EXCEPTION` block handles scenarios like no record found (`NO_DATA_FOUND`) and other errors using a user-defined exception (`fine_exception`).

---
