
## 📊 What is a **Fact Table**?

A **Fact Table**:
- Stores **measurable, numeric data** (facts)
- Represents **events or transactions**
- Usually contains **foreign keys** referencing dimension tables

### 🔸 Examples of facts:
- Sales amount
- Order quantity
- Revenue
- Profit
- Clicks, logins, conversions

### 🧪 Example:
| Order_ID | Date_ID | Product_ID | Customer_ID | Quantity | Total_Amount |
|----------|---------|------------|-------------|----------|--------------|
| 1001     | 20240301| 2001       | 3001        | 2        | 1000         |

> 🔑 `Date_ID`, `Product_ID`, `Customer_ID` → foreign keys to dimension tables

---

## 🧾 What is a **Dimension Table**?

A **Dimension Table**:
- Stores **descriptive, textual or categorical data**
- Provides **context to facts**
- Used to **filter, group, or label** facts in reports

### 🔸 Examples of dimensions:
- Product details (name, category)
- Customer info (name, region, gender)
- Time/date (year, month, quarter)
- Store location

### 🧪 Example:
| Product_ID | Product_Name | Category | Brand  |
|------------|--------------|----------|--------|
| 2001       | iPhone 15    | Phones   | Apple  |

---

## 🔄 Key Differences

| Feature              | Fact Table                            | Dimension Table                         |
|----------------------|----------------------------------------|------------------------------------------|
| 📌 Stores            | Measures or metrics                   | Attributes or descriptive data           |
| 🎯 Purpose           | Analyzing, aggregating                | Filtering, grouping, labeling            |
| 🧮 Type of Data      | Numeric (quantitative)                | Textual (qualitative)                    |
| 🔑 Keys             | Foreign keys to dimensions            | Primary keys (referred by facts)         |
| 📦 Size             | Very large (many rows)                | Smaller in size                          |
| 🧠 Examples          | Sales, Revenue, Count                 | Product, Customer, Time, Location        |

---

## 📘 Real-World Example:

Let’s say you're analyzing online sales.

- **Fact Table** = `sales_facts`
  - Contains `total_sales`, `order_quantity`, `customer_id`, `product_id`, `date_id`
- **Dimension Tables**:
  - `dim_customer` → name, region
  - `dim_product` → name, category, brand
  - `dim_date` → year, month, day

---
