
# Retail Store Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Store Sales Analysis  

In order to help organizations make data-driven decisions, this project seeks to demonstrate SQL techniques and abilities that are frequently used to review, clean, and analyze retail sales data. The project includes database and table creation, data analysis and cleansing, and a series of SQL queries that will generate reports and critical metrics such as total sales, transaction breakdowns, and customer demographics.


## Objectives

1.**Database and Table Creation**: Create a database and relevant tables to store sales transactions, product categories, customer information, and other necessary data.

2.**Data Exploration and Cleaning**: Explore the dataset to check for inconsistencies or missing data and clean it to ensure accurate and reliable analyses.

3.**Monthly Sales Trends**: Calculate the average sale for each month and identify the best-selling month in each year, helping to understand seasonal sales patterns.

4.**Shift-based Sales Analysis**: Categorize sales by different time shifts (Morning, Afternoon, Evening) and calculate the number of orders placed in each shift.

5.**Top Customers**: Identify the top 5 customers with the highest total sales, which can help retailers focus on their most valuable customers.


## Project Overflow
### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `retaildb`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE 'retaildb';

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
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

### 3. Data Analysis 


1. **Retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

3. **Calculate the total sales (total_sale) for each category.**:
```sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1
```

4. **Find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

5. **Find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
```

6. **Find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1
```

7. **Calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
```

8. **Find the top 5 customers based on the highest total sales **:
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

9. **Find the number of unique customers who purchased items from each category.**:
```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category
```

10. **Create shifts and calculate respective number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
```

## Reports

-**Seasonality**: Monthly sales trends reveal the best-selling months for different years, helping retailers to plan inventory, sales strategies, and promotions for peak seasons.

-**Top Customers**: Identifying top customers based on total sales allows businesses to foster customer loyalty and incentivize repeat purchases from their most valuable customers.

-**Sales Shifts**: Categorizing sales into time shifts (morning, afternoon, evening) provides a clear understanding of when most transactions occur, guiding staffing and operational decisions to optimize sales during peak hours.

-**Category Performance**: Certain categories, such as 'Clothing' and 'Beauty,' show varying levels of customer interest and sales volumes, allowing businesses to focus on high-performing areas.

## Conclusion

The Retail Sales SQL project provides significant insights into the dynamics of retail sales by analyzing customer behavior, product performance, and seasonal sales trends across multiple categories thereby helping businesses to make informed decisions.


