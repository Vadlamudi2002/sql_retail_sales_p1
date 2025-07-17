 🛍️ SQL Retail Sales Analysis - P1

This project explores a retail sales dataset using SQL in PostgreSQL. It focuses on data cleaning, exploration, and solving key business questions through SQL queries.

---

## 📌 Project Details

- **Project Title**: SQL Retail Sales Analysis - P1  
- **Database**: PostgreSQL  
- **Tool Used**: pgAdmin 4  
- **Skills Used**: SQL (DDL, DML, Aggregations, CTEs, Window Functions)  
- **Dataset**: Simulated retail sales data

---

## 🧱 Table Structure

**Table Name**: `retail_sales`

| Column Name     | Data Type    | Description                    |
|-----------------|--------------|--------------------------------|
| transactions_id | INT (PK)     | Transaction ID (Primary Key)  |
| sale_date       | DATE         | Date of sale                  |
| sale_time       | TIME         | Time of sale                  |
| customer_id     | INT          | Customer ID                   |
| gender          | VARCHAR(15)  | Gender of customer            |
| age             | INT          | Age of customer               |
| category        | VARCHAR(15)  | Product category              |
| quantity        | INT          | Quantity sold                 |
| price_per_unit  | FLOAT        | Unit price of the product     |
| cogs            | FLOAT        | Cost of goods sold            |
| total_sale      | FLOAT        | Total sale value              |

---

## 🧹 Data Cleaning

To ensure data quality, null values are removed using:

```sql
DELETE FROM retail_sales
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR age IS NULL
   OR category IS NULL
   OR quantity IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
🔍 Data Exploration
Total Sales Records

sql
Copy
Edit
SELECT COUNT(*) FROM retail_sales;
Unique Customers

sql
Copy
Edit
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
Product Categories

sql
Copy
Edit
SELECT COUNT(DISTINCT category) FROM retail_sales;
💼 Business Questions & SQL Solutions
Q1: Sales on a Specific Date (e.g., '2022-11-05')
sql
Copy
Edit
SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';
Q2: Clothing Sales with Quantity > 10 in Nov 2022
sql
Copy
Edit
SELECT * FROM retail_sales
WHERE category = 'Clothing'
  AND quantity > 10
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11';
Q3: Total Sales by Category
sql
Copy
Edit
SELECT category, SUM(total_sale) FROM retail_sales GROUP BY category;
Q4: Average Age of Beauty Shoppers
sql
Copy
Edit
SELECT AVG(age) FROM retail_sales WHERE category = 'Beauty';
Q5: Transactions with Sales Over 1000
sql
Copy
Edit
SELECT * FROM retail_sales WHERE total_sale > 1000;
Q6: Number of Transactions by Gender and Category
sql
Copy
Edit
SELECT category, gender, COUNT(transactions_id)
FROM retail_sales
GROUP BY category, gender;
Q7: Best-Selling Month of Each Year
sql
Copy
Edit
SELECT year, month, avg_sale
FROM (
  SELECT EXTRACT(YEAR FROM sale_date) AS year,
         EXTRACT(MONTH FROM sale_date) AS month,
         AVG(total_sale) AS avg_sale,
         RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
  FROM retail_sales
  GROUP BY 1, 2
) AS ranked_sales
WHERE rank = 1;
Q8: Top 5 Customers by Total Sales
sql
Copy
Edit
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
Q9: Unique Customers per Category
sql
Copy
Edit
SELECT category, COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
Q10: Orders by Time of Day (Shift)
sql
Copy
Edit
WITH shift_data AS (
  SELECT *,
    CASE
      WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
      WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
      ELSE 'Evening'
    END AS shift
  FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM shift_data
GROUP BY shift;
🛠️ Tools & Technologies
PostgreSQL 17

pgAdmin 4

SQL (DDL, DML, Aggregates, Joins, Window Functions)