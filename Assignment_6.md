### **PL/SQL Block Using a Parameterized Cursor to Merge Data**

#### **Step 1: Create the Tables (For Example)**
First, let's assume the structure of the `N_Roll_Call` and `O_Roll_Call` tables.

```sql
CREATE TABLE N_Roll_Call (
    Roll_No NUMBER PRIMARY KEY,
    Name VARCHAR2(50),
    Date_of_Call DATE
);

CREATE TABLE O_Roll_Call (
    Roll_No NUMBER PRIMARY KEY,
    Name VARCHAR2(50),
    Date_of_Call DATE
);
```

#### **Step 2: Write the PL/SQL Block**
The PL/SQL block will use a parameterized cursor to fetch data from `N_Roll_Call` and insert it into `O_Roll_Call` if it does not already exist.

```sql
DECLARE
    -- Declare a parameterized cursor
    CURSOR c_roll_call (p_roll_no NUMBER) IS
        SELECT Roll_No, Name, Date_of_Call
        FROM N_Roll_Call
        WHERE Roll_No = p_roll_no;

    -- Variable to hold fetched data
    v_roll_no N_Roll_Call.Roll_No%TYPE;
    v_name N_Roll_Call.Name%TYPE;
    v_date_of_call N_Roll_Call.Date_of_Call%TYPE;

    -- Variable to check if the record exists in O_Roll_Call
    v_count NUMBER;

BEGIN
    -- Loop through each record in N_Roll_Call
    FOR rec IN (SELECT Roll_No FROM N_Roll_Call) LOOP
        -- Open the parameterized cursor
        OPEN c_roll_call(rec.Roll_No);
        FETCH c_roll_call INTO v_roll_no, v_name, v_date_of_call;
        CLOSE c_roll_call;

        -- Check if the record already exists in O_Roll_Call
        SELECT COUNT(*)
        INTO v_count
        FROM O_Roll_Call
        WHERE Roll_No = v_roll_no;

        -- If the record does not exist, insert it
        IF v_count = 0 THEN
            INSERT INTO O_Roll_Call (Roll_No, Name, Date_of_Call)
            VALUES (v_roll_no, v_name, v_date_of_call);
        END IF;
    END LOOP;

    -- Commit the transaction
    COMMIT;
END;
/
```

#### **Explanation:**

1. **Cursor Declaration (`c_roll_call`)**: A parameterized cursor is declared, which takes `Roll_No` as a parameter and retrieves the corresponding record from `N_Roll_Call`.

2. **Loop Through `N_Roll_Call`**: A `FOR` loop iterates through each `Roll_No` in `N_Roll_Call`.

3. **Cursor Usage**: For each `Roll_No`, the cursor is opened to fetch the data, and it is checked whether that `Roll_No` already exists in `O_Roll_Call`.

4. **Inserting Data**: If the `Roll_No` does not exist in `O_Roll_Call`, the data is inserted.

5. **Commit**: The transaction is committed to save the changes.

---

### **Additional Notes:**

- **Parameterized Cursor**: This cursor allows you to pass parameters dynamically, making it flexible for fetching specific records.
- **Explicit Cursor**: Here, the cursor is explicitly declared and used, which gives more control over the data fetching process.

This code snippet is a complete solution to merge data between two tables while ensuring no duplicate entries. The comments included within the code should help you understand each step as you review or implement the block in your environment.
