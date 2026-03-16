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


**Day 23/50 SQL Challenge**


```sql

DROP TABLE IF EXISTS amazon_transactions;
CREATE TABLE amazon_transactions (
    id SERIAL PRIMARY KEY,
    user_id INT,
    item VARCHAR(255),
    purchase_date DATE,
    revenue NUMERIC
);

INSERT INTO amazon_transactions (user_id, item, purchase_date, revenue) VALUES
(109, 'milk', '2020-03-03', 123),
(139, 'biscuit', '2020-03-18', 421),
(120, 'milk', '2020-03-18', 176),
(108, 'banana', '2020-03-18', 862),
(130, 'milk', '2020-03-28', 333),
(103, 'bread', '2020-03-29', 862),
(122, 'banana', '2020-03-07', 952),
(125, 'bread', '2020-03-13', 317),
(139, 'bread', '2020-03-30', 929),
(141, 'banana', '2020-03-17', 812),
(116, 'bread', '2020-03-31', 226),
(128, 'bread', '2020-03-04', 112),
(146, 'biscuit', '2020-03-04', 362),
(119, 'banana', '2020-03-28', 127),
(142, 'bread', '2020-03-09', 503),
(122, 'bread', '2020-03-06', 593),
(128, 'biscuit', '2020-03-24', 160),
(112, 'banana', '2020-03-24', 262),
(149, 'banana', '2020-03-29', 382),
(100, 'banana', '2020-03-18', 599),
(130, 'milk', '2020-03-16', 604),
(103, 'milk', '2020-03-31', 290),
(112, 'banana', '2020-03-23', 523),
(102, 'bread', '2020-03-25', 325),
(120, 'biscuit', '2020-03-21', 858),
(109, 'bread', '2020-03-22', 432),
(101, 'milk', '2020-03-01', 449),
(138, 'milk', '2020-03-19', 961),
(100, 'milk', '2020-03-29', 410),
(129, 'milk', '2020-03-02', 771),
(123, 'milk', '2020-03-31', 434),
(104, 'biscuit', '2020-03-31', 957),
(110, 'bread', '2020-03-13', 210),
(143, 'bread', '2020-03-27', 870),
(130, 'milk', '2020-03-12', 176),
(128, 'milk', '2020-03-28', 498),
(133, 'banana', '2020-03-21', 837),
(150, 'banana', '2020-03-20', 927),
(120, 'milk', '2020-03-27', 793),
(109, 'bread', '2020-03-02', 362),
(110, 'bread', '2020-03-13', 262),
(140, 'milk', '2020-03-09', 468),
(112, 'banana', '2020-03-04', 381),
(117, 'biscuit', '2020-03-19', 831),
(137, 'banana', '2020-03-23', 490),
(130, 'bread', '2020-03-09', 149),
(133, 'bread', '2020-03-08', 658),
(143, 'milk', '2020-03-11', 317),
(111, 'biscuit', '2020-03-23', 204),
(150, 'banana', '2020-03-04', 299),
(131, 'bread', '2020-03-10', 155),
(140, 'biscuit', '2020-03-17', 810),
(147, 'banana', '2020-03-22', 702),
(119, 'biscuit', '2020-03-15', 355),
(116, 'milk', '2020-03-12', 468),
(141, 'milk', '2020-03-14', 254),
(143, 'bread', '2020-03-16', 647),
(105, 'bread', '2020-03-21', 562),
(149, 'biscuit', '2020-03-11', 827),
(117, 'banana', '2020-03-22', 249),
(150, 'banana', '2020-03-21', 450),
(134, 'bread', '2020-03-08', 981),
(133, 'banana', '2020-03-26', 353),
(127, 'milk', '2020-03-27', 300),
(101, 'milk', '2020-03-26', 740),
(137, 'biscuit', '2020-03-12', 473),
(113, 'biscuit', '2020-03-21', 278),
(141, 'bread', '2020-03-21', 118),
(112, 'biscuit', '2020-03-14', 334),
(118, 'milk', '2020-03-30', 603),
(111, 'milk', '2020-03-19', 205),
(146, 'biscuit', '2020-03-13', 599),
(148, 'banana', '2020-03-14', 530),
(100, 'banana', '2020-03-13', 175),
(105, 'banana', '2020-03-05', 815),
(129, 'milk', '2020-03-02', 489),
(121, 'milk', '2020-03-16', 476),
(117, 'bread', '2020-03-11', 270),
(133, 'milk', '2020-03-12', 446),
(124, 'bread', '2020-03-31', 937),
(145, 'bread', '2020-03-07', 821),
(105, 'banana', '2020-03-09', 972),
(131, 'milk', '2020-03-09', 808),
(114, 'biscuit', '2020-03-31', 202),
(120, 'milk', '2020-03-06', 898),
(130, 'milk', '2020-03-06', 581),
(141, 'biscuit', '2020-03-11', 749),
(147, 'bread', '2020-03-14', 262),
(118, 'milk', '2020-03-15', 735),
(136, 'biscuit', '2020-03-22', 410),
(132, 'bread', '2020-03-06', 161),
(137, 'biscuit', '2020-03-31', 427),
(107, 'bread', '2020-03-01', 701),
(111, 'biscuit', '2020-03-18', 218),
(100, 'bread', '2020-03-07', 410),
(106, 'milk', '2020-03-21', 379),
(114, 'banana', '2020-03-25', 705),
(110, 'bread', '2020-03-27', 225),
(130, 'milk', '2020-03-16', 494),
(117, 'bread', '2020-03-10', 209);

```

```sql

SELECT * FROM amazon_transactions
LIMIT 5;

```

| id | user_id | item    | purchase_date | revenue |
|----|--------|---------|---------------|--------|
| 1  | 109    | milk    | 2020-03-03    | 123    |
| 2  | 139    | biscuit | 2020-03-18    | 421    |
| 3  | 120    | milk    | 2020-03-18    | 176    |
| 4  | 108    | banana  | 2020-03-18    | 862    |
| 5  | 130    | milk    | 2020-03-28    | 333    |



Amazon Data Analyst Interview Question

Write a query that'll identify returning active users. 

A returning active user is a user that has made a 
second purchase within 7 days of their first purchase

Output a list of user_ids of these returning active users.

```sql

WITH rows AS (SELECT *,
     			 	ROW_NUMBER()OVER (PARTITION BY user_id ORDER BY purchase_date) AS rn,
			        LEAD(purchase_date)OVER (PARTITION BY user_id) AS second_purchase
  				 FROM amazon_transactions
			 )
	
SELECT user_id AS active_users
     FROM rows 
	 WHERE rn = 1 
	   AND second_purchase-purchase_date <= 7

```


| active_users |
|--------------|
| 100 |
| 103 |
| 105 |
| 109 |
| 110 |
| 111 |
| 114 |
| 117 |
| 122 |
| 129 |
| 130 |
| 131 |
| 133 |
| 141 |
| 143 |



Find the user_id who has not purchased anything for 7 days 
after first purchase but they have done second purchase after 7 days 

```sql

WITH rows AS (SELECT *,
     			 	ROW_NUMBER()OVER (PARTITION BY user_id ORDER BY purchase_date) AS rn,
			        LEAD(purchase_date)OVER (PARTITION BY user_id) AS second_purchase
  				 FROM amazon_transactions
			 )
	
SELECT user_id AS non_active_user
     FROM rows 
	 WHERE rn = 1 
	   AND second_purchase-purchase_date > 7

```

| non_active_user |
|-----------------|
| 101 |
| 112 |
| 116 |
| 118 |
| 119 |
| 120 |
| 128 |
| 137 |
| 139 |
| 140 |
| 146 |
| 147 |
| 149 |
| 150 |


--------------------------------------------------------------------------------------------------------------------------------------------

**Day 24/50 Days**


```sql

DROP TABLE IF EXISTS orders;

CREATE TABLE orders (
    id INT,
    cust_id INT,
    order_date DATE,
    order_details VARCHAR(50),
    total_order_cost INT
);

INSERT INTO orders (id, cust_id, order_date, order_details, total_order_cost) VALUES
(1, 7, '2019-03-04', 'Coat', 100),
(2, 7, '2019-03-01', 'Shoes', 80),
(3, 3, '2019-03-07', 'Skirt', 30),
(4, 7, '2019-02-01', 'Coat', 25),
(5, 7, '2019-03-10', 'Shoes', 80),
(6, 1, '2019-02-01', 'Boats', 100),
(7, 2, '2019-01-11', 'Shirts', 60),
(8, 1, '2019-03-11', 'Slipper', 20),
(9, 15, '2019-03-01', 'Jeans', 80),
(10, 15, '2019-03-09', 'Shirts', 50),
(11, 5, '2019-02-01', 'Shoes', 80),
(12, 12, '2019-01-11', 'Shirts', 60),
(13, 1, '2019-03-11', 'Slipper', 20),
(14, 4, '2019-02-01', 'Shoes', 80),
(15, 4, '2019-01-11', 'Shirts', 60),
(16, 3, '2019-04-19', 'Shirts', 50),
(17, 7, '2019-04-19', 'Suit', 150),
(18, 15, '2019-04-19', 'Skirt', 30),
(19, 15, '2019-04-20', 'Dresses', 200),
(20, 12, '2019-01-11', 'Coat', 125),
(21, 7, '2019-04-01', 'Suit', 50),
(22, 3, '2019-04-02', 'Skirt', 30),
(23, 4, '2019-04-03', 'Dresses', 50),
(24, 2, '2019-04-04', 'Coat', 25),
(25, 7, '2019-04-19', 'Coat', 125);

```


Calculate the total revenue from 
each customer in March 2019. 

Include only customers who 
were active in March 2019.

Output the revenue along with the 
customer id and sort the results based 
on the revenue in descending order.




```sql 

SELECT cust_id,
       SUM(total_order_cost) AS total_cost
	FROM orders 
	WHERE EXTRACT(MONTH FROM order_date) = 3
	GROUP BY cust_id;

```


| cust_id | total_cost |
|---------|------------|
| 1 | 	40 |
| 3 |	30 |
| 7 |	260 |
| 15 |	130 |



Find the customers who purchased from both 
March and April of 2019 and their total revenue.

```sql

SELECT cust_id,
       SUM(total_order_cost) AS total_cost
	FROM orders 
	WHERE EXTRACT(MONTH FROM order_date) = 3 
	  OR EXTRACT(MONTH FROM order_date) = 4
	GROUP BY cust_id;

```

| cust_id | total_cost |
|---------|------------|
| 1 |	40 |
| 2 |	25 |
| 3 |	110 |
| 4 |	50 |
| 7 |	585 |
| 15 |	360 |

--------------------------------------------------------------------------------------------------------------------------------------------

**Day 25/50 days sql challenge**

```sql

DROP TABLE customers;
-- Creating the customers table
CREATE TABLE customers (
    id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    city VARCHAR(50),
    address VARCHAR(100),
    phone_number VARCHAR(20)
);

-- Inserting sample data into the customers table
INSERT INTO customers (id, first_name, last_name, city, address, phone_number) VALUES
    (8, 'John', 'Joseph', 'San Francisco', NULL, '928868164'),
    (7, 'Jill', 'Michael', 'Austin', NULL, '8130567692'),
    (4, 'William', 'Daniel', 'Denver', NULL, '813155200'),
    (5, 'Henry', 'Jackson', 'Miami', NULL, '8084557513'),
    (13, 'Emma', 'Isaac', 'Miami', NULL, '808690201'),
    (14, 'Liam', 'Samuel', 'Miami', NULL, '808555201'),
    (15, 'Mia', 'Owen', 'Miami', NULL, '806405201'),
    (1, 'Mark', 'Thomas', 'Arizona', '4476 Parkway Drive', '602325916'),
    (12, 'Eva', 'Lucas', 'Arizona', '4379 Skips Lane', '3019509805'),
    (6, 'Jack', 'Aiden', 'Arizona', '4833 Coplin Avenue', '480230527'),
    (2, 'Mona', 'Adrian', 'Los Angeles', '1958 Peck Court', '714939432'),
    (10, 'Lili', 'Oliver', 'Los Angeles', '3832 Euclid Avenue', '5306951180'),
    (3, 'Farida', 'Joseph', 'San Francisco', '3153 Rhapsody Street', '8133681200'),
    (9, 'Justin', 'Alexander', 'Denver', '4470 McKinley Avenue', '9704337589'),
    (11, 'Frank', 'Jacob', 'Miami', '1299 Randall Drive', '8085905201');





-- Creating the orders table
CREATE TABLE orders (
    id INT PRIMARY KEY,
    cust_id INT,
    order_date DATE,
    order_details VARCHAR(100),
    total_order_cost INT
);

-- Inserting sample data into the orders table
INSERT INTO orders (id, cust_id, order_date, order_details, total_order_cost) VALUES
    (1, 3, '2019-03-04', 'Coat', 100),
    (2, 3, '2019-03-01', 'Shoes', 80),
    (3, 3, '2019-03-07', 'Skirt', 30),
    (4, 7, '2019-02-01', 'Coat', 25),
    (5, 7, '2019-03-10', 'Shoes', 80),
    (6, 15, '2019-02-01', 'Boats', 100),
    (7, 15, '2019-01-11', 'Shirts', 60),
    (8, 15, '2019-03-11', 'Slipper', 20),
    (9, 15, '2019-03-01', 'Jeans', 80),
    (10, 15, '2019-03-09', 'Shirts', 50),
    (11, 5, '2019-02-01', 'Shoes', 80),
    (12, 12, '2019-01-11', 'Shirts', 60),
    (13, 12, '2019-03-11', 'Slipper', 20),
    (14, 4, '2019-02-01', 'Shoes', 80),
    (15, 4, '2019-01-11', 'Shirts', 60),
    (16, 3, '2019-04-19', 'Shirts', 50),
    (17, 7, '2019-04-19', 'Suit', 150),
    (18, 15, '2019-04-19', 'Skirt', 30),
    (19, 15, '2019-04-20', 'Dresses', 200),
    (20, 12, '2019-01-11', 'Coat', 125),
    (21, 7, '2019-04-01', 'Suit', 50),
    (22, 7, '2019-04-02', 'Skirt', 30),
    (23, 7, '2019-04-03', 'Dresses', 50),
    (24, 7, '2019-04-04', 'Coat', 25),
    (25, 7, '2019-04-19', 'Coat', 125);

```


```sql

SELECT * FROM customers
LIMIT 5;

```

| id | first_name | last_name | city          | address | phone_number |
|----|------------|-----------|---------------|---------|--------------|
| 8  | John       | Joseph    | San Francisco |         | 928868164    |
| 7  | Jill       | Michael   | Austin        |         | 8130567692   |
| 4  | William    | Daniel    | Denver        |         | 813155200    |
| 5  | Henry      | Jackson   | Miami         |         | 8084557513   |
| 13 | Emma       | Isaac     | Miami         |         | 808690201    |

```sql

SELECT * FROM orders
LIMIT 5;

```

| id | cust_id | order_date | order_details | total_order_cost |
|----|---------|------------|---------------|------------------|
| 1  | 3       | 2019-03-04 | Coat          | 100              |
| 2  | 3       | 2019-03-01 | Shoes         | 80               |
| 3  | 3       | 2019-03-07 | Skirt         | 30               |
| 4  | 7       | 2019-02-01 | Coat          | 25               |
| 5  | 7       | 2019-03-10 | Shoes         | 80               |



You have given two tables customers with columns (id, name phone
address) and orders table columns(order_id, cxid order_date and cost)

Find the percentage of shippable orders.
Consider an order is shippable if the customer's address is known.


```sql

SELECT ROUND(100*(SELECT COUNT(o.id) FROM orders AS o
                         JOIN customers AS c ON c.id = o.cust_id
                         WHERE address NOTNULL)/COUNT(o.id),2) AS shippable_orders
      FROM orders AS o
```
 
| shippable_orders|
|-----------------|
| 28.00 |


Find out the percentage of orders where the customer
doesn't have a valid phone number.

-- Note 
The Length of valid phone no is 10 character 


```sql

WITH valid_no AS (SELECT *,
				           o.id AS order_id,
						   CASE WHEN LENGTH(c.phone_number) = 10 THEN 1 
						   ELSE 0 
						   END AS valid_phone
					  FROM orders AS o 
					  JOIN customers AS C ON c.id = o.cust_id
				 )
	SELECT ROUND(100*SUM(valid_phone)/COUNT(order_id),2) AS shippable_orders_percentage
	     FROM valid_no;
```

| shippable_orders_percentage |
|-----------------------------|
| 64.00 |

--------------------------------------------------------------------------------------------------------------------------------------------
