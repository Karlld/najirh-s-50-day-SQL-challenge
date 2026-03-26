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

**SQL Challenge 26/50**


```sql

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100),
    department VARCHAR(100),
    salary DECIMAL(10, 2),
    manager_id INT
);

INSERT INTO employees (employee_id, employee_name, department, salary, manager_id)
VALUES
    (1, 'John Doe', 'HR', 50000.00, NULL),
    (2, 'Jane Smith', 'HR', 55000.00, 1),
    (3, 'Michael Johnson', 'HR', 60000.00, 1),
    (4, 'Emily Davis', 'IT', 60000.00, NULL),
    (5, 'David Brown', 'IT', 65000.00, 4),
    (6, 'Sarah Wilson', 'Finance', 70000.00, NULL),
    (7, 'Robert Taylor', 'Finance', 75000.00, 6),
    (8, 'Jennifer Martinez', 'Finance', 80000.00, 6);

```


You have a employees table with columns emp_id, emp_name,
department, salary, manager_id (manager is also emp in the table))

Identify employees who have a higher salary than their manager. 

```sql

SELECT e1.employee_name,
       e1.salary,
	   e2.employee_name AS manager,
	   e2.salary AS manager_salary
	FROM employees AS e1
    LEFT JOIN employees AS e2 ON e1.manager_id = e2.employee_id
	WHERE e1.salary > e2.salary 
```

| employee_name      | salary   | manager        | manager_salary |
|--------------------|----------|---------------|----------------|
| Jane Smith         | 55000.00 | John Doe      | 50000.00       |
| Michael Johnson    | 60000.00 | John Doe      | 50000.00       |
| David Brown        | 65000.00 | Emily Davis   | 60000.00       |
| Robert Taylor      | 75000.00 | Sarah Wilson  | 70000.00       |
| Jennifer Martinez  | 80000.00 | Sarah Wilson  | 70000.00       |


Find all the employees who have a salary greater than average salary?

```sql

SELECT employee_name,
       salary,
      (SELECT ROUND(AVG(salary)) FROM employees) AS average
	FROM employees
	WHERE salary > (SELECT ROUND(AVG(salary)) FROM employees);
```

| employee_name       | salary   | average |
|---------------------|----------|---------|
| David Brown         | 65000.00 | 64375   |
| Sarah Wilson        | 70000.00 | 64375   |
| Robert Taylor       | 75000.00 | 64375   |
| Jennifer Martinez   | 80000.00 | 64375   |



-------------------------------------------------------------------------------------------------------------------------------------------


**Day 27/50 SQL challenge**

```sql

DROP TABLE IF EXISTS walmart_eu;
-- Create the online_retail table
CREATE TABLE walmart_eu (
    invoiceno VARCHAR(255),
    stockcode VARCHAR(255),
    description VARCHAR(255),
    quantity INT,
    invoicedate DATE,
    unitprice FLOAT,
    customerid FLOAT,
    country VARCHAR(255)
);

-- Insert the provided data into the online_retail table
INSERT INTO walmart_eu (invoiceno, stockcode, description, quantity, invoicedate, unitprice, customerid, country) VALUES
('544586', '21890', 'S/6 WOODEN SKITTLES IN COTTON BAG', 3, '2011-02-21', 2.95, 17338, 'United Kingdom'),
('541104', '84509G', 'SET OF 4 FAIRY CAKE PLACEMATS', 3, '2011-01-13', 3.29, NULL, 'United Kingdom'),
('560772', '22499', 'WOODEN UNION JACK BUNTING', 3, '2011-07-20', 4.96, NULL, 'United Kingdom'),
('555150', '22488', 'NATURAL SLATE RECTANGLE CHALKBOARD', 5, '2011-05-31', 3.29, NULL, 'United Kingdom'),
('570521', '21625', 'VINTAGE UNION JACK APRON', 3, '2011-10-11', 6.95, 12371, 'Switzerland'),
('547053', '22087', 'PAPER BUNTING WHITE LACE', 40, '2011-03-20', 2.55, 13001, 'United Kingdom'),
('573360', '22591', 'CARDHOLDER GINGHAM CHRISTMAS TREE', 6, '2011-10-30', 3.25, 15748, 'United Kingdom'),
('571039', '84536A', 'ENGLISH ROSE NOTEBOOK A7 SIZE', 1, '2011-10-13', 0.42, 16121, 'United Kingdom'),
('578936', '20723', 'STRAWBERRY CHARLOTTE BAG', 10, '2011-11-27', 0.85, 16923, 'United Kingdom'),
('559338', '21391', 'FRENCH LAVENDER SCENT HEART', 1, '2011-07-07', 1.63, NULL, 'United Kingdom'),
('568134', '23171', 'REGENCY TEA PLATE GREEN', 1, '2011-09-23', 3.29, NULL, 'United Kingdom'),
('552061', '21876', 'POTTERING MUG', 12, '2011-05-06', 1.25, 13001, 'United Kingdom'),
('543179', '22531', 'MAGIC DRAWING SLATE CIRCUS PARADE', 1, '2011-02-04', 0.42, 12754, 'Japan'),
('540954', '22381', 'TOY TIDY PINK POLKADOT', 4, '2011-01-12', 2.1, 14606, 'United Kingdom'),
('572703', '21818', 'GLITTER HEART DECORATION', 13, '2011-10-25', 0.39, 16110, 'United Kingdom'),
('578757', '23009', 'I LOVE LONDON BABY GIFT SET', 1, '2011-11-25', 16.95, 12748, 'United Kingdom'),
('542616', '22505', 'MEMO BOARD COTTAGE DESIGN', 4, '2011-01-30', 4.95, 16816, 'United Kingdom'),
('554694', '22921', 'HERB MARKER CHIVES', 1, '2011-05-25', 1.63, NULL, 'United Kingdom'),
('569545', '21906', 'PHARMACIE FIRST AID TIN', 1, '2011-10-04', 13.29, NULL, 'United Kingdom'),
('549562', '21169', 'YOU''RE CONFUSING ME METAL SIGN', 1, '2011-04-10', 1.69, 13232, 'United Kingdom'),
('580610', '21945', 'STRAWBERRIES DESIGN FLANNEL', 1, '2011-12-05', 1.63, NULL, 'United Kingdom'),
('558066', 'gift_0001_50', 'Dotcomgiftshop Gift Voucher £50.00', 1, '2011-06-24', 41.67, NULL, 'United Kingdom'),
('538349', '21985', 'PACK OF 12 HEARTS DESIGN TISSUES', 1, '2010-12-10', 0.85, NULL, 'United Kingdom'),
('537685', '22737', 'RIBBON REEL CHRISTMAS PRESENT', 15, '2010-12-08', 1.65, 18077, 'United Kingdom'),
('545906', '22614', 'PACK OF 12 SPACEBOY TISSUES', 24, '2011-03-08', 0.29, 15764, 'United Kingdom'),
('550997', '22629', 'SPACEBOY LUNCH BOX', 12, '2011-04-26', 1.95, 17735, 'United Kingdom'),
('558763', '22960', 'JAM MAKING SET WITH JARS', 3, '2011-07-03', 4.25, 12841, 'United Kingdom'),
('562688', '22918', 'HERB MARKER PARSLEY', 12, '2011-08-08', 0.65, 13869, 'United Kingdom'),
('541424', '84520B', 'PACK 20 ENGLISH ROSE PAPER NAPKINS', 9, '2011-01-17', 1.63, NULL, 'United Kingdom'),
('581405', '20996', 'JAZZ HEARTS ADDRESS BOOK', 1, '2011-12-08', 0.19, 13521, 'United Kingdom'),
('571053', '23256', 'CHILDRENS CUTLERY SPACEBOY', 4, '2011-10-13', 4.15, 12631, 'Finland'),
('563333', '23012', 'GLASS APOTHECARY BOTTLE PERFUME', 1, '2011-08-15', 3.95, 15996, 'United Kingdom'),
('568054', '47559B', 'TEA TIME OVEN GLOVE', 4, '2011-09-23', 1.25, 16978, 'United Kingdom'),
('574262', '22561', 'WOODEN SCHOOL COLOURING SET', 12, '2011-11-03', 1.65, 13721, 'United Kingdom'),
('569360', '23198', 'PANTRY MAGNETIC SHOPPING LIST', 6, '2011-10-03', 1.45, 14653, 'United Kingdom'),
('570210', '22980', 'PANTRY SCRUBBING BRUSH', 2, '2011-10-09', 1.65, 13259, 'United Kingdom'),
('576599', '22847', 'BREAD BIN DINER STYLE IVORY', 1, '2011-11-15', 16.95, 14544, 'United Kingdom'),
('579777', '22356', 'CHARLOTTE BAG PINK POLKADOT', 4, '2011-11-30', 1.63, NULL, 'United Kingdom'),
('566060', '21106', 'CREAM SLICE FLANNEL CHOCOLATE SPOT', 1, '2011-09-08', 5.79, NULL, 'United Kingdom'),
('550514', '22489', 'PACK OF 12 TRADITIONAL CRAYONS', 24, '2011-04-18', 0.42, 14631, 'United Kingdom'),
('569898', '23437', '50''S CHRISTMAS GIFT BAG LARGE', 2, '2011-10-06', 2.46, NULL, 'United Kingdom'),
('563566', '23548', 'WRAP MAGIC FOREST', 25, '2011-08-17', 0.42, 13655, 'United Kingdom'),
('559693', '21169', 'YOU''RE CONFUSING ME METAL SIGN', 1, '2011-07-11', 4.13, NULL, 'United Kingdom'),
('573386', '22112', 'CHOCOLATE HOT WATER BOTTLE', 24, '2011-10-30', 4.25, 17183, 'United Kingdom'),
('576920', '23312', 'VINTAGE CHRISTMAS GIFT SACK', 4, '2011-11-17', 4.15, 13871, 'United Kingdom'),
('564473', '22384', 'LUNCH BAG PINK POLKADOT', 10, '2011-08-25', 1.65, 16722, 'United Kingdom'),
('562264', '23321', 'SMALL WHITE HEART OF WICKER', 3, '2011-08-03', 3.29, NULL, 'United Kingdom'),
('542541', '79030D', 'TUMBLER, BAROQUE', 1, '2011-01-28', 12.46, NULL, 'United Kingdom'),
('579937', '22090', 'PAPER BUNTING RETROSPOT', 12, '2011-12-01', 2.95, 13509, 'United Kingdom'),
('574076', '22483', 'RED GINGHAM TEDDY BEAR', 1, '2011-11-02', 5.79, NULL, 'United Kingdom'),
('579187', '20665', 'RED RETROSPOT PURSE', 1, '2011-11-28', 5.79, NULL, 'United Kingdom'),
('542922', '22423', 'REGENCY CAKESTAND 3 TIER', 3, '2011-02-02', 12.75, 12682, 'France'),
('570677', '23008', 'DOLLY GIRL BABY GIFT SET', 2, '2011-10-11', 16.95, 12836, 'United Kingdom'),
('577182', '21930', 'JUMBO STORAGE BAG SKULLS', 10, '2011-11-18', 2.08, 16945, 'United Kingdom'),
('576686', '20992', 'JAZZ HEARTS PURSE NOTEBOOK', 1, '2011-11-16', 0.39, 16916, 'United Kingdom'),
('553844', '22569', 'FELTCRAFT CUSHION BUTTERFLY', 4, '2011-05-19', 3.75, 13450, 'United Kingdom'),
('580689', '23150', 'IVORY SWEETHEART SOAP DISH', 6, '2011-12-05', 2.49, 12994, 'United Kingdom'),
('545000', '85206A', 'CREAM FELT EASTER EGG BASKET', 6, '2011-02-25', 1.65, 15281, 'United Kingdom'),
('541975', '22382', 'LUNCH BAG SPACEBOY DESIGN', 40, '2011-01-24', 1.65, NULL, 'Hong Kong'),
('544942', '22551', 'PLASTERS IN TIN SPACEBOY', 12, '2011-02-25', 1.65, 15544, 'United Kingdom'),
('543177', '22667', 'RECIPE BOX RETROSPOT', 6, '2011-02-04', 2.95, 14466, 'United Kingdom'),
('574587', '23356', 'LOVE HOT WATER BOTTLE', 4, '2011-11-06', 5.95, 14936, 'Channel Islands'),
('543451', '22774', 'RED DRAWER KNOB ACRYLIC EDWARDIAN', 1, '2011-02-08', 2.46, NULL, 'United Kingdom'),
('578270', '22579', 'WOODEN TREE CHRISTMAS SCANDINAVIAN', 1, '2011-11-23', 1.63, 14096, 'United Kingdom'),
('551413', '84970L', 'SINGLE HEART ZINC T-LIGHT HOLDER', 12, '2011-04-28', 0.95, 16227, 'United Kingdom'),
('567666', '22900', 'SET 2 TEA TOWELS I LOVE LONDON', 6, '2011-09-21', 3.25, 12520, 'Germany'),
('571544', '22810', 'SET OF 6 T-LIGHTS SNOWMEN', 2, '2011-10-17', 2.95, 17757, 'United Kingdom'),
('558368', '23249', 'VINTAGE RED ENAMEL TRIM PLATE', 12, '2011-06-28', 1.65, 14329, 'United Kingdom'),
('546430', '22284', 'HEN HOUSE DECORATION', 2, '2011-03-13', 1.65, 15918, 'United Kingdom'),
('565233', '23000', 'TRAVEL CARD WALLET TRANSPORT', 1, '2011-09-02', 0.83, NULL, 'United Kingdom'),
('559984', '16012', 'FOOD/DRINK SPONGE STICKERS', 50, '2011-07-14', 0.21, 16657, 'United Kingdom'),
('576920', '23312', 'VINTAGE CHRISTMAS GIFT SACK', -4, '2011-11-17', 4.15, 13871, 'United Kingdom'),
('564473', '22384', 'LUNCH BAG PINK POLKADOT', 10, '2011-08-25', 1.65, 16722, 'United Kingdom'),
('562264', '23321', 'SMALL WHITE HEART OF WICKER', 3, '2011-08-03', 3.29, NULL, 'United Kingdom'),
('542541', '79030D', 'TUMBLER, BAROQUE', 1, '2011-01-28', 12.46, NULL, 'United Kingdom'),
('579937', '22090', 'PAPER BUNTING RETROSPOT', 12, '2011-12-01', 2.95, 13509, 'United Kingdom'),
('574076', '22483', 'RED GINGHAM TEDDY BEAR', 1, '2011-11-02', 5.79, NULL, 'United Kingdom'),
('579187', '20665', 'RED RETROSPOT PURSE', 1, '2011-11-28', 5.79, NULL, 'United Kingdom'),
('542922', '22423', 'REGENCY CAKESTAND 3 TIER', 3, '2011-02-02', 12.75, 12682, 'France'),
('570677', '23008', 'DOLLY GIRL BABY GIFT SET', 2, '2011-10-11', 16.95, 12836, 'United Kingdom'),
('577182', '21930', 'JUMBO STORAGE BAG SKULLS', 10, '2011-11-18', 2.08, 16945, 'United Kingdom'),
('576686', '20992', 'JAZZ HEARTS PURSE NOTEBOOK', 1, '2011-11-16', 0.39, 16916, 'United Kingdom'),
('553844', '22569', 'FELTCRAFT CUSHION BUTTERFLY', 4, '2011-05-19', 3.75, 13450, 'United Kingdom'),
('580689', '23150', 'IVORY SWEETHEART SOAP DISH', 6, '2011-12-05', 2.49, 12994, 'United Kingdom'),
('545000', '85206A', 'CREAM FELT EASTER EGG BASKET', 6, '2011-02-25', 1.65, 15281, 'United Kingdom'),
('541975', '22382', 'LUNCH BAG SPACEBOY DESIGN', 40, '2011-01-24', 1.65, NULL, 'Hong Kong'),
('544942', '22551', 'PLASTERS IN TIN SPACEBOY', 12, '2011-02-25', 1.65, 15544, 'United Kingdom'),
('543177', '22667', 'RECIPE BOX RETROSPOT', 6, '2011-02-04', 2.95, 14466, 'United Kingdom'),
('574587', '23356', 'LOVE HOT WATER BOTTLE', 4, '2011-11-06', 5.95, 14936, 'Channel Islands'),
('543451', '22774', 'RED DRAWER KNOB ACRYLIC EDWARDIAN', 1, '2011-02-08', 2.46, NULL, 'United Kingdom'),
('578270', '22579', 'WOODEN TREE CHRISTMAS SCANDINAVIAN', 1, '2011-11-23', 1.63, 14096, 'United Kingdom'),
('551413', '84970L', 'SINGLE HEART ZINC T-LIGHT HOLDER', 12, '2011-04-28', 0.95, 16227, 'United Kingdom'),
('567666', '22900', 'SET 2 TEA TOWELS I LOVE LONDON', 6, '2011-09-21', 3.25, 12520, 'Germany'),
('571544', '22810', 'SET OF 6 T-LIGHTS SNOWMEN', 2, '2011-10-17', 2.95, 17757, 'United Kingdom'),
('558368', '23249', 'VINTAGE RED ENAMEL TRIM PLATE', 12, '2011-06-28', 1.65, 14329, 'United Kingdom'),
('546430', '22284', 'HEN HOUSE DECORATION', 2, '2011-03-13', 1.65, 15918, 'United Kingdom'),
('565233', '23000', 'TRAVEL CARD WALLET TRANSPORT', 1, '2011-09-02', 0.83, NULL, 'United Kingdom'),
('559984', '16012', 'FOOD/DRINK SPONGE STICKERS', 50, '2011-07-14', 0.21, 16657, 'United Kingdom');

```



Find the best selling item for each month 
(no need to separate months by year) 
where the biggest total invoice was paid. 

```sql

SELECT month_no,
       month,
       MAX(total)
    FROM (SELECT stockcode,
				   description,
				   ROUND(SUM(quantity*unitprice)::DECIMAL,2) AS total,
		           EXTRACT(month FROM invoicedate) AS month_no,
				   TO_CHAR(invoicedate, 'month') AS month
			FROM walmart_eu
			GROUP BY stockcode, description, EXTRACT(month FROM invoicedate),TO_CHAR(invoicedate, 'month')
			ORDER BY month
		  )
  GROUP BY month_no, month
  ORDER BY month_no; 

```

| month_no | month      | max   |
|----------|-----------|-------|
| 1        | January   | 132.00|
| 2        | February  | 76.50 |
| 3        | March     | 102.00|
| 4        | April     | 23.40 |
| 5        | May       | 30.00 |
| 6        | June      | 41.67 |
| 7        | July      | 21.00 |
| 8        | August    | 33.00 |
| 9        | September | 39.00 |
| 10       | October   | 102.00|
| 11       | November  | 47.60 |
| 12       | December  | 70.80 |


Find Customer of the month from each MONTH - the one customer who has spent the highest amount (price * quantity) as total amount may include multiple purchase.

```sql

WITH total_spends AS  (SELECT customerid,
							  EXTRACT(month FROM invoicedate) AS month_no,
							  TO_CHAR(invoicedate, 'month') AS month,
							  ROUND(SUM(quantity * unitprice)::DECIMAL,2) AS total_spent,
							  ROW_NUMBER()OVER(PARTITION BY EXTRACT(month FROM invoicedate) 
											 ORDER BY ROUND(SUM(quantity * unitprice)::DECIMAL,2)DESC) AS rn
						FROM walmart_eu
						WHERE customerid NOTNULL
						GROUP BY EXTRACT(month FROM invoicedate),TO_CHAR(invoicedate, 'month'), customerid
						ORDER BY EXTRACT(month FROM invoicedate), total_spent DESC
					  )
					  
	SELECT customerid,
	       month_no,
		   month,
		   total_spent
	FROM total_spends 
	WHERE rn = 1;

```

| customerid | month_no | month      | total_spent |
|------------|----------|-----------|-------------|
| 16816      | 1        | January   | 19.80       |
| 12682      | 2        | February  | 76.50       |
| 13001      | 3        | March     | 102.00      |
| 17735      | 4        | April     | 23.40       |
| 13450      | 5        | May       | 30.00       |
| 14329      | 6        | June      | 39.60       |
| 16657      | 7        | July      | 21.00       |
| 16722      | 8        | August    | 33.00       |
| 12520      | 9        | September | 39.00       |
| 17183      | 10       | October   | 102.00      |
| 14936      | 11       | November  | 47.60       |
| 13509      | 12       | December  | 70.80       |

-------------------------------------------------------------------------------------------------------------------------------------------

**day 28/50 days SQL Challenge**

Using the same dataset from the previous challenge day.

Write a query to find the highest-selling 
product for each customer

Return cx id, product description, 
and total count of purchase.

```sql

WITH total_purchases AS (SELECT customerid,
							 stockcode,
							 description,
							 SUM(quantity) AS total_purchased,
							 RANK()OVER(PARTITION BY customerid ORDER BY SUM(quantity)DESC) AS rank
						 FROM walmart_eu
						 WHERE customerid NOTNULL
						 GROUP BY customerid, stockcode, description
						 ORDER BY customerid, SUM(quantity)DESC
					 )
					 
SELECT customerid,
       stockcode,
	   description,
	   total_purchased 
  FROM total_purchases 
  WHERE rank = 1
  ORDER BY customerid
  LIMIT 5;

```

| customerid | stockcode | description                          | total_purchased |
|------------|-----------|--------------------------------------|-----------------|
| 12371      | 21625     | VINTAGE UNION JACK APRON             | 3               |
| 12520      | 22900     | SET 2 TEA TOWELS I LOVE LONDON       | 12              |
| 12631      | 23256     | CHILDRENS CUTLERY SPACEBOY           | 4               |
| 12682      | 22423     | REGENCY CAKESTAND 3 TIER             | 6               |
| 12748      | 23009     | I LOVE LONDON BABY GIFT SET          | 1               |


```sql

Find each country and best selling product 
Return country_name, description, total count of sale


WITH country_totals AS (SELECT stockcode,
							   description,
							   country,
							   SUM(quantity) AS total_sold,
							   RANK()OVER(PARTITION BY country ORDER BY SUM(quantity)DESC) AS rank
						   FROM walmart_eu
						   GROUP BY country, stockcode, description
						   ORDER BY country, SUM(quantity) DESC
					   )
					   
SELECT country,
       description,
	   total_sold
FROM country_totals
WHERE rank = 1 
ORDER BY country

```

| country          | description                           | total_sold |
|------------------|---------------------------------------|------------|
| Channel Islands  | LOVE HOT WATER BOTTLE                | 8          |
| Finland          | CHILDRENS CUTLERY SPACEBOY           | 4          |
| France           | REGENCY CAKESTAND 3 TIER             | 6          |
| Germany          | SET 2 TEA TOWELS I LOVE LONDON       | 12         |
| Hong Kong        | LUNCH BAG SPACEBOY DESIGN            | 80         |
| Japan            | MAGIC DRAWING SLATE CIRCUS PARADE    | 1          |
| Switzerland      | VINTAGE UNION JACK APRON             | 3          |
| United Kingdom   | FOOD/DRINK SPONGE STICKERS           | 100        |

-------------------------------------------------------------------------------------------------------------------------------------------
