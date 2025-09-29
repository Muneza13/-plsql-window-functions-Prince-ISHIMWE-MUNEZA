
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
7. [Screenshots](#-screenshots)
8. [How to Run](#-how-to-run)
9. [References](#-references)
10. [Academic Integrity Statement](#-academic-integrity-statement)
11. [Contact Information](#-contact-information)

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

*(kept exactly as you wrote — sample SQL queries + screenshots)*

---

## 📈 Results Analysis

* **Descriptive (What happened?)**
* **Diagnostic (Why?)**
* **Prescriptive (What next?)**

---

## 🖼️ Screenshots

All query execution results and ER diagram screenshots are included (≥20 screenshots).

---

## ⚙️ How to Run

1. Open Oracle SQL Developer.
2. Run `01_create_tables.sql`.
3. Run `02_insert_sample_data.sql`.
4. Run `03_window_queries.sql`.
5. Take screenshots and review outputs.

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

---

## 📬 Contact Information

**Student:** Prince ISHIMWE MUNEZA
**Email:** [ishimwemunezaprince@gmail.com](mailto:ishimwemunezaprince@gmail.com)
**Phone Number:** +250 798 552 735
**Course:** Database Development with PL/SQL (INSY 8311)
**Instructor:** Eric Maniraguha ([eric.maniraguha@auca.ac.rw](mailto:eric.maniraguha@auca.ac.rw))
**Institution:** Adventist University of Central Africa (AUCA)
**Academic Year:** 2025–2026

