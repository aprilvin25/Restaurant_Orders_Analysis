# Analyzing Restaurant Orders Using SQL: A Step-by-Step Guide 

Article: https://medium.com/@aprilvin25/analyzing-restaurant-orders-using-sql-structured-query-language-a-step-by-step-guide-bb8b1a21bdd3


## Aim of the Project
In the food industry, an understanding of customer preferences is essential for having a successful business. This project dives into questions related to customer behavior and sales, exploring questions such as ‘What is the most popular cuisine?,’ ‘What is the average daily cost of sales?,’ and ‘Which days of the week have the highest record of sales?’


## About the Data
This data set comes from Maven Analytics Data Playground. This dataset contains data for a quarter, specifically January, February and March. The order_details table comprises 12,097 rows, while the menu_items table consists of 32 rows.

Source of data: https://mavenanalytics.io/data-playground 

`Menu_items` Table:   

| Column                  | Description                             | Data Type      |
| :---------------------- | :-------------------------------------- | :------------- |
| menu_item_id            | Unique ID of a menu item                | BIGINT         |
| item_name               | Name of a menu item                     | VARCHAR(50)    |
| category                | Category / type of cuisine of the menu item| VARCHAR(30) |
| price                   | Price of menu item in USD               | VARCHAR(30)    |


`Order_details` Table:   

| Column                  | Description                             | Data Type      |
| :---------------------- | :-------------------------------------- | :------------- |
| order_details_id        | Unique ID of an item in an order        | BIGINT         |
| order_id                | ID of an order                          | BIGINT     |
| order_date              | Date of an order was put in             | DATETIME    |
| order_time              | Time order was put in                   | DATEIME    |
| item_id                 | Foreign key to menu_item_id in Menu_items table | BIGINT  |

## Method

1. **Data Wrangling:**  

> 1. Create a database
> 2. Create tables with its data type, set **NOT NULL** for each column
> 3. Import data with Data Import Wizard

2. **Feature Engineering:** Create new columns from existing ones. 

> 1. Extract a new column called `time_of_day` from date 
> 2. Extract a new column called `day_name` from date
> 3. Extract a new column called `month_name` from date

3. **Exploratory Data Analysis (EDA):** Analyze and answer the business questions listed below.
> SQL Techniques employed:
  - Creating tables --> organized data and defined data types and lengths for each column
  - Left join --> combined two tables together to gather relevant columns matching all records from the left table
  - CASE statements --> used conditional logic to categorize values 
  - GROUP BY --> grouped data to perform aggragate functions and discover trends within distinct categories
  - ORDER BY --> sorted results to analyze key patterns
  - Feature Engineering --> created additional columns to capture a better analysis
  - IN statement --> efficient way of filtering in the WHERE statment based on multiple specified values
  - COUNT function --> to quantify
  - Aliases --> enhance code readability
  - SUM & AVERAGE functions --> calculated total and average values for overall trends
  - ROUND function --> to ensure precision and for better readability 



## Business Questions To Answer

### Generic Questions / Descriptive Statistics

1. How many months are in the dataset?
2. What is the average price for each category (type of cuisine)?
3. What is the average amount of customers who come in for each day of the week (for the 3 months of data)?

### Customer Questions

1. How many order items (order_details_id) did each customer order (order_id)? 
What was the least amount of items ordered and the greatest amount?
2. What time of day is the busiest, on average?
3. What is the total amount of customers that have come in each day of the week?
   
### Menu Item Questions

1. How many unique food menu items does this restaurant have?
2. What categories (cuisine) does this restaurant have to offer?

### Sales Questions

1. What were the least and most ordered items? What categories were they in?
2. What do the highest spend orders look like? Which items did they buy and how much did they spend?
3. What is the gross sales for each category (cuisine) in total? How about for each month? Each Day?
4. Which cuisines should we focus on developming more menu items for
based on the data?
5. What were the number of sales made in each time of the day
per weekday?
6. Get each category (cuisine) and add a column showing "Good," "Bad," and "Average".
Mark "Good" if it's greater than the average sales and "Bad" if it's lower than the average sales.
