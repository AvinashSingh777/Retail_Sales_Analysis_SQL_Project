# Retail_Sales_Analysis_SQL_Project

## Project Overview

This project is  to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named retail.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

CREATE DATABASE retail;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

--sql--
SELECT COUNT(*) FROM retail_sales;<br/>
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;<br/>
SELECT DISTINCT category FROM retail_sales;<br/>

SELECT * FROM retail_sales<br/>
WHERE <br/>
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR <br/>
    gender IS NULL OR age IS NULL OR category IS NULL OR <br/>
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;<br/>

DELETE FROM retail_sales<br/>
WHERE <br/>
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR <br/>
    gender IS NULL OR age IS NULL OR category IS NULL OR <br/>
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;<br/>

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:<br/>

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**: 

SELECT *<br/>
FROM retail_sales<br/>
WHERE sale_date = '2022-11-05';<br/>


2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:

SELECT *<br/>
FROM retail_sales<br/>
WHERE <br/>
    category = 'Clothing'<br/>
    AND <br/>
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'<br/>
    AND<br/>
    quantity >= 4;<br/>


3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:

SELECT <br/>
    category,<br/>
    SUM(total_sale) as net_sale,<br/>
    COUNT(*) as total_orders<br/>
FROM retail_sales<br/>
GROUP BY 1;<br/>


4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
   
SELECT<br/>
    ROUND(AVG(age), 2) as avg_age<br/>
FROM retail_sales<br/>
WHERE category = 'Beauty';<br/>


5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:

SELECT * FROM retail_sales<br/>
WHERE total_sale > 1000;<br/>


6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:

SELECT  category,gender,COUNT(*) as total_trans<br/>
FROM retail_sales<br/>
GROUP  BY category,gender ORDER BY 1;<br/>

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:

SELECT year,month,avg_sale FROM <br/>
( <br/>   
SELECT <br/>
    EXTRACT(YEAR FROM sale_date) as year,<br/>
    EXTRACT(MONTH FROM sale_date) as month,<br/>
    AVG(total_sale) as avg_sale,<br/>
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank<br/>
FROM retail_sales<br/>
GROUP BY 1, 2<br/>
) as t1<br/>
WHERE rank = 1;<br/>


8. **Write a SQL query to find the top 5 customers based on the highest total sales **:

SELECT customer_id, SUM(total_sale) as total_sales<br/>
FROM retail_sales<br/>
GROUP BY 1<br/>
ORDER BY 2 DESC<br/>
LIMIT 5;<br/>


9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:

SELECT category,COUNT(DISTINCT customer_id) as cnt_unique_cs<br/>
FROM retail_sales<br/>
GROUP BY category;<br/>


10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:

WITH hourly_sale <br/>
AS<br/>
(<br/>
SELECT *,<br/>
    CASE <br/>
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'<br/>
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'<br/>
        ELSE 'Evening'<br/>
    END as shift<br/>
FROM retail_sales<br/>
)<br/>
SELECT <br/>
    shift,<br/>
    COUNT(*) as total_orders  <br/>
FROM hourly_sale <br/>
GROUP BY shift; <br/>


## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.
