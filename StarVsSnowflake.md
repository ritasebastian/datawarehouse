### **Star Schema vs. Snowflake Schema**
Both **Star Schema** and **Snowflake Schema** are used in **Data Warehousing** to organize **fact and dimension tables**. The key difference is **normalization**.

---

## **1️⃣ Star Schema**
A **Star Schema** has a **central fact table** connected **directly** to multiple **denormalized dimension tables**.

### **Characteristics:**
✔ **Denormalized** (fewer joins, more redundant data)  
✔ **Faster Queries** (optimized for OLAP)  
✔ **Simpler Structure** (easy to understand & maintain)  
❌ **More Storage Space** (data redundancy)

### **Example of Star Schema**
```
                +------------------+
                |    Date_Dim      |
                |------------------|
                | Date_ID (PK)     |
                | Date             |
                | Month            |
                | Year             |
                +--------+---------+
                         |
                         |
       +-----------------+-----------------+
       | Sales_Fact (Fact Table)           |
       |-----------------------------------|
       | Order_ID (PK)                     |
       | Product_ID (FK)                    |
       | Customer_ID (FK)                   |
       | Date_ID (FK)                       |
       | Revenue                           |
       | Quantity_Sold                     |
       +--------+---------+----------------+
                |         |
     +----------+         +-------------+
     | Product_Dim       | Customer_Dim |
     |------------------ |-------------|
     | Product_ID (PK)   | Customer_ID (PK)|
     | Product_Name      | Customer_Name  |
     | Category          | Region         |
     | Supplier          | Country        |
     +------------------ +---------------+
```

👉 Here, `Product_Dim` and `Customer_Dim` **store all information in one table**.

---

## **2️⃣ Snowflake Schema**
A **Snowflake Schema** **normalizes** the dimension tables by splitting them into **sub-dimensions**.

### **Characteristics:**
✔ **Normalized** (less redundancy, saves storage)  
✔ **Better Data Integrity** (no duplicate data)  
❌ **Slower Queries** (more `JOIN` operations)  
❌ **More Complex Structure** (harder to maintain)

### **Example of Snowflake Schema**
```
                         +----------------+
                         |    Date_Dim    |
                         |----------------|
                         | Date_ID (PK)   |
                         | Year           |
                         | Month          |
                         | Day            |
                         +--------+-------+
                                  |
                          +-------+-------+
                          | Sales_Fact    |
                          |---------------|
                          | Order_ID (PK) |
                          | Product_ID (FK) |
                          | Customer_ID (FK)|
                          | Date_ID (FK)    |
                          | Revenue        |
                          | Quantity_Sold  |
                          +-------+--------+
                                  |       
            +----------------+    |    +-----------------+
            | Product_Dim     |    |    | Customer_Dim    |
            |----------------|    |    |-----------------|
            | Product_ID (PK) |    |    | Customer_ID (PK)|
            | Product_Name    |    |    | Customer_Name   |
            | Category_ID (FK)|    |    | Region_ID (FK)  |
            +--------+-------+    |    +--------+--------+
                     |            |              |
      +--------------+      +--------------+  +--------------+
      | Category_Dim |      | Region_Dim   |  | Supplier_Dim |
      |-------------|      |-------------|  |--------------|
      | Category_ID (PK)|  | Region_ID (PK)|  | Supplier_ID (PK)|
      | Category_Name  |  | Region_Name  |  | Supplier_Name  |
      +---------------+  +--------------+  +---------------+
```

👉 Here, `Category_Dim` and `Region_Dim` are **separate tables**, reducing redundancy but requiring **additional JOINs**.

---

## **3️⃣ Key Differences**
| Feature        | Star Schema ⭐ | Snowflake Schema ❄️ |
|---------------|-------------|----------------|
| **Normalization** | Denormalized (fewer tables) | Normalized (more tables) |
| **Query Performance** | Fast (fewer `JOINs`) | Slower (more `JOINs`) |
| **Storage Space** | Requires more storage (redundant data) | Uses less storage (no redundancy) |
| **Data Integrity** | Lower (duplicate data) | Higher (less duplication) |
| **Complexity** | Simple (easy to understand) | Complex (harder to manage) |
| **Use Case** | Best for **faster read operations & reporting** | Best for **complex data with less redundancy** |

---

## **4️⃣ When to Use?**
| Scenario | Best Schema |
|----------|------------|
| **Small to medium-sized datasets** | ✅ Star Schema |
| **Fast query performance needed** | ✅ Star Schema |
| **Minimizing storage costs** | ✅ Snowflake Schema |
| **Reducing data redundancy** | ✅ Snowflake Schema |
| **More complex relationships in dimensions** | ✅ Snowflake Schema |

---

## **5️⃣ SQL Query Example for Both Schemas**
### **Star Schema Query**
```sql
SELECT c.Customer_Name, p.Product_Name, SUM(s.Revenue) AS Total_Revenue
FROM Sales_Fact s
JOIN Customer_Dim c ON s.Customer_ID = c.Customer_ID
JOIN Product_Dim p ON s.Product_ID = p.Product_ID
GROUP BY c.Customer_Name, p.Product_Name;
```

### **Snowflake Schema Query**
```sql
SELECT c.Customer_Name, r.Region_Name, p.Product_Name, cat.Category_Name, SUM(s.Revenue) AS Total_Revenue
FROM Sales_Fact s
JOIN Customer_Dim c ON s.Customer_ID = c.Customer_ID
JOIN Region_Dim r ON c.Region_ID = r.Region_ID
JOIN Product_Dim p ON s.Product_ID = p.Product_ID
JOIN Category_Dim cat ON p.Category_ID = cat.Category_ID
GROUP BY c.Customer_Name, r.Region_Name, p.Product_Name, cat.Category_Name;
```
👉 **Snowflake requires more `JOINs`**, making it **slower**.

---

## **6️⃣ Conclusion**
✅ **Star Schema is Best for:**
- Faster query performance (fewer joins).
- Simpler design.
- Suitable for OLAP and Business Intelligence.

✅ **Snowflake Schema is Best for:**
- Efficient storage & reducing redundancy.
- Handling frequent updates in dimensions.
- When data integrity is critical.

