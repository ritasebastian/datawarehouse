### **Change Data Capture (CDC) Types with Examples (Type 1, Type 2, Type 3)**
Change Data Capture (**CDC**) is used in **Data Warehousing** to **track changes** in dimension tables over time. There are **three main types**:

---

## **1️⃣ Type 1: Overwrite (No History)**
- **Old data is overwritten** with the new data.
- **No history is maintained**.
- Best when **historical changes are not required**.

📌 **Example: `dim_customer` (Before Update)**
| customer_id | customer_name | country |
|------------|--------------|--------|
| 101        | Alice Smith  | USA    |
| 102        | Bob Johnson  | Canada |

#### **🔹 Update: Alice moves to the UK**
```sql
UPDATE dim_customer
SET country = 'UK'
WHERE customer_id = 101;
```

📌 **`dim_customer` (After Update)**  
| customer_id | customer_name | country |
|------------|--------------|--------|
| 101        | Alice Smith  | UK     |
| 102        | Bob Johnson  | Canada |

✅ **Advantage:** Simple & uses less storage.  
❌ **Disadvantage:** **No history of previous values**.  

---

## **2️⃣ Type 2: Maintain History (New Row for Each Change)**
- **New records are inserted** when data changes.
- **Tracks historical changes** using `start_date` and `end_date`.
- **End-date the old record** (`NULL` for active row).

📌 **Example: `dim_customer` (Before Update)**
| customer_sk | customer_id | customer_name | country | start_date | end_date |
|------------|------------|--------------|--------|------------|----------|
| 201        | 101        | Alice Smith  | USA    | 2023-01-01 | NULL     |

#### **🔹 Alice moves to the UK**
```sql
UPDATE dim_customer
SET end_date = '2024-03-01'
WHERE customer_id = 101 AND end_date IS NULL;

INSERT INTO dim_customer (customer_id, customer_name, country, start_date, end_date)
VALUES (101, 'Alice Smith', 'UK', '2024-03-02', NULL);
```

📌 **`dim_customer` (After Update)**
| customer_sk | customer_id | customer_name | country | start_date | end_date |
|------------|------------|--------------|--------|------------|----------|
| 201        | 101        | Alice Smith  | USA    | 2023-01-01 | 2024-03-01 |
| 202        | 101        | Alice Smith  | UK     | 2024-03-02 | NULL     |

✅ **Advantage:** Maintains **full history**.  
❌ **Disadvantage:** **Consumes more storage** (new rows for every change).  

---

## **3️⃣ Type 3: Maintain Partial History (Add New Column for Previous Value)**
- Adds a **new column (`previous_country`)** to store old values.
- Tracks **only the most recent change**.
- Good for **"Before vs After" comparisons**.

📌 **Example: `dim_customer` (Before Update)**
| customer_id | customer_name | country | previous_country |
|------------|--------------|--------|-----------------|
| 101        | Alice Smith  | USA    | NULL            |

#### **🔹 Alice moves to the UK**
```sql
UPDATE dim_customer
SET previous_country = country, 
    country = 'UK'
WHERE customer_id = 101;
```

📌 **`dim_customer` (After Update)**
| customer_id | customer_name | country | previous_country |
|------------|--------------|--------|-----------------|
| 101        | Alice Smith  | UK     | USA             |

✅ **Advantage:** **Uses less storage** than Type 2.  
❌ **Disadvantage:** **Tracks only the last change**, not full history.  

---

### **🚀 Summary Table**
| CDC Type  | **Method** | **Tracks Full History?** | **Storage Impact** | **Best For** |
|-----------|-----------|------------------|---------------|------------|
| **Type 1** | Overwrite | ❌ No | 🔹 Low | Simple updates, no history needed |
| **Type 2** | Insert new row | ✅ Yes | 🔺 High | Full historical tracking |
| **Type 3** | Add a column | 🟡 Partial | 🔸 Medium | Keeping only the last change |
---
