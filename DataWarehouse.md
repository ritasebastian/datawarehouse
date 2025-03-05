

### **1. What is a Data Warehouse?**
A **Data Warehouse** is a **centralized repository** where structured data from different sources is stored, transformed, and analyzed for decision-making. It is optimized for **analytical queries** rather than transactional processing.

---

### **2. How is a Data Warehouse different from a Database?**
| Feature           | Database (OLTP)        | Data Warehouse (OLAP) |
|------------------|----------------------|----------------------|
| Purpose         | Transaction Processing | Analytical Processing |
| Data Structure  | Normalized (3NF) | Denormalized (Star/Snowflake) |
| Performance     | Fast for CRUD operations | Optimized for complex queries |
| Data Type       | Current operational data | Historical & aggregated data |
| Example        | MySQL, PostgreSQL | Amazon Redshift, Snowflake |

---

### **3. What are the different components of a Data Warehouse?**
1. **ETL (Extract, Transform, Load)** – Extracts data from sources, transforms it, and loads it into the warehouse.
2. **Data Storage** – Stores historical data in relational/dimensional models.
3. **OLAP Engine** – Performs analytical queries efficiently.
4. **BI (Business Intelligence) Tools** – Provides insights and reports.

---

### **4. What is ETL in Data Warehousing?**
**ETL (Extract, Transform, Load)** is a process that:
- **Extracts** data from multiple sources.
- **Transforms** the data (cleaning, aggregating, formatting).
- **Loads** the processed data into a Data Warehouse.

---

### **5. What is the difference between OLTP and OLAP?**
| Feature        | OLTP (Online Transaction Processing) | OLAP (Online Analytical Processing) |
|--------------|----------------------------------|----------------------------------|
| Usage       | Operational system (e.g., banking) | Analytical system (e.g., reporting) |
| Data Type   | Current, detailed | Historical, summarized |
| Queries     | Simple, fast queries | Complex, multi-dimensional queries |
| Schema      | Normalized (3NF) | Denormalized (Star/Snowflake) |

---

### **6. What are Fact and Dimension Tables?**
- **Fact Table**: Stores **measurable business metrics** (e.g., sales, revenue, quantity).
- **Dimension Table**: Stores **descriptive attributes** related to facts (e.g., product name, customer details).

Example:
- **Fact Table**: Sales (Date, Product_ID, Revenue)
- **Dimension Table**: Product (Product_ID, Product_Name, Category)

---

### **7. What is a Star Schema and a Snowflake Schema?**
- **Star Schema**: 
  - A central **fact table** linked to multiple **dimension tables**.
  - Simple, fast for queries.
  - Example:
    ```
    Fact_Sales (Product_ID, Date, Revenue)
    Dimension_Product (Product_ID, Name, Category)
    Dimension_Date (Date, Year, Month)
    ```

- **Snowflake Schema**:
  - Normalized version of **Star Schema** (dimension tables are further divided).
  - More storage-efficient but slower queries.
  - Example:
    ```
    Dimension_Product -> Sub_Dimension_Category
    ```

---

### **8. What is Slowly Changing Dimension (SCD)?**
SCD is how **historical changes** in dimension data are handled:
- **Type 1 (Overwrite old data)** – No history maintained.
- **Type 2 (New row for each change)** – Keeps history using `start_date` and `end_date`.
- **Type 3 (New column for change)** – Limited history tracking.

Example: 
- Type 1: Customer address updated without history.
- Type 2: A new row is added for every change.
- Type 3: A new column `Previous_Address` is added.

---

### **9. What is Data Mart?**
A **Data Mart** is a **subset** of a Data Warehouse designed for a specific department (e.g., Sales, Finance). It allows **faster access** to relevant data.

---

### **10. What are Aggregate Tables in Data Warehousing?**
- Aggregate Tables store **precomputed summaries** of data (e.g., Monthly Sales instead of daily).
- They **improve query performance** by reducing processing time.

---

### **11. What is a Surrogate Key?**
A **Surrogate Key** is an **artificially generated unique identifier** (e.g., Auto-Increment ID) used in dimension tables instead of natural keys.

Example:
```
Customer_ID (Surrogate Key) | Customer_Email (Natural Key)
```

---

### **12. What is Partitioning in Data Warehousing?**
Partitioning improves **query performance** by dividing large tables into smaller, manageable pieces.

Types:
1. **Range Partitioning** – Based on date ranges (Jan-March, April-June).
2. **List Partitioning** – Based on specific values (Region: US, Europe).
3. **Hash Partitioning** – Evenly distributes data using a hashing function.

---

### **13. What is Indexing in Data Warehousing?**
Indexes improve **query speed** by allowing **faster lookups**.

Types:
1. **Bitmap Index** – Used for low-cardinality columns (e.g., Gender).
2. **B-Tree Index** – Used for high-cardinality columns (e.g., Customer_ID).

---

### **14. What are Materialized Views?**
- A **Materialized View** stores a **precomputed result** of a query.
- It improves query performance but needs **refreshing** to stay updated.

Example:
```
CREATE MATERIALIZED VIEW Monthly_Sales AS
SELECT Month, SUM(Revenue) FROM Sales GROUP BY Month;
```

---

### **15. What is a Data Lake vs. Data Warehouse?**
| Feature       | Data Warehouse | Data Lake |
|--------------|---------------|----------|
| Data Type    | Structured | Structured, Semi-structured, Unstructured |
| Storage Cost | High | Low |
| Processing   | SQL-based | Big Data Processing (Spark, Hadoop) |
| Use Case     | BI Reports, Dashboards | Data Science, AI |

---

### **16. What is ELT vs. ETL?**
| Feature  | ETL (Extract, Transform, Load) | ELT (Extract, Load, Transform) |
|---------|--------------------------------|--------------------------------|
| Transformation | Before loading | After loading |
| Performance | Slower for big data | Faster for big data |
| Tools | Informatica, Talend | Snowflake, BigQuery |

---

### **17. What is Real-Time Data Warehousing?**
- In **Real-Time Data Warehousing**, data is **continuously updated** instead of batch processing.
- Example: Stock market analytics.

Tools: **Kafka, Snowflake, Amazon Redshift Streaming**

---

### **18. What are some popular Data Warehousing tools?**
- **ETL Tools**: Informatica, Talend, Apache NiFi
- **Data Warehouses**: Snowflake, Amazon Redshift, Google BigQuery
- **BI Tools**: Tableau, Power BI, Looker

---

### **19. What is a Cube in OLAP?**
An **OLAP Cube** allows **multi-dimensional data analysis**.

Example:
- **Sales Analysis** Cube:
  - Dimensions: **Time, Region, Product**
  - Measure: **Revenue**

---

### **20. What are some performance optimization techniques for a Data Warehouse?**
1. **Partitioning** large tables.
2. **Indexing** frequently used columns.
3. Using **Materialized Views**.
4. **Precomputing Aggregations** (Aggregate Tables).
5. **Data Archiving** to remove old data.
6. **ETL Optimization** for faster data processing.

---

### **Final Tips for Data Warehouse Interviews:**
- Understand **Star/Snowflake Schema, ETL, OLAP, and SQL Queries**.
- Be comfortable with **SQL-based performance tuning**.
- Know **modern tools like Snowflake, BigQuery, AWS Redshift**.

---

