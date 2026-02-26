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

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


**Day 12/50**


```sql

DROP TABLE IF EXISTS Employees;
-- Create Employees table
CREATE TABLE Employees (
    id INT PRIMARY KEY,
    name VARCHAR(255)
);

-- Insert sample data into Employees table
INSERT INTO Employees (id, name) VALUES
    (1, 'Alice'),
    (7, 'Bob'),
    (11, 'Meir'),
    (90, 'Winston'),
    (3, 'Jonathan');


DROP TABLE IF EXISTS EmployeeUNI;
-- Create EmployeeUNI table
CREATE TABLE EmployeeUNI (
    id INT PRIMARY KEY,
    unique_id INT
);

-- Insert sample data into EmployeeUNI table
INSERT INTO EmployeeUNI (id, unique_id) VALUES
    (3, 1),
    (11, 2),
    (90, 3);

```


Write a solution to show the unique ID of each user, 
If a user does not have a unique ID replace just show null.

Return employee name and their unique_id.


```sql

SELECT * FROM Employees;

```


| id | name |
|----|------|
| 1	 | Alice |
| 7	 | Bob |
| 11 | Meir |
| 90 | Winston |
| 3	 | Jonathan |

```sql

SELECT * FROM EmployeeUNI;

```

| id | unique_id |
|----|-----------|
| 3 |	1 |
| 11 |	2 |
| 90 |	3 |

```sql

SELECT e.id,
       e.name,
	   COALESCE(uni.unique_id, 0) AS unique_id
   FROM employees AS e
   LEFT JOIN employeeUNI AS uni ON uni.id = e.id 
   ORDER BY id; 
```

| id | name | unique_id |
|----|------|-----------|
| 1	 |Alice |	0 |
| 3	 | Jonathan |	1 |
| 7	 | Bob |	0 |
| 11	| Meir |	2 |
| 90	| Winston |	3 |

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


 **Day 13/50**

```sql

DROP TABLE IF EXISTS employees;
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(100),
    manager_id INT,
    FOREIGN KEY (manager_id) REFERENCES employees(emp_id)
);

INSERT INTO employees (emp_id, name, manager_id) VALUES
(1, 'John Doe', NULL),        -- John Doe is not a manager
(2, 'Jane Smith', 1),          -- Jane Smith's manager is John Doe
(3, 'Alice Johnson', 1),       -- Alice Johnson's manager is John Doe
(4, 'Bob Brown', 3),           -- Bob Brown's manager is Alice Johnson
(5, 'Emily White', NULL),      -- Emily White is not a manager
(6, 'Michael Lee', 3),         -- Michael Lee's manager is Alice Johnson
(7, 'David Clark', NULL),      -- David Clark is not a manager
(8, 'Sarah Davis', 2),         -- Sarah Davis's manager is Jane Smith
(9, 'Kevin Wilson', 2),        -- Kevin Wilson's manager is Jane Smith
(10, 'Laura Martinez', 4);     -- Laura Martinez's manager is Bob Brown

```



You have a table named employees containing information about employees, 
including their emp_id, name, and manager_id. 
The manager_id refers to the emp_id of the employee's manager.


write a SQL query to retrieve all employees' 
details along with their manager's names based on the manager ID

```sql

SELECT * FROM employees;

```
| emp_id | name            | manager_id |
|--------|-----------------|------------|
| 1      | John Doe       |            |
| 2      | Jane Smith     | 1          |
| 3      | Alice Johnson  | 1          |
| 4      | Bob Brown      | 3          |
| 5      | Emily White    |            |
| 6      | Michael Lee    | 3          |
| 7      | David Clark    |            |
| 8      | Sarah Davis    | 2          |
| 9      | Kevin Wilson   | 2          |
| 10     | Laura Martinez | 4          |


```sql



SELECT e.emp_id,
       e.name,
	   e.manager_id,
	   m.name AS manager_name
FROM employees AS e
    LEFT JOIN employees AS m ON m.emp_id = e.manager_id;

``` 

| emp_id | name            | manager_id | manager_name   |
|--------|-----------------|------------|----------------|
| 1      | John Doe       |            |                |
| 2      | Jane Smith     | 1          | John Doe       |
| 3      | Alice Johnson  | 1          | John Doe       |
| 4      | Bob Brown      | 3          | Alice Johnson  |
| 5      | Emily White    |            |                |
| 6      | Michael Lee    | 3          | Alice Johnson  |
| 7      | David Clark    |            |                |
| 8      | Sarah Davis    | 2          | Jane Smith     |
| 9      | Kevin Wilson   | 2          | Jane Smith     |
| 10     | Laura Martinez | 4          | Bob Brown      |

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**SQL Challenge Day 14/50**

```sql

DROP TABLE IF EXISTS customers;
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    customer_email VARCHAR(100)
);

DROP TABLE IF EXISTS orders;
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    order_amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

INSERT INTO customers (customer_id, customer_name, customer_email) VALUES
(1, 'John Doe', 'john@example.com'),
(2, 'Jane Smith', 'jane@example.com'),
(3, 'Alice Johnson', 'alice@example.com'),
(4, 'Bob Brown', 'bob@example.com');

INSERT INTO orders (order_id, customer_id, order_date, order_amount) VALUES
(1, 1, '2024-01-03', 50.00),
(2, 2, '2024-01-05', 75.00),
(3, 1, '2024-01-10', 25.00),
(4, 3, '2024-01-15', 60.00),
(5, 2, '2024-01-20', 50.00),
(6, 1, '2024-02-01', 100.00),
(7, 2, '2024-02-05', 25.00),
(8, 3, '2024-02-10', 90.00),
(9, 1, '2024-02-15', 50.00),
(10, 2, '2024-02-20', 75.00);

```




You are given two tables: orders and customers. 
The orders table contains information about orders placed by customers, including the order_id, customer_id, order_date, and order_amount. 

The customers table contains information about customers, 
including the customer_id, customer_name, and customer_email.

-- Find the top 2 customers who have spent the most money across all their orders. 
Return their names, emails, and total amounts spent.

```sql

SELECT o.customer_id,
       c.customer_name,
	   c.customer_email,
       SUM(o.order_amount) AS total_amount
	FROM orders AS o 
	JOIN customers AS c ON c.customer_id = o.customer_id
	GROUP BY o.customer_id, c.customer_name, c.customer_email 
	ORDER BY total_amount DESC, customer_id 
	LIMIT 2;

```


| customer_id | customer_name | customer_email | total_amount |
|-------------|---------------|----------------|--------------|
| 1	 | John Doe | john@example.com |	225.00 |
| 2	 | Jane Smith | jane@example.com |	225.00 |

--------------------------------------------------------------------------------------------------------------------------------------------


**Day 16/50 SQL Challenge**


```sql

DROP TABLE IF EXISTS employees;
CREATE TABLE Employees (
    id INT PRIMARY KEY,
    name VARCHAR(255),
    department VARCHAR(255),
    managerId INT
);

INSERT INTO Employees (id, name, department, managerId) VALUES
(101, 'John', 'A', NULL),
(102, 'Dan', 'A', 101),
(103, 'James', 'A', 101),
(104, 'Amy', 'A', 101),
(105, 'Anne', 'A', 101),
(106, 'Ron', 'B', 101),
(107, 'Michael', 'C', NULL),
(108, 'Sarah', 'C', 107),
(109, 'Emily', 'C', 107),
(110, 'Brian', 'C', 107);


```

Given a table named employees with the following columns:
id, name, department, managerId

Write a SQL query to find the names of 
managers who have at least five direct reports. 
Return the result table in any order.

Ensure that no employee is their own manager.

The result format should include only the names
of the managers meeting the criteria.

```sql

SELECT * FROM employees;

```

| id  | name | department | managerid |
|-----|------|------------|-----------|
| 101	| John | A | |	
| 102	| Dan"	| A |	101 |
| 103	| James | A |	101 |
| 104	| Amy	| A |	101 |
| 105	| Anne | A | 101 |
| 106	| Ron | B |	101 |
| 107	| Michael | C |	|
| 108	 | Sarah | C |	107 |
| 109	| Emily  | C |	107 |
| 110	| Brian  | C |	107 |

```sql

SELECT m.name, 
       COUNT(e.managerid) AS employees
	FROM employees AS e
    LEFT JOIN employees AS m ON m.id = e.managerid 
    GROUP BY e.managerid, m.name
 HAVING COUNT(e.managerid) >= 5

```
  

| name | employees |
|------|-----------|
|  John 	| 5 |

--------------------------------------------------------------------------------------------------------------------------------------------

**Day 17/50**

```sql

DROP TABLE IF EXISTS customers;
-- Creating the Customers table
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(50)
);


DROP TABLE IF EXISTS purchases;
-- Creating the Purchases table
CREATE TABLE Purchases (
    purchase_id INT PRIMARY KEY,
    customer_id INT,
    product_category VARCHAR(50),
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);

-- Inserting sample data into Customers table
INSERT INTO Customers (customer_id, customer_name) VALUES
    (1, 'Alice'),
    (2, 'Bob'),
    (3, 'Charlie'),
    (4, 'David'),
    (5, 'Emma');

-- Inserting sample data into Purchases table
INSERT INTO Purchases (purchase_id, customer_id, product_category) VALUES
    (101, 1, 'Electronics'),
    (102, 1, 'Books'),
    (103, 1, 'Clothing'),
    (104, 1, 'Electronics'),
    (105, 2, 'Clothing'),
    (106, 1, 'Beauty'),
    (107, 3, 'Electronics'),
    (108, 3, 'Books'),
    (109, 4, 'Books'),
    (110, 4, 'Clothing'),
    (111, 4, 'Beauty'),
    (112, 5, 'Electronics'),
    (113, 5, 'Books');

```

Question:
Write an SQL query to find customers who have made 
purchases in all product categories.

Tables:
Customers: customer_id (INT), customer_name (VARCHAR)

Purchases: purchase_id (INT), customer_id (INT), 
product_category (VARCHAR)

Your query should return the customer_id and 
customer_name of these customers.


```sql

SELECT * FROM customers;

```

| customer_id | customer_name |
|-------------|---------------|
| 1	 | Alice | 
| 2	 | Bob | 
| 3	 | Charlie | 
| 4	 | David |
| 5	 | Emma |

```sql

SELECT * FROM purchases;

```

 | purchase_id | customer_id | product_category |
 |-------------|-------------|------------------|
| 101	| 1	| Electronics| 
| 102	| 1	| Books |
| 103	| 1	| Clothing |  
| 104	| 1	| Electronics |
| 105	| 2	| Clothing |
| 106	| 1	| Beauty |
| 107	| 3	| Electronics |
| 108	| 3	| Books |
| 109	| 4	 | Books |
| 110	| 4	| Clothing |
| 111	| 4	| Beauty |
| 112	| 5	| Electronics |
| 113	| 5	| Books |

```sql

WITH purch_cats AS (SELECT p.customer_id,
						   c.customer_name,
						   COUNT(DISTINCT(p.product_category)) AS purchase_cat
					  FROM purchases AS p
					  LEFT JOIN customers AS c ON c.customer_id = p.customer_id
					  GROUP BY p.customer_id, c.customer_name
				   )
				   
SELECT customer_id,
       customer_name
  FROM purch_cats  
  WHERE purchase_cat = (SELECT MAX(purchase_cat) 
						   FROM purch_cats);


```

| customer_id | customer_name |
|-------------|---------------|
| 1	 | Alice |

---------------------------------------------------------------------------------------------------------------------------------------------


