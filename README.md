--#1. Show Customers (full names, customerID, and Country) who are not in the US?

SELECT FirstName, LastName, Customerid, Country
FROM chinook.customers
WHERE Country != "US";

--#2. Show only the Customers from Brazil?

SELECT *
FROM chinook.customers
WHERE Country = "Brazil";

--#3.Find the Invoices of customers who are from Brazil?

SELECT C.FirstName, C.LastName, I.Invoiceid, I.InvoiceDate, I.BillingCountry 
FROM chinook.invoices AS I
LEFT JOIN chinook.customers AS C
ON C.customerId = I.customerID 
WHERE I.BillingCountry = "Brazil";

--#4. Employees who are Sales Agents?

SELECT FirstName, LastName, Title
FROM chinook.employees
WHERE Title LIKE "%Agent%";

--#5. Find a unique/distinct list of billing countries from the Invoice table?

SELECT DISTINCT BillingCountry
FROM chinook.invoices;

--#6.Provide a query that shows the invoices associated with each sales agent name?

SELECT E.FirstName, E.LastName, I.invoiceId
FROM chinook.employees AS E
JOIN chinook.customers AS C
ON C.supportRepId = E.employeeId
JOIN chinook.invoices AS I 
ON I.customerId = C.customerId;

--#7. Show the Invoice Total, Customer name, Country, and Sales Agent name for all invoices and customers?

SELECT E.FirstName, E.LastName, C.FirstName, C.LastName, C.Country, I.Total 
FROM chinook.employees AS E
JOIN chinook.customers AS C
ON C.supportRepId = E.employeeId
JOIN chinook.invoices AS I 
ON I.customerId = C.customerId;


--#8. How many Invoices were there in 2009?

SELECT COUNT(*) as TotalInvoice
FROM chinook.invoices
WHERE InvoiceDate BETWEEN '2009-01-01' AND '2009-12-31';

--#9. What are the total sales for 2009?

SELECT SUM(Total) AS TOTALSALES
FROM chinook.invoices
WHERE InvoiceDate BETWEEN '2009-01-01' AND '2009-12-31';

--#10. Write a query that includes the purchased track name with each invoice line ID?

SELECT T.name, I.InvoiceLineId
FROM chinook.invoice_items AS I
JOIN chinook.tracks AS T
ON I.TrackId = T.TrackId;

--#11. Write a query that includes the purchased track name AND artist name with each invoice line ID?

SELECT AR.name as Artist, T.Name as Track, I.InvoiceLineId
FROM chinook.Invoice_items I
LEFT JOIN chinook.tracks T 
ON I.TrackID=t.TrackID
INNER JOIN chinook.albums A
ON A.AlbumID=T.AlbumID
LEFT JOIN chinook.artists AS AR
ON AR.ArtistID=A.ArtistID;

--#12. Provide a query that shows all the Tracks, and include the Album name, Media type, and Genre?

SELECT T.Name AS 'Track Name', A.Title AS 'Album Title', M.Name AS 'Media Type', G.Name AS 'Genre'
FROM chinook.tracks T
JOIN chinook.Albums A 
on A.AlbumId = T.AlbumId
JOIN chinook.Media_Types M
on M.MediaTypeId = T.MediaTypeId
JOIN chinook.Genres G
on G.GenreId = T.GenreId;

--#13.Show the total sales made by each sales agent?

SELECT emp.FirstName, emp.LastName,
ROUND(SUM(Inv.Total), 2) as 'Total Sales' 
FROM chinook.Employees emp

JOIN chinook.Customers cust 
ON cust.SupportRepId = emp.EmployeeId

JOIN chinook.Invoices Inv 
ON Inv.CustomerId = cust.CustomerId

WHERE emp.Title = 'Sales Support Agent' 
GROUP BY emp.FirstName;

--#14. Which sales agent made the most dollars in sales in 2009?

SELECT E.FirstName, E.LastName,
ROUND(SUM(I.Total), 2) as 'Total Sales' 
FROM chinook.Employees E

JOIN chinook.Customers C 
ON C.SupportRepId = E.EmployeeId

JOIN chinook.Invoices I
ON I.CustomerId = C.CustomerId

WHERE E.Title = 'Sales Support Agent' 
AND I.InvoiceDate LIKE '2009%' 
GROUP BY E.FirstName
ORDER BY (round(sum(I.Total), 2))  DESC LIMIT 1;

--#15. How many users per country?

SELECT COUNT(C.CustomerId)
FROM chinook.customers AS C
ORDER BY 1;

--#16. how many songs per genre the music store has?

SELECT COUNT(TrackId), G.name
FROM chinook.tracks AS T
JOIN chinook.genres AS G
ON T.genreId = G.genreId
ORDER BY 2 DESC

--#17. Which city has the best customers? 

SELECT SUM(Total), BillingCity
FROM chinook.invoices AS I
ORDER BY 2 DESC ;

--#18. A query that shows the most purchased track of 2013?

SELECT*, COUNT(T.trackid) AS COUNT
FROM chinook.invoice_items AS IL
JOIN chinook.invoices as I 
ON I.invoiceid = IL.invoiceid
JOIN chinook.tracks AS T 
ON T.trackid = IL.trackid
where I.invoicedate between '2013-01-01' and '2013-12-31'
GROUP BY T.trackid
ORDER BY COUNT desc;


--#19. Which sales agent made the most in sales in 2010?

SELECT E.FirstName, E.LastName,
ROUND(SUM(I.Total), 2) as 'Total Sales' 
FROM chinook.Employees E

JOIN chinook.Customers C 
ON C.SupportRepId = E.EmployeeId

JOIN chinook.Invoices I
ON I.CustomerId = C.CustomerId

WHERE E.Title = 'Sales Support Agent' 
AND I.InvoiceDate LIKE '2010%' 
GROUP BY E.FirstName
ORDER BY (round(sum(I.Total), 2))  DESC LIMIT 1;

--#20. countries that have the most invoices/billing country?

SELECT COUNT(*) AS Number_Invoices, BillingCountry
FROM chinook.invoices 
GROUP BY BillingCountry
ORDER BY 2 DESC;


--#21. Write a query to return the email, name, and Genre of all Pop Music listeners?

SELECT C.email, C.FirstName, C.LastName, G.name 
FROM chinook.tracks AS T
JOIN chinook.invoice_items AS II ON II.trackId = T.trackId
JOIN chinook.invoices AS I ON II.InvoiceId = I.invoiceId
JOIN chinook.customers AS C ON c.customerId = I.customerId
JOIN chinook.genres AS G ON G.genreId = T.genreId
WHERE G.name IN ('Pop')
GROUP BY email; 
