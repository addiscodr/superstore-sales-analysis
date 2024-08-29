# Coffee Shop Sales Analysis 
## Table of Contents
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Explanatory Data Analysis](#explanatory-data-analysis)
## Project Overview
The project aim at ....
## Data Source
The data souce is Kaggle.com and can be downloaded [here](https://www.kaggle.com/datasets/ahmedabbas757/coffee-sales)
## Tools 
- MS Excel 2019 - Row Data 
- MySQL - for Data Cleaning, Preparation and Analysis
- Power BI - for Data Analysis and Visualization 
## Data Cleaning and Preparation
In this phase I performed the following tasks:
1. Data loading and inspection
2. Handling missing values
3. Data formatting
## Explanatory Data Analysis
In this EDA step I tried to show the following requirements which a hypotherical coffee shop owner would be interested in to have insights:
## *KPI's Requirements*
### 1. Total Sales Analysis:
- Calculate the total sales for each month
- Determine the month-on-month increase or decrease in sales
- Calculate the difference in sales between the selected month and the previous month
### 2. Total Orders Analysis:
- Calculate the total number of orders for each month
- Determine the month-on-month increase in the number of orders
- Calculate the difference in the number of orders between the selected month and the previous month
### 3. Total Quantity Sold Analysis:
- Calculate the total quantity sold for each month
- Determine the month-on-month increase or decrease in the total quantity sold
- Calculate the difference in the total quantity sold between the selected month and the previous month
## *Charts Requirements*
### 1. Calendar Heatmap:
- Implement a calendar heat map that dynamically adjusts based on the selected month from a slicer
- Each day on the calendar should be color-coded to represent sales volume, with darker shades indicating higher sales
- Implement tool tips to display detailed metrics (Sales, Orders, Quantity) when hovering over a specific day
### 2. Sales Analysis by Weekdays and Weekends:
- Segment sales data into weekdays and weekends to analyze performance variations
- Provide insights into whether sales patterns differ significantly between weekdays and weekends
### 3. Sales Analysis by Store Location:
- Visualize sales data by different store locations
- Include month-over-month (MoM) difference metrics
- Highlight MoM sales increase or decrease for each store to identify trends
### 4. Daily Sales Analysis with Avergae Line:
- Display daily sales for the selected month a line chart
- Incorporate an average line on the chart to represent the average daily sales
- Highlight bars exceeding or falling below the average sales to identify exceptional sales days
### 5. Sales Analysis by Product Category:
- Analyze sales performance across different product categories
- Provide insight into which product categories contribute the most to overall sales
### 6. Top 10 Products Sold:
- Identify and display the top 10 products based on sales volume
- Allow users to quickly visualize the best-performing products in terms of sales
### 7. Sales Analysis by Days and Hours:
- Utilize a heat map to visualize sales by days and hours
- Implement tooltips to display detailed metrics (Sales, Orders, Quantity) when hovering over a specific day-hour
## PART I - SQL Data Analysis
- Create a database
```sql
CREATE DATABASE coffee_sales_db;
```
- Use the database
```sql
USE coffee_sales_db;
```
- Create a table
```sql
CREATE TABLE coffee_shop_sales;
```
- View the table for inspection
```sql
SELECT *
FROM coffee_shop_sales;
```
- Covert date (transaction_date) column to proper date format
```sql
UPDATE coffee_shop_sales
SET transaction_date = str_to_date(transaction_date, '%d/%m/%Y');
```
- Alter date (transaction_date) column to DATE data type
```sql
ALTER TABLE coffee_shop_sales
MODIFY COLUMN transaction_date DATE;
```
- Covert time (transaction_time)  column to proper DATE format
```sql
UPDATE coffee_shop_sales
SET transaction_time = STR_TO_DATE(transaction_time, '%H:%i:%s');
```
- Alter time (transaction_time) colum to DATE data type
```sql
ALTER TABLE coffee_shop_sales
MODIFY COLUMN transaction_time TIME;
```
- Data types of different columns
```sql
DESCRIBE coffee_shop_sales;
```
- Change column name `ï»¿transaction_id` to transaction_id
```sql
ALTER TABLE coffee_shop_sales
CHANGE COLUMN `ï»¿transaction_id` transaction_id INT;
```
- Total sales
```sql
SELECT ROUND(SUM(transaction_qty * unit_price)) as total_sales 
FROM coffee_shop_sales 
WHERE MONTH(transaction_date) = 5 -- For month of May
```
- Total sales KPI - MoM difference and MoM growth
```sql
SELECT 
    MONTH(transaction_date) AS month,
    ROUND(SUM(transaction_qty * unit_price)) AS total_sales,
    (SUM(transaction_qty * unit_price) - LAG(SUM(transaction_qty * unit_price), 1)
    OVER (ORDER BY MONTH(transaction_date))) / LAG(SUM(transaction_qty * unit_price), 1) 
    OVER (ORDER BY MONTH(transaction_date)) * 100 AS mom_increase_percentage
FROM 
    coffee_shop_sales
WHERE 
    MONTH(transaction_date) IN (4, 5) -- for months of April and May
GROUP BY 
    MONTH(transaction_date)
ORDER BY 
    MONTH(transaction_date);
```


 
 
 

 




