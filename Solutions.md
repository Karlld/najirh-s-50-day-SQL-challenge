**Day 02/50**



Given the Orders table with columns OrderID, 
OrderDate, and TotalAmount, and the 
Returns table with columns ReturnID and OrderID, 

write an SQL query to calculate the total 
numbers of returned orders for each month

```sql

DROP TABLE IF EXISTS Orders;
	
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    OrderDate DATE,
    TotalAmount DECIMAL(10, 2)
);

DROP TABLE IF EXISTS Returns;
CREATE TABLE Returns (
    ReturnID INT PRIMARY KEY,
    OrderID INT,
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);

INSERT INTO Orders (OrderID, OrderDate, TotalAmount) VALUES
(1, '2023-01-15', 150.50),
(2, '2023-02-20', 200.75),
(3, '2023-02-28', 300.25),
(4, '2023-03-10', 180.00),
(5, '2023-04-05', 250.80);

INSERT INTO Returns (ReturnID, OrderID) VALUES
(101, 2),
(102, 4),
(103, 5),
(104, 1),
(105, 3);

```


```sql

 SELECT * FROM orders;

```


| orderid | orderdate | totalamount |
|---------|-----------|-------------|
| 1	 | 2023-01-15 |	150.50 |
| 2	| 2023-02-20 |	200.75 |
| 3	| 2023-02-28 |	300.25 |
| 4	| 2023-03-10 | 180.00 |
| 5	| 2023-04-05 |	250.80 |


```sql

SELECT * FROM returns;

```

| returnid | orderid |
|----------|---------|
| 101 |	2 |
| 102 | 4 |
| 103 |	5 |
| 104 |	1 |
| 105 |	3 |


```sql

SELECT order_month,
       no_of_returns 
	FROM (SELECT EXTRACT(MONTH FROM o.orderdate) AS month_no,
								 TO_CHAR(TO_DATE(EXTRACT(MONTH FROM o.orderdate)::TEXT,'MM'), 'MONTH') AS order_month,
								 COUNT(r.returnid) AS no_of_returns
							FROM orders AS o
							LEFT JOIN returns AS r ON r.orderid = o.orderid
							GROUP BY month_no, order_month
							ORDER BY month_no 
						  )

```
	     


| order_month | no_of_returns |
|-------------|---------------|
| JANUARY  |	1 |
| FEBRUARY |	2 |
| MARCH    |	1 |
| APRIL    |	1 |





--------------------------------------------------------------------------------------------------------------------------------------------

**Day 04/50** 


Find the top 2 products in the top 2 categories based on spend amount?


```sql

create table orders(
  	category varchar(20),
	product varchar(20),
	user_id int , 
  	spend int,
  	transaction_date DATE
);

Insert into orders values
('appliance','refrigerator',165,246.00,'2021/12/26'),
('appliance','refrigerator',123,299.99,'2022/03/02'),
('appliance','washingmachine',123,219.80,'2022/03/02'),
('electronics','vacuum',178,152.00,'2022/04/05'),
('electronics','wirelessheadset',156,	249.90,'2022/07/08'),
('electronics','TV',145,189.00,'2022/07/15'),
('Television','TV',165,129.00,'2022/07/15'),
('Television','TV',163,129.00,'2022/07/15'),
('Television','TV',141,129.00,'2022/07/15'),
('toys','Ben10',145,189.00,'2022/07/15'),
('toys','Ben10',145,189.00,'2022/07/15'),
('toys','yoyo',165,129.00,'2022/07/15'),
('toys','yoyo',163,129.00,'2022/07/15'),
('toys','yoyo',141,129.00,'2022/07/15'),
('toys','yoyo',145,189.00,'2022/07/15'),
('electronics','vacuum',145,189.00,'2022/07/15');



SELECT * FROM orders;

```

| category      | product          | user_id | spend | transaction_date |
|---------------|------------------|---------|-------|------------------|
| appliance     | refrigerator     | 165     | 246   | 2021-12-26       |
| appliance     | refrigerator     | 123     | 300   | 2022-03-02       |
| appliance     | washingmachine   | 123     | 220   | 2022-03-02       |
| electronics   | vacuum           | 178     | 152   | 2022-04-05       |
| electronics   | wirelessheadset  | 156     | 250   | 2022-07-08       |
| electronics   | TV               | 145     | 189   | 2022-07-15       |
| Television    | TV               | 165     | 129   | 2022-07-15       |
| Television    | TV               | 163     | 129   | 2022-07-15       |
| Television    | TV               | 141     | 129   | 2022-07-15       |
| toys          | Ben10            | 145     | 189   | 2022-07-15       |
| toys          | Ben10            | 145     | 189   | 2022-07-15       |
| toys          | yoyo             | 165     | 129   | 2022-07-15       |
| toys          | yoyo             | 163     | 129   | 2022-07-15       |
| toys          | yoyo             | 141     | 129   | 2022-07-15       |
| toys          | yoyo             | 145     | 189   | 2022-07-15       |
| electronics   | vacuum           | 145     | 189   | 2022-07-15       |


```sql

WITH top_category AS( SELECT category,
					         SUM(spend) AS total_spent
					 FROM orders
					 GROUP BY category 
					 ORDER BY total_spent DESC
					 LIMIT 2),
	
top_product AS (SELECT category,
       			product,
	   			SUM(spend) AS total_spent,
	   			DENSE_RANK() OVER (PARTITION BY category) AS rn
			FROM orders
			GROUP BY category, product
			ORDER BY category, total_spent DESC
		       ),
	  
top_cat_prod AS (SELECT tp.category,
				  tp.product,
				  tp.total_spent,
				  ROW_NUMBER() OVER (PARTITION BY tp.category) AS rn
				FROM top_product AS tp
				RIGHT JOIN top_category AS tc ON tp.category = tc.category
				  )
	SELECT category,
	       product,
		   total_spent
		FROM top_cat_prod
		WHERE rn <= 2

```


| category | product | total_spent |
|----------|---------|-------------|
| electronics | vacuum | 341 |
| electronics | wirelessheadset |	250 |
| toys | yoyo | 576 |
| toys | Ben10 | 378 |


