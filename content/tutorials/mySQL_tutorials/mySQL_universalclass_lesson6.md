---
title: "MySQL Universalclass Lesson6"
linktitle: "MySQL Universalclass Lesson6"
summary:
date: 2020-07-15T16:36:06-04:00
lastmod: 2020-07-15T16:36:06-04:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 60
menu:
  mySQL_tutorials:
    name: "Lesson6 adv queries"
    parent: "MySQL Universalclass"
    weight: 2020071517
---

[https://www.youtube.com/watch?v=es_t9QXpXY4&list=PLBlpUqEneF0-xZ1ctyLVqhwJyoQsyfOsO&index=6](https://www.youtube.com/watch?v=es_t9QXpXY4&list=PLBlpUqEneF0-xZ1ctyLVqhwJyoQsyfOsO&index=6)

<br>

## Customers, Orders example
- 123 Customers
- 326 Orders

<br>

***
## JOINS

<br>

### natural joins
- { old way of INNER join }
- for each customer, want to see all orders they placed
- expect to see 326 rows return for all orders,
    - and have both **customers** and **orders** table's columns in it

```sql
SELECT *
FROM customers, orders
WHERE customers.customernumber = orders.customernumber;
```  

- have to use fully qualified table names
- { since orders have to have a customer, this NATURAL/INNER join is same as LEFT join (with Orders first) }

<br>

### only show certain columns
- and change column name to be displayed with `AS`

```sql
SELECT orders.ordernumber AS 'Order Number', customers.customername 
AS 'Customer Name', orders.orderdate AS 'Order Date', orders.shippeddate AS 'Shipped Date'
FROM customers, orders
WHERE customers.customernumber = orders.customernumber;
```  

<br>

### using alias
- specify alias in FROM statement
- change all other table name to alias

```sql
SELECT o.ordernumber AS 'Order Number', 
	   c.customername AS 'Customer Name', 
       o.orderdate AS 'Order Date', 
       o.shippeddate AS 'Shipped Date'
FROM customers c, orders o
WHERE c.customernumber = o.customernumber;
```

<br>

### SELF JOIN
- { why??? }
- relate a table back onto itself
- e.g. have list of employees and some employees are managers 
    - want to see list of all employees,
    - along with name of his/her manager, 
    - query will have to look back into same employee table
    - { i.e. have to generate 2 employee cols,
        - relating employee-to-manager }
- solution = give table alias name
- { but below example doesn't really relate employee-to-manager...? }

```sql
SELECT a.customername, b.customername
-- FROM customers, customers; 	# not allowed
FROM customers a, customers b	
-- can give different alias to same table
WHERE a.customername = b.customername;
```

<br>

### EXPLICIT JOINS  
```
  INNER JOIN      only records where id matches btw table1 and table2
  OUTER JOIN      retain all records (if tables not same size, will have NULLs)
  LEFT JOIN       retains all records from first table
  RIGHT JOIN      retains all records from second table
```  

- better than SELF JOIN
- replaces WHERE a.customername = b.customername
    - with JOIN ... ON ...

<br>

#### INNER JOIN

```sql
SELECT a.customername, b.customername
FROM customers a
INNER JOIN customers b				-- INNER optional
ON a.customername = b.customername;
```  

- JOIN ON clause more efficient than FROM ... WHERE
- FROM ... WHERE has to create a huge table of
    - all combinations of a and b tables,
    - then narrow it down to rows where a.customername = b.customername
- JOIN ON clause only write to new table if
	  - a.customername = b.customername

<br>

#### LEFT JOIN
- want customername in col1, and all their orders in col2
- i.e. want everything from left table (i.e. "orders" table)

```sql
SELECT r.customername, l.ordernumber, l.orderdate   -- get all columns
FROM orders l                                       -- alias 'l' for left table
JOIN customers r                                    -- alias 'r' for right table
ON l.customernumber = r.customernumber              -- filter rows according to ON 
-- (customer has to have placed an order to show up)
ORDER BY r.customername;
```

<br>

#### RIGHT JOIN
- want everything from right table (i.e. "customers" table)

```sql
SELECT r.customername, l.ordernumber, l.orderdate
FROM orders l
RIGHT JOIN customers r					- 351 rows / e.g. American Souvenir - Null ordernumber
ON l.customernumber = r.customernumber
ORDER BY r.customername;
```

- { LEFT JOIN 
    - same result as INNER JOIN since every order has a corresponding customer }
- { RIGHT JOIN 
    - diff result from INNER JOIN since not every customer placed an order }

<br>

***
## AGGREGATE function
- count, sum, average

```sql
SELECT COUNT(*)
FROM customers;		-- output = 123

SELECT COUNT(*) AS 'Number of Customers'
FROM customers;		-- output = 123

SELECT COUNT(*) AS 'Number of Order Items'
FROM orderdetails;	-- output = 2996

SELECT * FROM orderdetails;
-- composite key with ordernumber + productcode

-- for ordernumber 10100, how many items were ordered?
SELECT ordernumber, SUM(quantityordered)
FROM orderdetails
WHERE ordernumber = 10100;
```

<br>

***
## literals
- force a literal on a column

```sql
-- get num of items ordered for ordernumber 10100
SELECT ordernumber, 'Total:', SUM(quantityordered)
FROM orderdetails
WHERE ordernumber = 10100;

-- do for all customers, using GROUP BY
SELECT ordernumber, 'Total:', SUM(quantityordered) AS 'Num item ordered'
FROM orderdetails
GROUP BY ordernumber;      -- want to compute SELECT ... SUM() for each ordernumber

-- which ordernumber has most num items ordered?
SELECT ordernumber, 'Total:', SUM(quantityordered) AS 'Num item ordered'
FROM orderdetails
GROUP BY ordernumber
ORDER BY SUM(quantityordered) DESC;

-- get max value in a column {NOT grouped by order number}
SELECT MAX(quantityordered)		-- MIN() for minimum
FROM orderdetails;

-- calculations in a separate column
SELECT *, quantityordered * priceeach AS 'total'
from orderdetails;

-- for each "product", how many were ordered
SELECT productcode, count(quantityordered), sum(priceeach)
FROM orderdetails
GROUP BY productcode;

-- for each "product", sum of price
SELECT productcode, count(quantityordered), sum(priceeach)
FROM orderdetails
GROUP BY productcode;

-- find total number of item ordered for each "order", total price for each "order"
-- list order number, 
-- GROUP by order number -- the sum of units ordered {doesn't matter productcode...}, 
-- GROUP by order number -- the sum of price num of units * price each unit
SELECT ordernumber, sum(quantityordered), sum(quantityordered * priceeach) AS 'total'
FROM orderdetails
GROUP BY ordernumber;
```

<br>

***
## subqueries (nested queries)
- perform a query to get a value,
- use that value to perform another query

<br>

### example
- get all of people who placed an order,
- where the number of items on their order > average number of items customers ordered

```sql
SELECT AVG(quantityordered)
FROM orderdetails;

SELECT ordernumber, productcode, quantityordered
FROM orderdetails
WHERE quantityordered > 35.1572    -- replace with SELECT AVG()...
ORDER BY ordernumber;

SELECT ordernumber, productcode, quantityordered
FROM orderdetails
WHERE quantityordered >
	(
    SELECT AVG(quantityordered)
    FROM orderdetails      -- you don't need to change DELIMITER???
	)
ORDER BY ordernumber;

-- for each customer, how much they paid (need GROUP BY)
SELECT customernumber, SUM(amount)
FROM payments
GROUP BY customernumber;

-- find customers who paid more than the average
SELECT customernumber, SUM(amount)
FROM payments
WHERE amount >
(						-- mysql doesn't care about indent...
SELECT AVG(amount)
FROM payments
)
GROUP BY customernumber;
```