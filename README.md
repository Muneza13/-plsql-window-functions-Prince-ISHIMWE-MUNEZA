

# 🏗️ Construction Supply Company – Window Functions Project
**Author:** Prince ISHIMWE MUNEZA
**Course:** Database Development with PL/SQL (INSY 8311)   
**Institution**: Adventist University of Central Africa (AUCA)  
**Instructor**: Eric Maniraguha  
**Assignment**: Individual Assignment I - Window Functions Mastery Project  
**Submission Date**: September 29, 2025

---

## 📑 Table of Contents

1. [Project Overview](#-project-overview)
2. [Business Problem/challenge](#-business-problemchallenge)
3. [Success Criteria](#-success-criteria)
4. [Database Schema](#️-database-schema)
5. [Analytical Queries](#-analytical-queries)
6. [Results Analysis](#-results-analysis)
7. [References](#-references)
8. [Academic Integrity Statement](#-academic-integrity-statement)
9. [Contact Information](#-contact-information)

---

## 📌 Project Overview

This project demonstrates the use of **Oracle SQL analytic/window functions** in the context of a **MUNEZA supplies LTD (Construction equipment seller company)**.
The analysis provides insights into top-selling products, customer segmentation, monthly growth, and sales trends to support business decisions.

*MUNEZA Supplies Ltd is a construction supply company that sells cement, steel bars, bricks, roofing sheets, paint, and other materials across different regions in RWANDA. The management wants to identify top-selling materials per region and quarter, monitor monthly revenue growth, segment customers by spend, and analyze sales trends to guide inventory and marketing decisions.*

---

## 🏢 Business Problem/challenge

MUNEZA supplies LTD faces critical challenges in:

* Optimizing inventory allocation across regional warehouses
* Understanding seasonal demand patterns for different construction materials
* Managing relationships with diverse customer segments (large contractors to individual builders)
* Predicting demand fluctuations between dry and rainy construction seasons
* Developing data-driven procurement and pricing strategies

---

## 🎯 Success Criteria

| **#** | **Success Criteria**                                  | **SQL Function(s)**                                 | **Business Use**                                                             |
| ----- | ----------------------------------------------------- | --------------------------------------------------- | ---------------------------------------------------------------------------- |
| 1     | Identify **Top 5 products per region per quarter**    | `RANK()`, `ROW_NUMBER()`                            | Know which construction materials are top sellers by region                  |
| 2     | Track **running monthly sales totals per region**     | `SUM() OVER (PARTITION BY ... ORDER BY ...)`        | Monitor cumulative revenue growth across months for each region              |
| 3     | Calculate **month-over-month (MoM) revenue growth %** | `LAG()`                                             | Detect sales growth/decline trends, plan stock & promotions                  |
| 4     | Segment **customers into quartiles by total spend**   | `NTILE(4)`                                          | Classify contractors (high spenders vs. low spenders) for marketing strategy |
| 5     | Compute **3-month moving average of product sales**   | `AVG() OVER (ROWS BETWEEN 2 PRECEDING AND CURRENT)` | Smooth seasonal fluctuations & forecast demand                               |

---

## 🗂️ Database Schema

**Entity Relationship (ER) Diagram**

![model er diagram](https://github.com/user-attachments/assets/a9ea92be-3951-4bab-94e1-2ae32d5b0e16)

### Customers Table

```sql
CREATE TABLE customers (
  customer_id NUMBER PRIMARY KEY,
  name        VARCHAR2(100),
  region      VARCHAR2(50),
  signup_date DATE
);
```

![table customers 02](https://github.com/user-attachments/assets/f98c4ac2-5e7d-42e9-b383-6f9743dabb8f)

### Products Table

```sql
CREATE TABLE products (
  product_id NUMBER PRIMARY KEY,
  name       VARCHAR2(100),
  category   VARCHAR2(50),
  price      NUMBER(10,2)
);
```

![table products 03](https://github.com/user-attachments/assets/e11d58b6-e25f-48d6-b027-5972e0851772)

### Transactions Table

```sql
CREATE TABLE transactions (
  transaction_id NUMBER PRIMARY KEY,
  customer_id    NUMBER NOT NULL,
  product_id     NUMBER NOT NULL,
  sale_date      DATE NOT NULL,
  quantity       NUMBER(5),
  amount         NUMBER(12,2) NOT NULL,
  CONSTRAINT fk_customer FOREIGN KEY(customer_id) REFERENCES customers(customer_id),
  CONSTRAINT fk_product FOREIGN KEY(product_id) REFERENCES products(product_id)
);
```

![table transactions 04](https://github.com/user-attachments/assets/4ebbd173-0a35-4ab9-a0cd-b8627f592bb4)

---

## 📊 Analytical Queries

### **Ranking functions** (`ROW_NUMBER`, `RANK`, `DENSE_RANK`, `PERCENT_RANK`) → Top customers/products

```sql
WITH cust_spend AS (
  SELECT c.customer_id, c.name, SUM(t.amount) AS total_spend
  FROM customers c
  JOIN transactions t ON c.customer_id = t.customer_id
  GROUP BY c.customer_id, c.name
)
SELECT customer_id, name, total_spend,
       ROW_NUMBER() OVER (ORDER BY total_spend DESC) AS row_num,
       RANK()       OVER (ORDER BY total_spend DESC) AS rnk,
       DENSE_RANK() OVER (ORDER BY total_spend DESC) AS dense_rnk,
       PERCENT_RANK() OVER (ORDER BY total_spend DESC) AS pct_rank
FROM cust_spend
ORDER BY total_spend DESC;
```

![rank function](https://github.com/user-attachments/assets/7bb5431c-081f-40de-8098-a3423c79bfeb)

---

### **Aggregate windows** (`SUM OVER`) → Running totals per region

```sql
WITH monthly_sales AS (
  SELECT TRUNC(t.sale_date,'MONTH') AS month_start,
         c.region,
         SUM(t.amount) AS monthly_total
  FROM transactions t
  JOIN customers c ON t.customer_id = c.customer_id
  GROUP BY TRUNC(t.sale_date,'MONTH'), c.region
)
SELECT month_start,
       region,
       monthly_total,
       SUM(monthly_total) OVER (PARTITION BY region ORDER BY month_start
                                ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM monthly_sales
ORDER BY region, month_start;
```

![aggregates](https://github.com/user-attachments/assets/056ec139-7a5f-4789-9b82-edf21d62ef6b)

---

### **Navigation functions** (`LAG`, `LEAD`) → Month-over-month growth

```sql
WITH monthly_sales AS (
  SELECT TRUNC(t.sale_date,'MONTH') AS month_start,
         c.region,
         SUM(t.amount) AS monthly_total
  FROM transactions t
  JOIN customers c ON t.customer_id = c.customer_id
  GROUP BY TRUNC(t.sale_date,'MONTH'), c.region
)
SELECT month_start,
       region,
       monthly_total,
       LAG(monthly_total) OVER (PARTITION BY region ORDER BY month_start) AS prev_month_total,
       ROUND(
         (monthly_total - LAG(monthly_total) OVER (PARTITION BY region ORDER BY month_start))
         / NULLIF(LAG(monthly_total) OVER (PARTITION BY region ORDER BY month_start),0) * 100,
         2
       ) AS pct_change
FROM monthly_sales
ORDER BY region, month_start;
```

![navigation](https://github.com/user-attachments/assets/5f9aca38-b385-45cf-9f7c-c93b04ca1fbd)

---

### **Distribution functions** (`NTILE`, `CUME_DIST`) → Customer segmentation

```sql
SELECT customer_id, name, total_spend,
       NTILE(4) OVER (ORDER BY total_spend DESC) AS quartile,
       CUME_DIST() OVER (ORDER BY total_spend DESC) AS cume_dist
FROM (
  SELECT c.customer_id, c.name, SUM(t.amount) AS total_spend
  FROM customers c
  JOIN transactions t ON c.customer_id = t.customer_id
  GROUP BY c.customer_id, c.name
) cs
ORDER BY total_spend DESC;
```

![distribution](https://github.com/user-attachments/assets/4b841fd1-822c-4da3-bac3-1ecd0dc1a6be)

---

### **Moving average** (`AVG OVER`) → 3-month product sales trends

```sql
WITH monthly_prod_sales AS (
  SELECT TRUNC(sale_date,'MONTH') AS month_start,
         p.product_id,
         p.name AS product_name,
         SUM(amount) AS monthly_total
  FROM transactions t
  JOIN products p ON t.product_id = p.product_id
  GROUP BY TRUNC(sale_date,'MONTH'), p.product_id, p.name
)
SELECT month_start,
       product_id,
       product_name,
       monthly_total,
       AVG(monthly_total) OVER (PARTITION BY product_id ORDER BY month_start
                                ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS ma_3month
FROM monthly_prod_sales
ORDER BY product_id, month_start;
```

![average function](https://github.com/user-attachments/assets/73ab580f-b856-4df4-85bc-f831f9e053ad)

---

## 📈 Results Analysis

* **Descriptive (What happened?)**
  Cement and steel bars consistently ranked as top sellers; Kigali and East regions contributed the highest revenue.

* **Diagnostic (Why?)**
  Large orders from top contractors in Kigali drove spikes in monthly totals. Seasonal demand in roofing and paint created fluctuations.

* **Prescriptive (What next?)**
  Maintain higher inventory of cement and steel; design loyalty promotions for quartile-2 customers to push them toward quartile-1.

---

## 📚 References

1. [Oracle Database SQL Language Reference – Analytic Functions](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/Analytic-Functions.html)
2. [Oracle Documentation on LAG and LEAD](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/LAG.html)
3. [Oracle Documentation on NTILE](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/NTILE.html)
4. [Oracle SQL Developer User Guide](https://docs.oracle.com/en/database/oracle/sql-developer/)
5. [Stack Overflow – SQL Window Functions](https://stackoverflow.com/questions/tagged/sql-window-functions)
6. [Mode Analytics Blog – SQL Window Functions](https://mode.com/sql-tutorial/sql-window-functions/)
7. [TutorialsPoint – SQL Window Functions](https://www.tutorialspoint.com/sql/sql-window-functions.htm)
8. [DataCamp – SQL Window Functions Tutorial](https://www.datacamp.com/tutorial/tutorial-sql-window-functions)
9. [W3Schools – SQL Window Functions](https://www.w3schools.com/sql/sql_window.asp)
10. Course Lecture Notes & “SQL for Data Analytics” textbook

---

## 🛡️ Academic Integrity Statement

* I did not copy content directly from AI tools.
* All external sources I used are listed in the References section.
* The SQL code was written by me to practice and show my own skills.
* The business analysis is based on my own understanding of the course ideas.
* Any teamwork was only for discussing concepts, as allowed by course rules.
*   “All sources were properly cited. Implementations and analysis represent original work. No AI
     generated content was copied without attribution or adaptation.”
      Contact Information

---

## 📬 Contact Information
**Student:** Prince ISHIMWE MUNEZA
**Email:** {ishimwemunezaprince@gmail.com} 
**PHONE NUMBER** +250798552735
**Course:** Database Development with PL/SQL (INSY 8311)  
**Instructor:** Eric Maniraguha (eric.maniraguha@auca.ac.rw)  
**Institution:** Adventist University of Central Africa (AUCA)  
**Academic Year:** 2025-2026

---
"Whoever is faithful in very little is also faithful in much." – Luke 16:10
Muneza prince ISHIMWE
