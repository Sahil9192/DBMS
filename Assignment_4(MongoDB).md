#### **Steps for MongoDB Operations:**

##### **1. Aggregation: Calculate Total Fines per Student**

**Query:**

```javascript
db.fine.aggregate([
    {
        $group: {
            _id: "$Roll_no",
            totalFine: { $sum: "$Amt" }
        }
    },
    {
        $sort: { totalFine: -1 }
    }
]);
```

**Description:**
- **Aggregation** groups fines by `Roll_no` and sums up the fine amounts.
- **Sorting** arranges the results in descending order by the total fine amount.

##### **2. Indexing: Optimize Queries on `Roll_no` and `Name_of_Book`**

**Query:**

```javascript
db.borrower.createIndex({ Roll_no: 1 });
db.borrower.createIndex({ Name_of_Book: 1 });
```

**Description:**
- **Indexing** on `Roll_no` and `Name_of_Book` helps optimize searches and queries on these fields.

##### **3. Map-Reduce: Calculate Average Fine per Student**

**Map-Reduce Operation:**

```javascript
var mapFunction = function() {
    emit(this.Roll_no, this.Amt);
};

var reduceFunction = function(key, values) {
    return Array.sum(values) / values.length;
};

db.fine.mapReduce(
    mapFunction,
    reduceFunction,
    {
        out: "avg_fine_per_student"
    }
);
```

**Description:**
- **Map Function:** Emits `Roll_no` as the key and the fine amount (`Amt`) as the value.
- **Reduce Function:** Computes the average fine amount for each student.
- **Output:** Stores the results in a new collection `avg_fine_per_student`.

---

These approaches cover both the PL/SQL and MongoDB requirements, with easy-to-understand queries and detailed explanations to include in your notebook for better understanding.
