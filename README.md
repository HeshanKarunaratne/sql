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