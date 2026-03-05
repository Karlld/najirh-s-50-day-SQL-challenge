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
