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

