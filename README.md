# 🏗️ Construction Supply Company – Window Functions Project

## 📑 Table of Contents

1. [Project Overview](#project-overview)
2. [Business Problem](#business-problem)
3. [Success Criteria](#success-criteria)
4. [Database Schema](#database-schema)
5. [SQL Scripts](#sql-scripts)
6. [Analytical Queries](#analytical-queries)
7. [Results Analysis](#results-analysis)
8. [Screenshots](#screenshots)
9. [How to Run](#how-to-run)
10. [References](#references)
11. [Academic Integrity Statement](#academic-integrity-statement)

---

## 📌 Project Overview

This project demonstrates the use of **Oracle SQL analytic/window functions** in the context of a **construction supply company**. The analysis provides insights into top-selling products, customer segmentation, monthly growth, and sales trends to support business decisions.

---

## 🏢 Business Problem

*BuildRwanda Supplies Ltd is a construction supply company that sells cement, steel bars, bricks, roofing sheets, paint, and other materials across different regions. The management wants to identify top-selling materials per region and quarter, monitor monthly revenue growth, segment customers by spend, and analyze sales trends to guide inventory and marketing decisions.*

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

```
customers (customer_id PK) 1 --- * transactions (transaction_id PK) * --- 1 products (product_id PK)
```

* **Customers**: Construction clients (contractors, companies)
* **Products**: Construction materials (cement, steel, bricks, roofing, etc.)
* **Transactions**: Sales records linking customers and products

---

## 📜 SQL Scripts

All scripts are inside `/sql/` folder:

* `01_create_tables.sql` → Defines tables (`customers`, `products`, `transactions`)
* `02_insert_sample_data.sql` → Inserts sample data (≥50 transactions)
* `03_window_queries.sql` → Contains analytic queries (ranking, running totals, MoM, quartiles, moving averages)

---

## 📊 Analytical Queries

* **Ranking functions** (`ROW_NUMBER`, `RANK`, `DENSE_RANK`, `PERCENT_RANK`) → Top customers/products
* **Aggregate windows** (`SUM OVER`) → Running totals per region
* **Navigation functions** (`LAG`, `LEAD`) → Month-over-month growth
* **Distribution functions** (`NTILE`, `CUME_DIST`) → Customer segmentation
* **Moving average** (`AVG OVER`) → 3-month product sales trends

---

## 📈 Results Analysis

* **Descriptive (What happened?)**
  Cement and steel bars consistently ranked as top sellers; Kigali and East regions contributed the highest revenue.

* **Diagnostic (Why?)**
  Large orders from top contractors in Kigali drove spikes in monthly totals. Seasonal demand in roofing and paint created fluctuations.

* **Prescriptive (What next?)**
  Maintain higher inventory of cement and steel; design loyalty promotions for quartile-2 customers to push them toward quartile-1.

---

## 🖼️ Screenshots

All query execution results and ER diagram screenshots are available in the `/screenshots/` folder. (≥20 as required).

---

## ⚙️ How to Run

1. Open Oracle SQL Developer.
2. Run `01_create_tables.sql` to create tables.
3. Run `02_insert_sample_data.sql` to load data.
4. Run queries from `03_window_queries.sql`.
5. Verify outputs and take screenshots.

---

## 📚 References

1. Oracle Database SQL Language Reference — Analytic Functions
2. Oracle Documentation on `LAG`, `LEAD`, `NTILE`
3. Oracle SQL Developer User Guide
4. Stack Overflow discussions on window functions
5. Mode Analytics Blog — SQL Window Functions
6. TutorialsPoint SQL Window Functions Guide
7. DataCamp — SQL for Data Analysis
8. Course Lecture Notes
9. “SQL for Data Analytics” textbook
10. W3Schools SQL Documentation

---

## 🛡️ Academic Integrity Statement

> I declare that this project was developed by me for academic purposes. All sources consulted are cited. No unauthorized assistance was used.

