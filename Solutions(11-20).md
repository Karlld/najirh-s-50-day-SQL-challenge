**Day 11/50**

```sql

DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS returns;



CREATE TABLE orders (
    order_id VARCHAR(10),
    customer_id VARCHAR(10),
    order_date DATE,
    product_id VARCHAR(10),
    quantity INT
);


CREATE TABLE returns (
    return_id VARCHAR(10),
    order_id VARCHAR(10)
    );

INSERT INTO orders (order_id, customer_id, order_date, product_id, quantity)
VALUES
    ('1001', 'C001', '2023-01-15', 'P001', 4),
    ('1002', 'C001', '2023-02-20', 'P002', 3),
    ('1003', 'C002', '2023-03-10', 'P003', 8),
    ('1004', 'C003', '2023-04-05', 'P004', 2),
    ('1005', 'C004', '2023-05-20', 'P005', 3),
    ('1006', 'C002', '2023-06-15', 'P001', 6),
    ('1007', 'C003', '2023-07-20', 'P002', 1),
    ('1008', 'C004', '2023-08-10', 'P003', 2),
    ('1009', 'C005', '2023-09-05', 'P002', 3),
    ('1010', 'C001', '2023-10-20', 'P002', 1);


INSERT INTO returns (return_id, order_id)
VALUES
    ('R001', '1001'),
    ('R002', '1002'),
    ('R003', '1005'),
    ('R004', '1008'),
    ('R005', '1007');
```


Identify returning customers based on their order history. 
Categorize customers as "Returning" if they have placed more than one return, 
and as "New" otherwise. 

Considering you have two table orders has information about sale
and returns has information about returns 


```sql

SELECT * FROM orders;

```

| order_id | customer_id | order_date  | product_id | quantity |
|----------|-------------|------------|-----------|---------|
| 1001     | C001        | 2023-01-15 | P001      | 4       |
| 1002     | C001        | 2023-02-20 | P002      | 3       |
| 1003     | C002        | 2023-03-10 | P003      | 8       |
| 1004     | C003        | 2023-04-05 | P004      | 2       |
| 1005     | C004        | 2023-05-20 | P005      | 3       |
| 1006     | C002        | 2023-06-15 | P001      | 6       |
| 1007     | C003        | 2023-07-20 | P002      | 1       |
| 1008     | C004        | 2023-08-10 | P003      | 2       |
| 1009     | C005        | 2023-09-05 | P002      | 3       |
| 1010     | C001        | 2023-10-20 | P002      | 1       |



```sql

SELECT * FROM returns;

```

| return_id | order_id |
|-----------|----------|
| R001 | 1001 |
| R002 | 1002 |
| R003 | 1005 |
| R004 | 1008 |
| R005 | 1007 |

```sql

WITH cust_returned AS (SELECT o.*,
							   r.return_id,
							   CASE WHEN return_id ISNULL THEN 0
									 ELSE 1 
									 END AS returned
					          
					     FROM orders AS o
         				 LEFT JOIN returns AS r ON r.order_id = o.order_id
					  )
					  
	SELECT order_id,
	       customer_id,
		   order_date,
		   product_id,
		   quantity,
		   CASE WHEN returned > 0 THEN 'Returning'
		        ELSE 'New'
				END AS returning_customer
	FROM cust_returned
	ORDER BY order_id;

```


| order_id | customer_id | order_date  | product_id | quantity | returning_customer |
|----------|-------------|------------|-----------|---------|------------------|
| 1001     | C001        | 2023-01-15 | P001      | 4       | Returning        |
| 1002     | C001        | 2023-02-20 | P002      | 3       | Returning        |
| 1003     | C002        | 2023-03-10 | P003      | 8       | New              |
| 1004     | C003        | 2023-04-05 | P004      | 2       | New              |
| 1005     | C004        | 2023-05-20 | P005      | 3       | Returning        |
| 1006     | C002        | 2023-06-15 | P001      | 6       | New              |
| 1007     | C003        | 2023-07-20 | P002      | 1       | Returning        |
| 1008     | C004        | 2023-08-10 | P003      | 2       | Returning        |
| 1009     | C005        | 2023-09-05 | P002      | 3       | New              |
| 1010     | C001        | 2023-10-20 | P002      | 1       | New              |

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
