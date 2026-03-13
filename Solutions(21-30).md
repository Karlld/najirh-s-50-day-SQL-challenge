**Day 21/50 Days SQL Challenge**

```sql

DROP TABLE IF EXISTS products;
-- Creating the products table
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    price DECIMAL(10, 2),
    quantity_sold INT
);

-- Inserting sample data for products
INSERT INTO products (product_id, product_name, price, quantity_sold) VALUES
    (1, 'iPhone', 899.00, 600),
    (2, 'iMac', 1299.00, 150),
    (3, 'MacBook Pro', 1499.00, 500),
    (4, 'AirPods', 499.00, 800),
    (5, 'Accessories', 199.00, 300);


SELECT * FROM products;

```

| product_id | product_name | price  | quantity_sold |
|------------|--------------|--------|---------------|
| 1          | iPhone       | 899.00 | 600           |
| 2          | iMac         | 1299.00| 150           |
| 3          | MacBook Pro  | 1499.00| 500           |
| 4          | AirPods      | 499.00 | 800           |
| 5          | Accessories  | 199.00 | 300           |



You have a table called products with below columns
product_id, product_name, price, qty 

Calculate the percentage contribution of each product 
to total revenue?

Round the result into 2 decimal


```sql

SELECT product_id, 
       product_name,
	   price*quantity_sold AS product_revenue,
	   ROUND(100*(price * quantity_sold)/
			(SELECT SUM(price * quantity_sold) FROM products),2) 
			   AS product_revenue_percentage
	  
	FROM products;


```
	
   
| product_id | product_name  | product_revenue | product_revenue_percentage |
|------------|---------------|----------------|----------------------------|
| 1          | iPhone        | 539400.00      | 27.77                      |
| 2          | iMac          | 194850.00      | 10.03                      |
| 3          | MacBook Pro   | 749500.00      | 38.58                      |
| 4          | AirPods       | 399200.00      | 20.55                      |
| 5          | Accessories   | 59700.00       | 3.07                       |


--------------------------------------------------------------------------------------------------------------------------------------------

**Day 22/50 SQL Challenge**

```sql

DROP TABLE IF EXISTS delivery;
-- Create the Delivery table
CREATE TABLE Delivery (
    delivery_id SERIAL PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    customer_pref_delivery_date DATE
);

-- Insert data into the Delivery table
INSERT INTO Delivery (customer_id, order_date, customer_pref_delivery_date) VALUES
(1, '2019-08-01', '2019-08-02'),
(2, '2019-08-02', '2019-08-02'),
(1, '2019-08-11', '2019-08-12'),
(3, '2019-08-24', '2019-08-24'),
(3, '2019-08-21', '2019-08-22'),
(2, '2019-08-11', '2019-08-13'),
(4, '2019-08-09', '2019-08-09'),
(5, '2019-08-09', '2019-08-10'),
(4, '2019-08-10', '2019-08-12'),
(6, '2019-08-09', '2019-08-11'),
(7, '2019-08-12', '2019-08-13'),
(8, '2019-08-13', '2019-08-13'),
(9, '2019-08-11', '2019-08-12');    


SELECT * FROM delivery;

```

| delivery_id | customer_id | order_date  | customer_pref_delivery_date |
|------------|-------------|------------|----------------------------|
| 1          | 1           | 2019-08-01 | 2019-08-02                 |
| 2          | 2           | 2019-08-02 | 2019-08-02                 |
| 3          | 1           | 2019-08-11 | 2019-08-12                 |
| 4          | 3           | 2019-08-24 | 2019-08-24                 |
| 5          | 3           | 2019-08-21 | 2019-08-22                 |
| 6          | 2           | 2019-08-11 | 2019-08-13                 |
| 7          | 4           | 2019-08-09 | 2019-08-09                 |
| 8          | 5           | 2019-08-09 | 2019-08-10                 |
| 9          | 4           | 2019-08-10 | 2019-08-12                 |
| 10         | 6           | 2019-08-09 | 2019-08-11                 |
| 11         | 7           | 2019-08-12 | 2019-08-13                 |
| 12         | 8           | 2019-08-13 | 2019-08-13                 |
| 13         | 9           | 2019-08-11 | 2019-08-12                 |


You have dataset of a food delivery company
with columns order_id, customer_id, order_date, 
pref_delivery_date


If the customer's preferred delivery date is 
the same as the order date, then the order is 
called immediate; otherwise, it is called scheduled.


Write a solution to find the percentage of immediate
orders in the first orders of all customers, 
rounded to 2 decimal places.


```sql


WITH rows AS (SELECT *,  
      ROW_NUMBER()OVER (PARTITION BY customer_id ORDER BY order_date) as rn
	FROM delivery
			 ),

order_type AS (SELECT delivery_id,
				   customer_id,
				   order_date,
				   customer_pref_delivery_date,
				   CASE WHEN order_date = customer_pref_delivery_date THEN 1
						ELSE 0
						END AS immediate
			       FROM rows
			       WHERE rn = 1
		   )
	
SELECT ROUND(100*SUM(immediate)/COUNT(immediate),2) AS immediate_percentage
    FROM order_type;

```


| immediate_percentage |
|----------------------|
| 33.00 |



Write an SQL query to determine the percentage
of orders where customers select next day delivery. 
We're excited to see your solution! 

```sql



WITH order_type AS (SELECT delivery_id,
				   customer_id,
				   order_date,
				   customer_pref_delivery_date,
				   CASE WHEN (order_date + INTERVAL '1 day') = customer_pref_delivery_date THEN 1
						ELSE 0
						END AS next_day
			       FROM  delivery
		   )
	
SELECT ROUND(100*SUM(next_day)/COUNT(next_day),2) AS next_day_percentage
    FROM order_type;

```

| next_day_percentage |
|---------------------|
| 46.00 |

--------------------------------------------------------------------------------------------------------------------------------------------


**Day 22/50 SQL Challenge**

```sql

DROP TABLE IF EXISTS delivery;
-- Create the Delivery table
CREATE TABLE Delivery (
    delivery_id SERIAL PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    customer_pref_delivery_date DATE
);

-- Insert data into the Delivery table
INSERT INTO Delivery (customer_id, order_date, customer_pref_delivery_date) VALUES
(1, '2019-08-01', '2019-08-02'),
(2, '2019-08-02', '2019-08-02'),
(1, '2019-08-11', '2019-08-12'),
(3, '2019-08-24', '2019-08-24'),
(3, '2019-08-21', '2019-08-22'),
(2, '2019-08-11', '2019-08-13'),
(4, '2019-08-09', '2019-08-09'),
(5, '2019-08-09', '2019-08-10'),
(4, '2019-08-10', '2019-08-12'),
(6, '2019-08-09', '2019-08-11'),
(7, '2019-08-12', '2019-08-13'),
(8, '2019-08-13', '2019-08-13'),
(9, '2019-08-11', '2019-08-12');    

```

```sql

SELECT * FROM delivery;

```

| delivery_id | customer_id | order_date | customer_pref_delivery_date |
|-------------|-------------|------------|------------------------------|
| 1  | 1 | 2019-08-01 | 2019-08-02 |
| 2  | 2 | 2019-08-02 | 2019-08-02 |
| 3  | 1 | 2019-08-11 | 2019-08-12 |
| 4  | 3 | 2019-08-24 | 2019-08-24 |
| 5  | 3 | 2019-08-21 | 2019-08-22 |
| 6  | 2 | 2019-08-11 | 2019-08-13 |
| 7  | 4 | 2019-08-09 | 2019-08-09 |
| 8  | 5 | 2019-08-09 | 2019-08-10 |
| 9  | 4 | 2019-08-10 | 2019-08-12 |
| 10 | 6 | 2019-08-09 | 2019-08-11 |
| 11 | 7 | 2019-08-12 | 2019-08-13 |
| 12 | 8 | 2019-08-13 | 2019-08-13 |
| 13 | 9 | 2019-08-11 | 2019-08-12 |




You have dataset of a food delivery company
with columns order_id, customer_id, order_date, 
pref_delivery_date


If the customer's preferred delivery date is 
the same as the order date, then the order is 
called immediate; otherwise, it is called scheduled.


Write a solution to find the percentage of immediate
orders in the first orders of all customers, 
rounded to 2 decimal places.


```sql

WITH rows AS (SELECT *,  
      ROW_NUMBER()OVER (PARTITION BY customer_id ORDER BY order_date) as rn
	FROM delivery
			 ),

order_type AS (SELECT delivery_id,
				   customer_id,
				   order_date,
				   customer_pref_delivery_date,
				   CASE WHEN order_date = customer_pref_delivery_date THEN 1
						ELSE 0
						END AS immediate
			       FROM rows
			       WHERE rn = 1
		   )
	
SELECT ROUND(100*SUM(immediate)/COUNT(immediate),2) AS immediate_percentage
    FROM order_type;

```

| immediate_percentage |
|----------------------|
| 33.00 |


Write an SQL query to determine the percentage
of orders where customers select next day delivery. 
We're excited to see your solution! 

```sql

WITH order_type AS (SELECT delivery_id,
				   customer_id,
				   order_date,
				   customer_pref_delivery_date,
				   CASE WHEN (order_date + INTERVAL '1 day') = customer_pref_delivery_date THEN 1
						ELSE 0
						END AS next_day
			       FROM  delivery
		   )
	
SELECT ROUND(100*SUM(next_day)/COUNT(next_day),2) AS next_day_percentage
    FROM order_type;

```

| next_day_percentage |
|---------------------|
| 46.00 |

--------------------------------------------------------------------------------------------------------------------------------------------
