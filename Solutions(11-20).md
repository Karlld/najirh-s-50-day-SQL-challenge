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

