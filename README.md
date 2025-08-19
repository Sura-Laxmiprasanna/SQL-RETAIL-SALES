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

DELETE FROM retail_sales
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;

2. Data Exploration

Total number of transactions

Unique customers

Product categories

Key SQL Queries
Q1. Sales on a specific date
SELECT * FROM retail_sales 
WHERE sale_date = '2022-11-05';

Q2. Clothing transactions with quantity >4 in Nov 2022
SELECT * FROM retail_sales
WHERE category='clothing'
  AND TO_CHAR(sale_date,'YYYY-MM')='2022-11'
  AND quantiy>=4;

Q3. Total sales per category
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;

Q4. Average age of Beauty category customers
SELECT ROUND(AVG(age),2) AS avg_age
FROM retail_sales
WHERE category='Beauty';

Q5. High-value transactions (>1000)
SELECT * FROM retail_sales
WHERE total_sale>1000;

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
