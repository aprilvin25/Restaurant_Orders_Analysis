# Analyzing Restaurant Orders Using SQL: A Step-by-Step Guide


## Aim of the Project
In the food industry, an understanding of customer preferences is essential for having a successful business. This project dives into questions related to customer behavior and sales, exploring questions such as ‘What is the most popular cuisine?,’ ‘What is the average daily cost of sales?,’ and ‘Which day of the week have the highest record of sales?’

## About the Data
This data set comes from Maven Analytics Data Playground. This dataset contains data for a quarter, specifically January, February and March. The order_details table comprises 12,097 rows, while the menu_items table consists of 32 rows. 

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

2. **Exploratory Data Analysis (EDA):** Analyze and answer the business questions listed below. 


## Business Questions To Answer

### Generic Questions / Descriptive Statistics

1. How many months are in the dataset?
2. What is the average price for each category (type of cuisine)?

### Product Questions

1. ----
2. ----

### Sales Questions

1. ----
2. 2. ---

### Customer Questions

1. ----
2. ----



-- Finding the average amount of customers who come in for each day of the week, for the three months of data

/* Step 1) Find the total number of customers per month and day of the week; 
this will be the inner query of a subquery */

SELECT COUNT(DISTINCT order_id) AS total_customers, month, day_name
FROM order_details
GROUP BY month, day_name ORDER BY total_customers DESC;

/* Step 2) The average was rounded to the nearest whole integer, as you can't have a fraction of a person.
Here, about 294 customers come in on average every Monday and about 236 customers come in on Saturday, on average*/

SELECT ROUND(AVG(total_customers),0) AS average_total_customer, day_name
FROM (
	SELECT COUNT(DISTINCT order_id) AS total_customers, month, day_name
	FROM order_details
	GROUP BY month, day_name ORDER BY total_customers DESC
    ) AS total_customers_count -- This is the inner query
GROUP BY day_name;


-- ---------------------------------------------------------------------------------------
-- --------------------------- Customer QUESTIONS -----------------------------------------

/* How many order items (order_details_id) did each customer order (order_id)? 
What was the least amount of items ordered and the greatest amount? */
SELECT order_id AS customer, 
	COUNT(order_details_id) AS number_of_items_ordered
FROM order_details
GROUP BY order_id 
ORDER BY number_of_items_ordered DESC;

-- What time of day is the busiest, on average?
/* S1) making the inner query to find the total number of customers 
per time_of_day in each day of the week and month */
SELECT COUNT(DISTINCT order_id) AS number_of_customers, time_of_day, month, day_name
FROM order_details
GROUP BY time_of_day, month, day_name 
ORDER BY number_of_customers DESC;

/* S2) creating a subquery to find the average number of customers that order during each time
of the day */
SELECT ROUND(AVG(number_of_customers),0) AS avg_number_of_customers, time_of_day 
FROM (
	SELECT COUNT(DISTINCT order_id) AS number_of_customers, time_of_day, month, day_name
	FROM order_details
	GROUP BY time_of_day, month, day_name 
	ORDER BY number_of_customers DESC
    ) AS total_customers_per_time_of_day
GROUP BY time_of_day;


-- What is the total amount of customers that have come in each day of the week?
SELECT COUNT(order_id) AS number_of_customers, day_name
FROM order_details
GROUP BY day_name ORDER BY number_of_customers DESC;


-- ----------------------------------------------------------------------------------------
-- --------------------------- MENU ITEM QUESTIONS ----------------------------------------

-- How many unique food menu items does this restaurant have?
SELECT COUNT(DISTINCT menu_item_id) AS number_of_menu_items
FROM menu_items;

-- What categories (cuisine) does this restaurant have to offer?
SELECT DISTINCT category 
FROM menu_items;
-- ---------------------------------------------------------------------------------------
-- --------------------------- SALES QUESTIONS -------------------------------------------
 
 /* What were the least and most ordered items?
 What categories were they in? */
 SELECT COUNT(order_details.order_details_id) AS number_of_items_ordered, 
menu_items.menu_item_id, menu_items.item_name, menu_items.category
FROM order_details
	LEFT JOIN menu_items
		ON menu_items.menu_item_id = order_details.item_id
GROUP BY menu_item_id, item_name;
 
 /* What do the highest spend orders look like?
 Which items did they buy and how much did they spend? */ 
 /* S1) Firstly, I will find the total amount that each person ordered. */ 
SELECT order_details.order_id, SUM(menu_items.price) AS total_amount_each_person_spent
FROM order_details
	LEFT JOIN menu_items
		ON menu_items.menu_item_id = order_details.item_id
GROUP BY order_details.order_id
ORDER BY totaL_amount_each_person_spent DESC
LIMIT 3;

-- Next, to find what items each customer (order_id) they bought:
SELECT order_details.order_id,  
menu_items.item_name, menu_items.category
FROM order_details
	LEFT JOIN menu_items
		ON menu_items.menu_item_id = order_details.item_id
WHERE order_id IN (440, 2075,1957);

 
/* What is the gross sales for each category (cuisine) in total? How about for each month? Each Day? */

/* Which cuisines should we focus on developming more menu items for
based on the data? */

/* What were the number of sales made in each time of the day
per weekday? */ 

/* Get each category (cuisine) and add a column showing "Good," "Bad," and "Average".
Mark "Good" if it's greater than the average sales and "Bad" if it's lower than the average sales.*/

-- ------------------------------------------------------------------------------------------------
-- Shows the two tables joined: 
SELECT order_details.order_details_id, order_details.order_id, order_details.month,
order_details.day_name, order_details.time_of_day, menu_items.menu_item_id, 
menu_items.item_name, menu_items.category, menu_items.price
FROM order_details
	LEFT JOIN menu_items
		ON menu_items.menu_item_id = order_details.item_id;
