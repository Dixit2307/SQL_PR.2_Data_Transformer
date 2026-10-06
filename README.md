
<div align="center">

<!-- Hero Banner / Identity -->
<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1,11,21,31&height=220&section=header&text=DATA%20TRANSFORMER&fontSize=65&fontAlignY=38&animation=fadeIn&fontColor=ffffff" alt="Header" width="100%"/>
</p>

[![MySQL](https://img.shields.io/badge/MySQL-8.0+-00758F?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![Engine](https://img.shields.io/badge/Engine-InnoDB-FF6F00?style=for-the-badge&logo=databricks&logoColor=white)](#)
[![Queries](https://img.shields.io/badge/Queries-17%20Optimized-00E5FF?style=for-the-badge)](#)
[![Status](https://img.shields.io/badge/Production-Ready-00E676?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/License-MIT-blueviolet?style=for-the-badge)](#)

<p align="center">
  <strong>An enterprise-grade, relational data transformation framework demonstrating relational algebra, window ranking mechanics, and conditional data routing.</strong>
</p>

<p align="center">
  <a href="#-architecture">Architecture</a> •
  <a href="#-the-pipeline">Pipeline Breakdown</a> •
  <a href="#-deep-dive-queries">Key Transformations</a> •
  <a href="#-quickstart">Quickstart</a>
</p>

</div>

---

## ⚡ The Architectural Blueprint

```ascii
                      ┌────────────────────────────────────────┐
                      │            DATA TRANSFORMER            │
                      └────────────────────────────────────────┘
                                          │
            ┌─────────────────────────────┴─────────────────────────────┐
            ▼                                                           ▼
  ┌───────────────────┐                                       ┌───────────────────┐
  │   CUSTOMERS (c)   │                                       │   EMPLOYEES (e)   │
  ├───────────────────┤                                       ├───────────────────┤
  │ PK  Customer_id   │──┐                                    │ PK  Employee_id   │
  │     First_name    │  │                                    │     First_name    │
  │     Last_name     │  │                                    │     Last_name     │
  │     Email         │  │ 1:N                                │     Department    │
  │     Reg_date      │  │ Relational                         │     Hire_date     │
  └───────────────────┘  │ Mapping                            │     Salary        │
                         │                                    └───────────────────┘
                         ▼                                              │
              ┌───────────────────┐                                     │
              │    ORDER (o)      │                                     │
              ├───────────────────┤                                     ▼
              │ PK  Order_id      │                           ┌───────────────────┐
              │ FK  Customer_id   │                           │  SALARY BRACKETS  │
              │     Order_date    │                           │  Low | Med | High │
              │     Total_amount  │                           └───────────────────┘
              └───────────────────┘
                         │
                         ▼
              ┌───────────────────┐
              │ WINDOW ANALYTICS  │
              │ Running Totals    │
              │ Dense Ranks       │
              └───────────────────┘

```

---

## 📊 Pipeline Matrix

| Domain | Table | Primary Metric | Core Applied Logic

 |
| --- | --- | --- | --- |
| **Identity Management** | `Customers`<br> | Customer Lifecycle | String Sanitation, Whitespace Normalization

 |
| **Commerce Engine** | `Order`<br> | Cumulative Revenue | Rolling Totals via `SUM() OVER()`, Dynamic `RANK()`<br> |
| **People Analytics** | `Employees`<br> | Compensation Index | Subquery Average Thresholds, Custom Categorical Tiers

 |

---

## 🔥 Deep-Dive Query Showcase

Computes cumulative gross revenue across all transactions without self-joins, leveraging native SQL execution windows:

```sql
SELECT 
    Order_id, 
    Total_amount,
    SUM(Total_amount) OVER(ORDER BY Order_date, Order_id) AS Running_total,
    RANK() OVER(ORDER BY Total_amount DESC) AS OrderRank
FROM `Order`;

```

* **Use Case:** Real-time financial dashboards and pacing analysis.



Applies customer reward discounting rules dynamically based on invoice volume:

```sql
SELECT 
    Order_id,
    Customer_id,
    Order_date,
    Total_amount,
    CASE 
        WHEN Total_amount >= 900 THEN '15% Discount'
        WHEN Total_amount >= 500 THEN '10% Discount'
        WHEN Total_amount >= 100 THEN '5% Discount'
        ELSE 'No Discount'
    END AS Discount_Percentage
FROM `Order`;

```

Normalizes user inputs, eliminating whitespace artifacts and formatting clean identity markers:

```sql
SELECT 
    TRIM(First_name) AS CleanedFirst,
    TRIM(Last_name) AS CleanedLast,
    CONCAT(TRIM(First_name), ' ', TRIM(Last_name)) AS Full_name,
    UPPER(TRIM(First_name)) AS First_Upper,
    LOWER(TRIM(Last_name)) AS Last_Lower
FROM Employees;

```

Synthesizes unmatched and matched records across customer acquisition and order channels using unified relational operations:

```sql
SELECT c.Customer_id, c.First_name, c.Last_name, o.Order_id, o.Total_amount
FROM Customers c
LEFT JOIN `Order` o ON c.Customer_id = o.Customer_id
UNION
SELECT c.Customer_id, c.First_name, c.Last_name, o.Order_id, o.Total_amount
FROM Customers c
RIGHT JOIN `Order` o ON c.Customer_id = o.Customer_id;

```

---

## ⚡ Quickstart

```bash
# 1. Clone the project environment
git clone [https://github.com/your-username/data-transformer.git](https://github.com/your-username/data-transformer.git)
cd data-transformer

# 2. Bootstrapping DB & executing migrations
mysql -u root -p -e "CREATE DATABASE Data_transformer;"
mysql -u root -p Data_transformer < "Data Transformer.sqlbook"

```

---

## 🛠️ Stack & Optimization Standards

```json
{
  "engine": "MySQL 8.0+",
  "paradigm": "Declarative Relational Algebra",
  "features": [
    "ACID-Compliant Constraints",
    "Non-correlated Subqueries",
    "Analytic Window Buffers",
    "Deterministic String & Date Manipulation"
  ]
}

```

---

```
Engineered for clean schemas, optimized queries, and modern data analytics.

```
