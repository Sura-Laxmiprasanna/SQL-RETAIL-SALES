Retail Sales Data Analysis using SQL

This project demonstrates end-to-end data analysis using SQL on a retail sales dataset. It covers data cleaning, exploration, and business-focused analysis, providing actionable insights for decision-making.



Project Overview

The goal is to analyze sales transactions to:

Identify trends in revenue and customer behavior.

Understand which product categories perform best.

Discover high-value customers and seasonal patterns.

Support business decisions with data-driven recommendations.


Dataset Schema

The project uses a SQL table named retail_sales with the following columns:

Column Name	Data Type	Description
transactions_id	INT (PK)	Unique transaction ID
sale_date	DATE	Date of sale
sale_time	TIME	Time of sale
customer_id	INT	Customer identifier
gender	VARCHAR(15)	Gender of customer
age	INT	Age of customer
category	VARCHAR(15)	Product category
quantiy*	INT	Number of units sold (*typo in column name)
price_per_unit	FLOAT	Price per unit
cogs	FLOAT	Cost of goods sold
total_sale	FLOAT	Total sale amount

Steps Performed
1. Data Cleaning

Checked for missing or NULL values.

Removed invalid transactions:

---DELETE FROM retail_sales
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
   ---


2. Data Exploration
 ---<img width="997" height="273" alt="Screenshot 2025-08-19 235816" src="https://github.com/user-attachments/assets/92701a19-4c5f-42d3-8f17-dd066616c076" />

Total number of transactions

Unique customers

Product categories

<img width="372" height="536" alt="Screenshot 2025-08-20 000103" src="https://github.com/user-attachments/assets/8192ab24-4be1-46db-acd5-c49be6279418" />

Key SQL Queries
Q1. Sales on a specific date
---SELECT * FROM retail_sales 
WHERE sale_date = '2022-11-05';---
<img width="990" height="374" alt="Screenshot 2025-08-20 000443" src="https://github.com/user-attachments/assets/aa7ab851-f012-42f5-be49-8fded568c9f0" />


Q2. Clothing transactions with quantity >4 in Nov 2022
---SELECT * FROM retail_sales
WHERE category='clothing'
  AND TO_CHAR(sale_date,'YYYY-MM')='2022-11'
  AND quantiy>=4;---

Q3. Total sales per category
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;


<img width="799" height="427" alt="Screenshot 2025-08-20 000550" src="https://github.com/user-attachments/assets/4a89eb62-bdeb-4e7c-bb54-50ecab27d968" />

Q4. Average age of Beauty category customers
SELECT ROUND(AVG(age),2) AS avg_age
FROM retail_sales
WHERE category='Beauty';

Q5. High-value transactions (>1000)
SELECT * FROM retail_sales
WHERE total_sale>1000;

<img width="1001" height="323" alt="Screenshot 2025-08-20 000656" src="https://github.com/user-attachments/assets/112e3029-24c0-4784-bd46-dca30b80f918" />

Q6. Transactions by gender and category
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;

Q7. Best-selling month (per year)
SELECT year, month, avg_sale
FROM (
    SELECT year, month, avg_sale,
           RANK() OVER (PARTITION BY year ORDER BY avg_sale DESC) AS rnk
    FROM (
        SELECT EXTRACT(YEAR FROM sale_date) AS year,
               EXTRACT(MONTH FROM sale_date) AS month,
               AVG(total_sale) AS avg_sale
        FROM retail_sales
        GROUP BY 1,2
    ) sub
) ranked
WHERE rnk = 1;
<img width="441" height="474" alt="Screenshot 2025-08-20 000814" src="https://github.com/user-attachments/assets/e2cf9c6b-f5ec-47a0-8d55-26ae5df589a3" />

Q8. Top 5 customers by total sales
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC 
LIMIT 5;

Q9. Unique customers per category
SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;

<img width="988" height="422" alt="Screenshot 2025-08-20 000917" src="https://github.com/user-attachments/assets/58a00dd3-a50c-4bbd-a9f3-f0108a3bf26e" />

Q10. Orders by time shift
WITH hourly_sale AS (
    SELECT *,
           CASE
               WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'morning'
               WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'afternoon'
               ELSE 'evening'
           END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
<img width="751" height="549" alt="Screenshot 2025-08-20 001038" src="https://github.com/user-attachments/assets/e742af94-82ba-4c37-9aee-00de7f565297" />

Insights & Recommendations

Data Quality: Missing records were removed to ensure reliable results.

Customer Demographics: Specific categories (e.g., Beauty) attract a defined age group.

Seasonality: Best-selling months can be targeted for promotions and stock planning.

High-Value Customers: Top spenders can be engaged through loyalty programs.

Time-of-Day Trends: Morning vs. evening sales insights can optimize staffing and marketing campaigns.

How to Run the Project

Create a database in PostgreSQL/MySQL (or any RDBMS).

Use the CREATE TABLE script to set up the schema.

Insert your dataset (CSV/SQL dump).

Run each query in sequence to reproduce the analysis.

End of Project

This SQL project demonstrates a complete data workflow: cleaning, exploration, and deriving actionable business insights using structured queries.# SQL-RETAIL-SALES-QUERY
