# SQL Basics

- SELECT: Is used to select data from a database
~~~sql
SELECT * FROM Customers;

SELECT cus.CustomerName,cus.City, cus.PostalCode FROM Customers AS cus;
~~~

- SELECT DISTINCT: Is used to return only distinct (different) values
~~~sql
SELECT Country FROM Customers;
SELECT COUNT(Country) FROM Customers;

SELECT DISTINCT Country FROM Customers;
SELECT COUNT(DISTINCT Country) FROM Customers;

SELECT Count(*) AS DistinctCountries
FROM (SELECT DISTINCT Country FROM Customers);
~~~

- WHERE: Is used to filter records
~~~sql
SELECT * FROM Customers
WHERE Country='Mexico';

SELECT * FROM Customers
WHERE Country is 'Mexico';

SELECT * FROM Customers
WHERE Country is not 'Mexico';

SELECT * FROM Customers
WHERE CustomerName like '%Alfre%'

SELECT * FROM Customers
WHERE City IN ('Paris','London');

SELECT * FROM Products
WHERE Price BETWEEN 40 AND 60;
~~~

- AND, OR and NOT Operators
    - The AND operator displays a record if all the conditions separated by AND are TRUE
    - The OR operator displays a record if any of the conditions separated by OR is TRUE
~~~sql
SELECT * FROM Customers
WHERE Country='Germany' AND City='Berlin';

SELECT * FROM Customers
WHERE City='Berlin' OR City='München';

SELECT * FROM Customers
WHERE NOT Country='Germany';
~~~

- ORDER BY: Is used to sort the result-set in ascending or descending order
~~~sql
SELECT * FROM Customers
ORDER BY City DESC;

SELECT * FROM Customers
ORDER BY City;

SELECT * FROM Customers
ORDER BY ContactName,CustomerID;
~~~

- INSERT INTO: Is used to insert new records in a table
~~~sql
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');
~~~

- NULL values
~~~sql
SELECT CustomerName, ContactName, Address
FROM Customers
WHERE Address IS NULL;

SELECT CustomerName, ContactName, Address
FROM Customers
WHERE Address IS NOT NULL;
~~~

- UPDATE: Is used to modify the existing records in a table
~~~sql
UPDATE Customers
SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
WHERE CustomerID = 1;

UPDATE Customers
SET ContactName='Juan'
WHERE Country='Mexico';
~~~

- Be careful when updating records. If you omit the WHERE clause, ALL records will be updated!
~~~sql
UPDATE Customers
SET ContactName='Juan';
~~~

- DELETE: Is used to delete existing records in a table
~~~sql
DELETE FROM Customers WHERE CustomerName='Alfreds Futterkiste';

DELETE FROM Customers;
~~~
 
- SELECT TOP: Is used to specify the number of records to return
- Not all database systems support the SELECT TOP clause. MySQL supports the LIMIT clause to select a limited number of records, while Oracle uses FETCH FIRST n ROWS ONLY and ROWNUM
~~~sql
SELECT TOP 3 * FROM Customers;

SELECT * FROM Customers
LIMIT 3;

SELECT * FROM Customers
FETCH FIRST 3 ROWS ONLY;

SELECT TOP 3 * FROM Customers
WHERE Country='Germany';

SELECT * FROM Customers
WHERE Country='Germany'
LIMIT 3;

SELECT * FROM Customers
WHERE Country='Germany'
FETCH FIRST 3 ROWS ONLY;
~~~

- MIN() and MAX() 
    - The MIN() function returns the smallest value of the selected column
    - The MAX() function returns the largest value of the selected column
~~~sql
SELECT MIN(Price) AS SmallestPrice
FROM Products;

SELECT MAX(Price) AS LargestPrice
FROM Products;
~~~

- COUNT(), AVG() and SUM()
    - COUNT() function returns the number of rows that matches a specified criterion
    - AVG() function returns the average value of a numeric column
    - SUM() function returns the total sum of a numeric column
~~~sql
SELECT COUNT(ProductID)
FROM Products;

SELECT AVG(Price)
FROM Products;

SELECT SUM(Quantity)
FROM OrderDetails;
~~~

- LIKE: Is used in a WHERE clause to search for a specified pattern in a column
~~~sql
SELECT * FROM Customers
WHERE CustomerName LIKE 'b_o%s';

SELECT * FROM Customers
WHERE CustomerName LIKE '_r%';

SELECT * FROM Customers
WHERE CustomerName NOT LIKE 'a%';
~~~

- Wildcards
    - \*	Represents zero or more characters	
    - ?	    Represents a single character	
    - []	Represents any single character within the brackets	
    - !	    Represents any character not in the brackets	
    - \-	Represents any single character within the specified range	
    - \#	Represents any single numeric character
    
~~~sql
SELECT * FROM Customers
WHERE City LIKE 'ber%';

SELECT * FROM Customers
WHERE City LIKE '%es%';

SELECT * FROM Customers
WHERE City LIKE '[bsp]%';

SELECT * FROM Customers
WHERE City LIKE '[!bsp]%';

SELECT * FROM Customers
WHERE City NOT LIKE '[bsp]%';
~~~

- IN Operator 
    - Allows you to specify multiple values in a WHERE clause
    - Is a shorthand for multiple OR conditions
~~~sql
SELECT * FROM Customers
WHERE Country IN ('Germany', 'France', 'UK');

SELECT * FROM Customers
WHERE Country IN (SELECT Country FROM Suppliers);
~~~

- BETWEEN 
    - Selects values within a given range
~~~sql
SELECT * FROM Products
WHERE Price BETWEEN 10 AND 20;

SELECT * FROM Products
WHERE Price BETWEEN 10 AND 20
AND CategoryID NOT IN (1,2,3);

SELECT * FROM Products
WHERE ProductName BETWEEN 'Carnarvon Tigers' AND 'Mozzarella di Giovanni'
ORDER BY ProductName;

SELECT * FROM Orders
WHERE OrderDate BETWEEN '1996-07-01' AND '1996-07-31';
~~~

- Aliases: Are used to give a table, or a column in a table, a temporary name
~~~sql
SELECT CustomerID AS ID, CustomerName AS Customer
FROM Customers;

SELECT CustomerName AS Customer, ContactName AS [Contact Person]
FROM Customers;

SELECT CustomerName, Address + ', ' + PostalCode + ' ' + City + ', ' + Country AS Address
FROM Customers;

SELECT Orders.OrderID, Orders.OrderDate, Customers.CustomerName
FROM Customers, Orders
WHERE Customers.CustomerName='Around the Horn' AND Customers.CustomerID=Orders.CustomerID;

SELECT o.OrderID, o.OrderDate, c.CustomerName
FROM Customers AS c, Orders AS o
WHERE c.CustomerName='Around the Horn' AND c.CustomerID=o.CustomerID;
~~~

- Joins: Is used to combine rows from two or more tables, based on a related column between them
    - (INNER) JOIN: Returns records that have matching values in both tables
    - LEFT (OUTER) JOIN: Returns all records from the left table, and the matched records from the right table
    - RIGHT (OUTER) JOIN: Returns all records from the right table, and the matched records from the left table
    - FULL (OUTER) JOIN: Returns all records when there is a match in either left or right table
~~~sql
SELECT *
FROM Orders AS o
INNER JOIN Customers AS c
ON o.CustomerID=c.CustomerID;
~~~

- INNER JOIN: Selects records that have matching values in both tables
~~~sql
SELECT Orders.OrderID, Customers.CustomerName, Shippers.ShipperName
FROM ((Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID)
INNER JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID);
~~~

- LEFT JOIN: 
    - Returns all records from the left table (table1), and the matching records from the right table (table2) 
    - The result is 0 records from the right side, if there is no match
~~~sql
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
ORDER BY Customers.CustomerName;

SELECT *
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
WHERE Orders.ShipperID is not null
ORDER BY Customers.CustomerName;
~~~

- RIGHT JOIN:
    - Returns all records from the right table (table2), and the matching records from the left table (table1) 
    - The result is 0 records from the left side, if there is no match
~~~sql
SELECT Orders.EmployeeID ,Orders.OrderID, Employees.LastName, Employees.FirstName
FROM Orders
RIGHT JOIN Employees
ON Orders.EmployeeID = Employees.EmployeeID
WHERE Orders.EmployeeID is not null
ORDER BY Orders.OrderID;
~~~

- FULL OUTER JOIN:
    - Returns all records when there is a match in left (table1) or right (table2) table records
~~~sql
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;
~~~

- Self Join: Self join is a regular join, but the table is joined with itself.
~~~sql
SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID
AND A.City = B.City
ORDER BY A.City;

select * from
customer as c1, customer as c2
-- 36 records

select c1.id as customer_id, c1.name as customer_name, c2.name as supervisor_name from
customer as c1
LEFT JOIN
customer as c2
ON
c1.sup_id = c2.id
-- 6 records
~~~

- UNION 
    - Is used to combine the result-set of two or more SELECT statements
    - Every SELECT statement within UNION must have the same number of columns
    - The columns must also have similar data types
    - The columns in every SELECT statement must also be in the same order
    - The UNION operator selects only distinct values by default. To allow duplicate values, use UNION ALL

- The following SQL statement returns the cities (only distinct values) from both the "Customers" and the "Suppliers" table
~~~sql
SELECT City FROM Customers
UNION
SELECT City FROM Suppliers
ORDER BY City;
~~~

- The following SQL statement returns the cities (duplicate values also) from both the "Customers" and the "Suppliers" table
~~~sql
SELECT City FROM Customers
UNION ALL
SELECT City FROM Suppliers
ORDER BY City;
~~~

- So, here we have created a temporary column named "Type", that list whether the contact person is a "Customer" or a "Supplier"
~~~sql
SELECT 'Customer' AS Type, ContactName, City, Country
FROM Customers
UNION
SELECT 'Supplier', ContactName, City, Country
FROM Suppliers
~~~

- GROUP BY: Groups rows that have the same values into summary rows
~~~sql
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country;

SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
ORDER BY COUNT(CustomerID);

SELECT Shippers.ShipperName, COUNT(Orders.OrderID) AS NumberOfOrders FROM Orders
LEFT JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
GROUP BY ShipperName;
~~~

- HAVING: HAVING clause was added to SQL because the WHERE keyword cannot be used with aggregate functions
~~~sql
-- The following SQL statement lists the number of customers in each country. Only include countries with more than 5 customers
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) < 5;

-- The following SQL statement lists the number of customers in each country, sorted high to low (Only include countries with more than 5 customers)
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5
ORDER BY COUNT(CustomerID);

-- The following SQL statement lists the employees that have registered more than 10 orders
SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders
FROM (Orders
INNER JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID)
GROUP BY LastName
HAVING COUNT(Orders.OrderID) > 10;

-- The following SQL statement lists if the employees "Davolio" or "Fuller" have registered more than 25 orders
SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders
FROM Orders
INNER JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
WHERE LastName = 'Davolio' OR LastName = 'Fuller'
GROUP BY LastName
HAVING COUNT(Orders.OrderID) > 25;
~~~

- EXISTS
    - The EXISTS operator is used to test for the existence of any record in a subquery
    - The EXISTS operator returns TRUE if the subquery returns one or more records
~~~sql
-- The following SQL statement returns TRUE and lists the suppliers with a product price less than 20
SELECT SupplierName
FROM Suppliers
WHERE EXISTS (SELECT ProductName FROM Products WHERE Products.SupplierID = Suppliers.supplierID AND Price < 20);

-- The following SQL statement returns TRUE and lists the suppliers with a product price equal to 22
SELECT SupplierName
FROM Suppliers
WHERE EXISTS (SELECT ProductName FROM Products WHERE Products.SupplierID = Suppliers.supplierID AND Price = 22);

~~~
    
- SQL ANY
    - Returns a boolean value as a result
    - Returns TRUE if ANY of the sub query values meet the condition
~~~sql
SELECT ProductName
FROM Products
WHERE ProductID = ANY
  (SELECT ProductID
  FROM OrderDetails
  WHERE Quantity = 10);

SELECT ProductName
FROM Products
WHERE ProductID = ANY
  (SELECT ProductID
  FROM OrderDetails
  WHERE Quantity > 1000);
~~~
        
- SQL ALL
    - Returns a boolean value as a result
    - Returns TRUE if ALL of the sub query values meet the condition
    - Is used with SELECT, WHERE and HAVING statements
~~~sql
-- The following SQL statement lists the ProductName if ALL the records in the OrderDetails table has Quantity equal to 10. This will of course return FALSE because the Quantity column has many different values (not only the value of 10)
SELECT ProductName 
FROM Products
WHERE ProductID = ALL (SELECT ProductID FROM OrderDetails WHERE Quantity = 10);
~~~

- SELECT INTO: Statement copies data from one table into a new table
~~~sql
SELECT * INTO CustomersBackup
FROM Customers;

SELECT CustomerName, ContactName INTO CustomersBackup
FROM Customers;

SELECT Customers.CustomerName, Orders.OrderID
INTO CustomersOrderBackup2017
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID;

SELECT * INTO newtable
FROM oldtable
WHERE 1 = 0;
~~~

- INSERT INTO SELECT: 
    - Statement copies data from one table and inserts it into another table
    - Requires that the data types in source and target tables match

~~~sql
-- If the table is created with same schema then can insert values from source table to target table
INSERT INTO customer_bk
SELECT * FROM customer

-- The following SQL statement copies "Suppliers" into "Customers" (the columns that are not filled with data, will contain NULL)
INSERT INTO Customers (CustomerName, City, Country)
SELECT SupplierName, City, Country FROM Suppliers;

INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
SELECT SupplierName, ContactName, Address, City, PostalCode, Country FROM Suppliers;
~~~

- CASE Expressions
    - The CASE expression goes through conditions and returns a value when the first condition is met 
    - So, once a condition is true, it will stop reading and return the result
    - If no conditions are true, it returns the value in the ELSE clause
    - If there is no ELSE part and no conditions are true, it returns NULL
~~~sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;

SELECT  Quantity,
CASE 
WHEN Quantity = 10 THEN OrderID
ELSE 'Quantity not equals to 10'
END AS QuantityText
FROM OrderDetails;

SELECT CustomerName, City, Country FROM Customers
ORDER BY (CASE
WHEN City IS NULL THEN Country
ELSE City
END);
~~~

- IFNULL(): Function lets you return an alternative value if an expression is NULL
~~~sql
SELECT ProductName, UnitPrice * (UnitsInStock + IFNULL(UnitsOnOrder, 0))
FROM Products;
~~~

- ISNULL(): Function lets you return an alternative value when an expression is NULL
~~~sql
SELECT ProductName, UnitPrice * (UnitsInStock + ISNULL(UnitsOnOrder, 0))
FROM Products;
~~~