# SQL-Practice
Just For Fun
/*Resolve below queries using OLTP

 1-From the following table write a query in SQL to retrieve the total sales for each year. Filter the result set for those orders where order year
is on or before 2016. Return the year part of orderdate and total due amount. Sort the result in ascending order on year part of order date.

Sample table: Sales.SalesOrderHeader*/
SELECT DATEPART(Year, OrderDate ) AS OrderYRPart, TotalDue FROM [Sales].[SalesOrderHeader]
WHERE DATEPART(Year, OrderDate ) <= 2016

/*2-From the following table write a query in SQL to remove the substring 'HN' from the start of the column productnumber. 
Filter the results to only show those productnumbers that start with "HN". Return original productnumber column and 'TrimmedProductnumber'.
Sample table: production.Product*/
SELECT ProductNumber, TRIM(REPLACE(ProductNumber, 'HN', ' ')) AS TrimmedProductnumber FROM [Production].[Product]
WHERE SUBSTRING(ProductNumber,1,2) = 'HN'


/*3- From the following table write a SQL query to locate the position of the string "yellow" where it appears in the product name.
Sample table: production.product*/
SELECT * FROM production.product
SELECT Name, CHARINDEX('Yellow', Name)+1 AS "String Position" FROM [Production].[Product]
WHERE CHARINDEX('yellow', Name) > 0



/*4- From the following table write a query in SQL to return all category descriptions containing strings with prefixes of either chain or full.
Sample table: Production.Product*/
SELECT * FROM [Production].[Product]
WHERE Name LIKE 'Chain%' OR Name LIKE 'Full%'


/*3-From Production.Product, Write a query that returns the last four digits of each productNumber starting with 'CA' delineation*/

SELECT ProductNumber, SUBSTRING(ProductNumber,CHARINDEX('-', ProductNumber)+1,4) AS LastFour FROM [Production].[Product]
WHERE ProductNumber LIKE 'CA%'

SELECT ProductNumber, RIGHT(ProductNumber,4) AS LastFour FROM [Production].[Product]
WHERE ProductNumber LIKE 'CA%'

/*4- From Production.Product, Write a query that returns the last four digits of each product starting with 'HJ' delineation. 
Return also the year, month and day part of those products.*/
SELECT * FROM [Production].[Product]

SELECT ProductNumber, RIGHT(ProductNumber,4) AS LastFour, DATEPART(Year, ModifiedDate ) AS ModYR, 
DATEPART(Month, ModifiedDate ) AS ModMonth, DATEPART(Day, ModifiedDate ) AS ModDay  FROM [Production].[Product]
WHERE ProductNumber LIKE 'HJ%'

SELECT ProductNumber, RIGHT(ProductNumber,4) AS LastFour, DATEPART(Year, SellEndDate) AS EndYR, 
DATEPART(Month, SellEndDate) AS EndMonth, DATEPART(Day, SellEndDate) AS EndDay  FROM [Production].[Product]
WHERE ProductNumber LIKE 'HJ%'

SELECT ProductNumber, RIGHT(ProductNumber,4) AS LastFour, DATEPART(Year, SellStartDate) AS StartYR, 
DATEPART(Month, SellStartDate) AS StartMonth, DATEPART(Day, SellStartDate) AS StartDay  FROM [Production].[Product]
WHERE ProductNumber LIKE 'HJ%'

/*5- Write a query that returns all the products that are no longer on the market for sale, as well as the days that they spent on the market. 
What's the max time spent, and the least time spent on the market?
----Which product lasted longer among them? Which one  lasted the least?-----*/
SELECT TOP 5* FROM [Production].[Product]
WHERE SellEndDate IS NOT NULL

SELECT Name, DATEDIFF(Day, SellStartDate, SellEndDate) AS DaysOnMarket FROM [Production].[Product]
WHERE SellEndDate IS NOT NULL
ORDER BY DaysOnMarket DESC


WITH CTE(Name, DaysOnMarket) AS 
(SELECT Name, DATEDIFF(Day, SellStartDate, SellEndDate) AS DaysOnMarket FROM [Production].[Product]
WHERE SellEndDate IS NOT NULL)
SELECT Name/*, Max(DaysOnMarket) AS Longest, Min(DaysOnMarket) AS Shortest  */FROM CTE
GROUP BY Name
HAVING Max(DaysOnMarket) = 729

