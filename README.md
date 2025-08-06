
 Retail Store Sales Analysis – SQL Project
----
 Project Overview:

Retailers lose millions in revenue due to poor sales tracking, inefficient demand forecasting, and lack of actionable customer insights. This project showcases advanced SQL techniques to analyze, clean, and transform raw sales data into game-changing business intelligence.

From database creation to deep sales analytics, this project delivers crystal-clear insights into revenue trends, peak sales periods, high-value customers, and operational inefficiencies—helping businesses maximize profitability and sharpen their competitive edge.

----

 Key Objectives:

. Database & Table Creation – Structured storage of sales transactions, products, and customer data

. Data Cleaning & Exploration – Identify missing values, remove inconsistencies, and ensure high data quality

. Sales Trend Analysis – Identify best-selling months, track revenue trends, and uncover seasonal patterns

. Peak Sales Hours & Shifts – Understand when transactions peak (Morning, Afternoon, Evening) for better staffing & promotions

. Top Customers & High-Value Segments – Identify the top 5 revenue-generating customers to drive loyalty & retention

----

 Skills Used:

. SQL Querying & Optimization – Writing efficient queries to extract insights

. Data Cleaning & Transformation – Handling missing data, removing inconsistencies

. Data Aggregation & Grouping – Using GROUP BY, HAVING, and JOINs for deeper insights

. Database Design & Schema Creation – Structuring tables for efficient storage & retrieval

. Time Series Analysis – Identifying peak sales periods & forecasting demand

----


##  SQL Implementation Highlights
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
----
 Business Impact:
 
. Seasonal Sales Trends – Optimize promotions & inventory based on high-revenue months

. High-Value Customers – Retain top-spending customers with targeted marketing

. Time-Based Sales Insights – Adjust store hours & staffing for peak demand

. Category Performance – Improve product strategy to increase sales & reduce waste

----


 Conclusion:

This SQL project transforms raw sales data into actionable intelligence, helping businesses optimize revenue, improve operations, and sharpen decision-making.

 Data-Driven Decisions = Higher Revenue, Lower Costs, Smarter Growth!


