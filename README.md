# Retail-Sales_SQL

**Project Overview**

**Project Title**: Retail Sales Analysis

**Level**: Beginner

 This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

**Objectives**

**Data Cleaning**: Identify and remove any records with missing or null values.

**Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.

**Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

**Project Structure**

**Table Creation:** A table named retail_sales is created to store the sales data. The table structure includes columns for s_no, transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount. After table creation import the dataset from local file.

```sql

CREATE TABLE retail_sales
(
    s_no INT,
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

insert into retail_sales1 SELECT * from retail_sales
SELECT * FROM retail_sales1
```

**Data Exploration & Cleaning**

**Record Count:** Determine the total number of records in the dataset.

**Customer Count:** Find out how many unique customers are in the dataset.

**Category Count:** Identify all unique product categories in the dataset.

**Null Value Check:** Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM retail_sales1;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales1;
SELECT DISTINCT category FROM retail_sales1;

SELECT * FROM retail_sales1
WHERE 
transactions_id  is NULL OR
    sale_date IS NULL OR 
    sale_time IS NULL OR 
    customer_id IS NULL OR 
    gender IS NULL OR 
    age IS NULL OR 
    category IS NULL OR 
    quantity IS NULL OR 
    price_per_unit IS NULL OR 
    cogs IS NULL;
    
    DELETE FROM retail_sales1
    WHERE
    s_no is NULL OR
    transactions_id  is NULL OR
    sale_date IS NULL OR 
    sale_time IS NULL OR 
    customer_id IS NULL OR 
    gender IS NULL OR 
    age IS NULL OR 
    category IS NULL OR 
    quantity IS NULL OR 
    price_per_unit IS NULL OR 
    cogs IS NULL;

SELECT count(*) FROM retail_sales1
```

**Data Analysis & Findings**

The following SQL queries were developed to answer specific business questions:

1.To retrieve all columns for sales made on '12-12-2022'

```sql 
SELECT * FROM retail_sales1 where sale_date = '12-12-2022';
```


2.To retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 3 in the month of Dec-2022:

```sql
 SELECT category,sale_date,quantity from retail_sales1 
WHERE
category='Clothing' OR
sale_date='11-2022' OR
quantity >3
```

3.To calculate the total sales (total_sale) for each category

```sql 
SELECT category,sum(total_sale)as net_sale FROM retail_sales1 GROUP BY category;
```

4.To find the average age of customers who purchased items from the 'Beauty' category

```sql
SELECT Round(AVG(age)) as Avg_age FROM retail_sales1 
WHERE 
category='Beauty';
```

5.To find all transactions where the total_sale is greater than 1000

 ```sql
SELECT * FROM retail_sales1
 WHERE 
 total_sale > 1000
```

 6.To find the total number of transactions (transaction_id) made by each gender in each category

```sql
 SELECT gender,category,count (*) as tot_transaction from retail_sales1 
GROUP by gender,category
 ```

 7.To find the top 5 customers based on the highest total sales 

```sql
 SELECT customer_id,sum(total_sale) as total_sales FROM retail_sales1 
GROUP by 1 ORDER by 2 DESC LIMIT 5;
```

8.To find the number of unique customers who purchased items from each category

```sql
 SELECT category,COUNT(DISTINCT customer_id)AS unique_cus FROM retail_sales1 
GROUP BY category
```

9.To calculate the average sale for each month. Find out best selling month in each year

```sql
SELECT 
    year,
    month,
    avg_sale
FROM 
( SELECT 
        EXTRACT(YEAR FROM TO_DATE(sale_date, 'DD-MM-YYYY')) AS year,
        EXTRACT(MONTH FROM TO_DATE(sale_date, 'DD-MM-YYYY')) AS month,
        Round(AVG(total_sale)) AS avg_sale,
        RANK() OVER (PARTITION BY EXTRACT(YEAR FROM TO_DATE(sale_date, 'DD-MM-YYYY')) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales1
    GROUP BY 1,2)AS t1
    WHERE rank = 1;
```

 10.To create each shift and number of orders Example Morning <12, Afternoon Between 12 & 17, Evening >17

```sql
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM TO_TIMESTAMP(sale_time, 'HH24:MI:SS')) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM TO_TIMESTAMP(sale_time, 'HH24:MI:SS')) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END as shift
    FROM retail_sales1
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift;
```







