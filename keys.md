### **Types of Keys in a Data Warehouse (with E-commerce Examples)**  

In a **data warehouse**, different types of keys help maintain **data integrity, uniqueness, and relationships** between tables. Below are the most important types:

---

## **1️⃣ Surrogate Key (SK)**
### **Definition:**  
A **system-generated, unique identifier** (usually an auto-incremented integer or UUID) used as the **primary key** in dimension tables.

📌 **Example in E-commerce (`dim_customer`)**  
| customer_sk | customer_id | customer_name | email              | country |
|------------|------------|--------------|----------------|---------|
| 101        | CUST_001   | Alice Smith  | alice@email.com | USA     |
| 102        | CUST_002   | Bob Johnson  | bob@email.com   | Canada  |

- `customer_sk` is a **surrogate key** (auto-generated).  
- `customer_id` is a **business key** (natural key from the source system).  
- **Surrogate keys don’t change** even if `customer_id` changes.

---

## **2️⃣ Natural Key (NK) / Business Key**
### **Definition:**  
A key **from the source system** that uniquely identifies an entity (e.g., Order Number, Email, SKU).  

📌 **Example in E-commerce (`dim_product`)**  
| product_sk | product_id  | product_name  | category    | price  |
|-----------|-----------|--------------|-----------|------|
| 501       | SKU1234   | Laptop X     | Electronics | 1250 |
| 502       | SKU5678   | Phone Y      | Mobile     | 700  |

- `product_id = SKU1234` is a **natural key** (exists in the operational database).
- `product_sk = 501` is a **surrogate key** in the warehouse.

✅ **Use Case:** When integrating multiple data sources, **natural keys might conflict**, so **surrogate keys** are preferred.

---

## **3️⃣ Primary Key (PK)**
### **Definition:**  
A **column (or set of columns)** that uniquely identifies each row in a table.  

📌 **Example in E-commerce (`dim_store`)**  
| store_sk | store_id  | store_name  | city      |
|---------|---------|----------|--------|
| 301     | STR1001  | Store A  | New York |
| 302     | STR1002  | Store B  | Toronto  |

- `store_sk` is the **Primary Key (PK)** (ensures uniqueness).  
- `store_id = STR1001` is a business key from the source system.

✅ **Best Practice:** Use **surrogate keys as PKs** for consistency.

---

## **4️⃣ Foreign Key (FK)**
### **Definition:**  
A column in one table that **refers to the primary key of another table**, establishing **relationships**.

📌 **Example in E-commerce (`fact_sales`)**
| sale_id | date_sk  | customer_sk | product_sk | store_sk | sales_amount | quantity |
|--------|--------|------------|------------|---------|-------------|---------|
| 10001  | 20240301 | 101        | 501        | 301     | 250.00      | 2       |
| 10002  | 20240302 | 102        | 502        | 302     | 100.00      | 1       |

- `customer_sk`, `product_sk`, `store_sk` are **foreign keys** linking to dimension tables.
- This ensures **referential integrity** between facts and dimensions.

✅ **Use Case:** Enables **fast joins** between tables for querying.

---

## **5️⃣ Composite Key**
### **Definition:**  
A **primary key made of multiple columns**, used when **a single column cannot uniquely identify a record**.

📌 **Example in E-commerce (`fact_sales`)**
| sale_id | date_sk  | customer_sk | product_sk | quantity | sales_amount |
|--------|--------|------------|------------|---------|-------------|
| 10001  | 20240301 | 101        | 501        | 2       | 250.00      |
| 10001  | 20240301 | 101        | 502        | 1       | 100.00      |

- `sale_id` alone is **not unique** (same order can have multiple products).
- (`sale_id`, `product_sk`) **together form a Composite Key**.

✅ **Use Case:** Used in **fact tables** to track multiple measures per transaction.

---

## **6️⃣ Alternate Key**
### **Definition:**  
A column that is **a unique candidate for the primary key** but is **not chosen as the primary key**.

📌 **Example in E-commerce (`dim_customer`)**
| customer_sk | customer_id | email              | phone_number  |
|------------|------------|----------------|--------------|
| 101        | CUST_001   | alice@email.com | 9876543210   |
| 102        | CUST_002   | bob@email.com   | 8765432109   |

- `customer_id` and `email` are **both unique**, but `customer_sk` is the **Primary Key**.
- `email` could be used as an **Alternate Key**.

✅ **Use Case:** Helps enforce **unique constraints** on non-PK columns.

---

## **7️⃣ Degenerate Key (DK)**
### **Definition:**  
A key that **exists in a fact table** but **does not have a related dimension table**.

📌 **Example in E-commerce (`fact_sales`)**
| sale_id | date_sk  | customer_sk | product_sk | store_sk | order_number |
|--------|--------|------------|------------|---------|-------------|
| 10001  | 20240301 | 101        | 501        | 301     | ORD12345    |
| 10002  | 20240302 | 102        | 502        | 302     | ORD12346    |

- `order_number = ORD12345` is **a business identifier**, but no `dim_order` table exists.
- It’s stored **directly in the fact table** as a **Degenerate Key**.

✅ **Use Case:** Used when **no additional attributes** exist for the key.

---

## **8️⃣ Slowly Changing Key (SCD - Type 1, 2, 3)**
### **Definition:**  
Handles **historical changes** in dimension tables.

📌 **Example in E-commerce (`dim_customer`)**  
**📌 SCD Type 2: Stores historical versions with surrogate keys.**
| customer_sk | customer_id | customer_name | country | start_date | end_date |
|------------|------------|--------------|--------|------------|----------|
| 101        | CUST_001   | Alice Smith  | USA    | 2023-01-01 | 2024-03-01 |
| 102        | CUST_001   | Alice Johnson| USA    | 2024-03-02 | NULL     |

- Alice’s **last name changed**, so a **new row is created**.
- `end_date = NULL` indicates **current active record**.

✅ **Use Case:** Tracks **historical changes** in customer/product data.

---

### **🚀 Summary Table**
| **Key Type** | **Definition** | **Example (E-commerce)** |
|------------|-------------|---------------------|
| **Surrogate Key (SK)** | System-generated unique ID | `customer_sk = 101` |
| **Natural Key (NK)** | Business-defined unique ID | `customer_id = CUST_001` |
| **Primary Key (PK)** | Uniquely identifies records | `store_sk` in `dim_store` |
| **Foreign Key (FK)** | Connects fact to dimension tables | `customer_sk` in `fact_sales` |
| **Composite Key** | Multiple columns as a PK | `(sale_id, product_sk)` in `fact_sales` |
| **Alternate Key** | Unused unique key | `email` in `dim_customer` |
| **Degenerate Key (DK)** | Fact table key without a dimension | `order_number` in `fact_sales` |
| **Slowly Changing Key (SCD)** | Maintains historical changes | `customer_sk` in `dim_customer` |

---
