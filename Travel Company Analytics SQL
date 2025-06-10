SELECT * FROM customers;
SELECT * FROM bookings;


-- Using CTEs and subqueries

-- 1. Using CTE for Booking Status Counts
WITH booking_counts AS (
    SELECT 
        Booking_Status,
        COUNT(*) AS Status_Count
    FROM bookings
    GROUP BY Booking_Status
)
SELECT * FROM booking_counts;

--  2. Subquery: Most Common Booking Month
SELECT Month, Count FROM (
    SELECT 
        MONTH(Booking_Date) AS Month, 
        COUNT(*) AS Count
    FROM bookings 
    GROUP BY MONTH(Booking_Date)
    ORDER BY Count DESC
    LIMIT 5
) AS Most_Common_Month;

--  3. Using CTE for Customer Lifetime Value
WITH customer_value AS (
    SELECT 
        b.Customer_ID,
        c.First_Name,
        c.Last_Name,
        SUM(b.Service_Cost) AS Customer_Lifetime_Value
    FROM bookings b
    JOIN customers c ON b.Customer_ID = c.Customer_ID
    GROUP BY b.Customer_ID, c.First_Name, c.Last_Name
)
SELECT * FROM customer_value
ORDER BY Customer_Lifetime_Value DESC;

--  4. Using Subquery to calculate Revenue by Booking Status
SELECT 
    Booking_Status,
    SUM(Service_Cost) AS Revenue,
    ROUND(
        SUM(Service_Cost) / (SELECT SUM(Service_Cost) FROM bookings), 2
    ) AS Revenue_Percentage
FROM bookings
GROUP BY Booking_Status;

--  5. Using CTE for Top Revenue Locations
WITH top_locations AS (
    SELECT 
        Location AS Destination,
        SUM(Service_Cost) AS Revenue
    FROM bookings
    WHERE Booking_Status = 'Confirmed'
    GROUP BY Location
)
SELECT * FROM top_locations
ORDER BY Revenue DESC
LIMIT 5;

--  6. Using CTE to simplify Monthly Revenue Trend
WITH monthly_revenue AS (
    SELECT 
        DATE_FORMAT(Booking_Date, '%Y-%m') AS Month,
        SUM(Service_Cost) AS Revenue
    FROM bookings
    WHERE Booking_Status = 'Confirmed'
    GROUP BY DATE_FORMAT(Booking_Date, '%Y-%m')
)
SELECT * FROM monthly_revenue
ORDER BY Month;

--  7. Using CTE for Most Profitable Days
WITH daily_revenue AS (
    SELECT 
        Booking_Date,
        SUM(Service_Cost) AS Revenue
    FROM bookings
    WHERE Booking_Status = 'Confirmed'
    GROUP BY Booking_Date
)
SELECT * FROM daily_revenue
ORDER BY Revenue DESC
LIMIT 5;

-- Total Customers -- 
SELECT COUNT(*) AS Total_Customers
FROM customers;

SELECT COUNT(Booking_Status) AS `Cancelled Booking`
FROM bookings
WHERE Booking_Status = 'Cancelled';

SELECT COUNT(Booking_Status) AS `Confirmed Booking`
FROM bookings
WHERE Booking_Status = 'Confirmed';

SELECT COUNT(Booking_Status) AS `Pending Booking`
FROM bookings
WHERE Booking_Status = 'Pending';

SELECT COUNT(Booking_Status) AS `Total Bookings`
FROM bookings;

SELECT SUM(Service_Cost) AS `Cost of Confirmed Booking`
FROM bookings
WHERE Booking_Status = 'Confirmed';

-- Most Common Booking Month
SELECT MONTH(Booking_Date) AS Month, COUNT(*) AS Count FROM bookings GROUP BY MONTH(Booking_Date) ORDER BY Count DESC LIMIT 1;

-- Month on Month Revenue Growth
SELECT
  YEAR(Booking_Date) AS Year,
  MONTH(Booking_Date) AS Month,
  SUM(Service_Cost) AS Revenue,
  LAG(SUM(Service_Cost)) OVER (ORDER BY YEAR(Booking_Date), MONTH(Booking_Date)) AS Previous_Month_Revenue,
  ROUND((SUM(Service_Cost) - LAG(SUM(Service_Cost)) OVER (ORDER BY YEAR(Booking_Date), MONTH(Booking_Date))) * 100.0 / NULLIF(LAG(SUM(Service_Cost)) OVER (ORDER BY YEAR(Booking_Date), MONTH(Booking_Date)), 0), 2) AS MoM_Growth
FROM bookings GROUP BY YEAR(Booking_Date), MONTH(Booking_Date);

-- Trip Length by Season
SELECT CASE 
  WHEN MONTH(Departure_Date) IN (12,1,2) THEN 'Winter'
  WHEN MONTH(Departure_Date) IN (3,4,5) THEN 'Spring'
  WHEN MONTH(Departure_Date) IN (6,7,8) THEN 'Summer'
  ELSE 'Autumn'
END AS Season, AVG(DATEDIFF(Arrival_Date, Departure_Date)) AS Avg_Trip_Length 
FROM bookings 
GROUP BY Season;

-- Best travel Month
SELECT 
  CASE 
    WHEN MONTH(Departure_Date) IN (12, 1, 2) THEN 'Winter'
    WHEN MONTH(Departure_Date) IN (3, 4, 5) THEN 'Spring'
    WHEN MONTH(Departure_Date) IN (6, 7, 8) THEN 'Summer'
    ELSE 'Autumn'
  END AS Season,
  COUNT(Booking_Status) AS Total_Trips
FROM bookings
WHERE Departure_Date IS NOT NULL AND Booking_Status = 'Confirmed'
GROUP BY Season;

-- Top 6 Destination -- 
SELECT 
    Location AS Destionation, 
    COUNT(*) AS `Booking Count`,
    SUM(Service_Cost) AS `Total Revenue`,
    AVG(DATEDIFF(Arrival_Date, Departure_Date)) AS `Avg. Days Spent`
FROM 
    bookings
WHERE Booking_Status = 'Confirmed'  
GROUP BY 
    Location
ORDER BY 
    `Booking Count` DESC
LIMIT 6;


SELECT 
    Location AS Destionation, 
    SUM(Service_Cost) AS `Total Revenue`
FROM bookings
WHERE Booking_Status = 'Confirmed'  
GROUP BY 
    Location
ORDER BY 
    Destionation ASC
LIMIT 5;

-- Booking Over Time
SELECT 
    DATE_FORMAT(Booking_Date, '%Y-%m') AS YearMonth,
    COUNT(*) AS `Booking Over Time`
FROM 
    bookings
GROUP BY 
    YearMonth
ORDER BY 
    YearMonth;

-- Booking Cancelation
SELECT 
    DATE_FORMAT(Booking_Date, '%Y-%m') AS YearMonth,
    COUNT(*) AS `Booking Cancelation`
FROM 
    bookings
WHERE
    Booking_Status = 'Cancelled'    
GROUP BY 
    YearMonth
ORDER BY 
    YearMonth;
    
SELECT 
    Location,
    COUNT(*) AS `Booking Confirmed`
FROM 
    bookings
WHERE
    Booking_Status = 'Confirmed'    
GROUP BY 
    Location
ORDER BY 
    Location
LIMIT 4;  

SELECT 
    Location,
    COUNT(*) AS `Booking Cancelled`
FROM 
    bookings
WHERE
    Booking_Status = 'Cancelled'    
GROUP BY 
    Location
ORDER BY 
    Location
LIMIT 4;  

SELECT 
    Location,
    COUNT(*) AS `Booking Pending`
FROM 
    bookings
WHERE
    Booking_Status = 'Pending'    
GROUP BY 
    Location
ORDER BY 
    Location
LIMIT 4;       

-- Revenue by location
SELECT 
    Location AS Destination,
    SUM(Service_Cost) AS `Total Revenue`
FROM 
    bookings
WHERE
     Booking_Status = 'Confirmed'    
GROUP BY 
    Location
ORDER BY 
    Location DESC;

--  Service Sold  
SELECT 
    Service_Type, 
    COUNT(*) AS `Service Sold `
FROM 
    bookings
WHERE
     Booking_Status = 'Confirmed'      
GROUP BY 
    Service_Type
ORDER BY 
    `Service Sold ` DESC;
  
  -- Most Cancelled 
 SELECT 
    Service_Type, 
    COUNT(*) AS `Most Cancelled `
FROM 
    bookings
WHERE
     Booking_Status = 'Cancelled'      
GROUP BY 
    Service_Type
ORDER BY 
    `Most Cancelled ` DESC;   

-- Revenue by Service Type 
SELECT Service_Type, SUM(Service_Cost) AS Revenue
FROM bookings
WHERE
     Booking_Status = 'Confirmed' 
GROUP BY Service_Type
ORDER BY Revenue DESC;

-- Monthly Revenue Trend
SELECT 
    DATE_FORMAT(Booking_Date, '%Y-%m') AS Month,
    SUM(Service_Cost) AS Revenue
FROM bookings
WHERE
     Booking_Status = 'Confirmed' 
GROUP BY DATE_FORMAT(Booking_Date, '%Y-%m')
ORDER BY Month;

-- Top Revenue Generating Locations (Destination)
SELECT 
    Location AS Destination,
    SUM(Service_Cost) AS Revenue
FROM bookings
WHERE
     Booking_Status = 'Confirmed' 
GROUP BY Location
ORDER BY Revenue DESC;

-- Revenue by Booking Status
SELECT 
    Booking_Status,
    SUM(Service_Cost) AS Revenue
FROM bookings
GROUP BY Booking_Status;

-- Year over Year Revenue
SELECT 
    DATE_FORMAT(Booking_Date, '%Y-%m') AS Month,
    SUM(Service_Cost) AS Revenue
FROM bookings
WHERE
     Booking_Status = 'Confirmed' 
GROUP BY DATE_FORMAT(Booking_Date, '%Y-%m')
ORDER BY Month;


SELECT 
    b.Customer_ID,
    c.First_Name,
    c.Last_Name,
    SUM(b.Service_Cost) AS Customer_Lifetime_Value
FROM bookings b
JOIN customers c ON b.Customer_ID = c.Customer_ID
GROUP BY b.Customer_ID, c.First_Name, c.Last_Name
ORDER BY Customer_Lifetime_Value DESC;

-- Top 5 Most Profitable Days
SELECT 
    Booking_Date,
    SUM(Service_Cost) AS Revenue
FROM bookings
WHERE
     Booking_Status = 'Confirmed' 
GROUP BY Booking_Date
ORDER BY Revenue DESC
LIMIT 5;

-- Revenue by Day of the Week
SELECT 
    DAYNAME(Booking_Date) AS Day_Name,
    SUM(Service_Cost) AS Revenue
FROM bookings
WHERE
     Booking_Status = 'Confirmed' 
GROUP BY DAYNAME(Booking_Date)
ORDER BY Revenue DESC;

-- Percentage Revenue Contribution by Service
SELECT 
    Service_Type,
    ROUND(SUM(Service_Cost) / (SELECT SUM(Service_Cost) FROM bookings), 2) AS Revenue_Percentage
FROM bookings
WHERE
     Booking_Status = 'Confirmed' 
GROUP BY Service_Type
ORDER BY Revenue_Percentage DESC;

SELECT 
    Booking_Status,
     ROUND(SUM(Service_Cost) / (SELECT SUM(Service_Cost) FROM bookings), 2) AS Revenue_Percentage
FROM bookings
WHERE Booking_Status IN ('Confirmed', 'Cancelled')
GROUP BY Booking_Status;

-- Table
SELECT 
    b.Customer_ID,
    c.First_Name,
    c.Last_Name,
    c.Email,
    c.Phone,
    b.Location,
    b.Booking_ID,
    b.Service_Type,
    b.Service_Cost,
    b.Booking_Date,
    b.Departure_Date,
    b.Arrival_Date,
    b.Booking_Status
FROM bookings b
LEFT JOIN customers c ON b.Customer_ID = c.Customer_ID

UNION

SELECT 
    b.Customer_ID,
    c.First_Name,
    c.Last_Name,
    c.Email,
    c.Phone,
    b.Location,
    b.Booking_ID,
    b.Service_Type,
    b.Service_Cost,
    b.Booking_Date,
    b.Departure_Date,
    b.Arrival_Date,
    b.Booking_Status
FROM bookings b
RIGHT JOIN customers c ON b.Customer_ID = c.Customer_ID;

-- Total bookings and Destination 
SELECT 
    c.Customer_ID,
    c.First_Name,
    c.Last_Name,
    c.Email,
    COUNT(b.Booking_ID) AS Total_Bookings,
    SUM(b.Service_Cost) AS Total_Spent,
    MIN(b.Booking_Date) AS First_Booking,
    MAX(b.Booking_Date) AS Last_Booking,
    COUNT(DISTINCT b.Location) AS Unique_Destinations
FROM customers c
LEFT JOIN bookings b
    ON c.Customer_ID = b.Customer_ID
WHERE Service_Cost IS NOT NULL
GROUP BY 
    c.Customer_ID, c.First_Name, c.Last_Name, c.Email
ORDER BY Total_Spent DESC
LIMIT 20;

-- Customer Retention 
SELECT 
    c.Customer_ID,
    c.First_Name,
    c.Last_Name,
    COUNT(DISTINCT YEAR(b.Booking_Date)) AS Active_Years
FROM customers c
LEFT JOIN bookings b ON c.Customer_ID = b.Customer_ID
GROUP BY c.Customer_ID, c.First_Name, c.Last_Name
ORDER BY Active_Years DESC
LIMIT 6;

-- Average Spending Per Booking
SELECT 
    c.Customer_ID,
    c.First_Name,
    c.Last_Name,
    ROUND(AVG(b.Service_Cost), 2) AS Avg_Spending_Per_Booking
FROM customers c
JOIN bookings b ON c.Customer_ID = b.Customer_ID
GROUP BY c.Customer_ID, c.First_Name, c.Last_Name
ORDER BY Avg_Spending_Per_Booking DESC
LIMIT 15;

-- Booking Status Distribution by Customer
SELECT 
    c.Customer_ID,
    c.First_Name,
    c.Last_Name,
    b.Booking_Status,
    COUNT(*) AS Status_Count
FROM customers c
JOIN bookings b ON c.Customer_ID = b.Customer_ID
GROUP BY c.Customer_ID, c.First_Name, c.Last_Name, b.Booking_Status
ORDER BY Status_Count DESC
LIMIT 20;

-- Top Locations Per Customer
SELECT 
  c.Customer_ID,
  c.First_Name,
  c.Last_Name, 
  b.Location,
  COUNT(*) AS Times_Visited,
  b.Booking_Status
FROM bookings b
JOIN customers c ON b.Customer_ID = c.Customer_ID
GROUP BY c.Customer_ID, c.First_Name, c.Last_Name, b.Location, b.Booking_Status
ORDER BY Times_Visited DESC;


SELECT c.Last_Name, SUM(Service_Cost) AS Total_Spent
FROM bookings b
JOIN customers c ON b.Customer_ID = c.Customer_ID
GROUP BY c.Last_Name
ORDER BY Total_Spent DESC
LIMIT 6;

-- Most Cancellations
SELECT c.Last_Name, COUNT(*) AS Cancellations
FROM bookings b
JOIN customers c ON b.Customer_ID = c.Customer_ID
WHERE Booking_Status = 'Cancelled'
GROUP BY c.Last_Name
ORDER BY Cancellations DESC
LIMIT 5;

--  Most Confirmed
SELECT c.Last_Name, COUNT(*) AS Confirmed
FROM bookings b
JOIN customers c ON b.Customer_ID = c.Customer_ID
WHERE Booking_Status = 'Confirmed'
GROUP BY c.Last_Name
ORDER BY Confirmed DESC
LIMIT 6;

-- Top Destination
SELECT 
  YEAR(Departure_Date) AS Year,
  Location,
  COUNT(*) AS Visits
FROM bookings
GROUP BY Year, Location
ORDER BY Year, Visits DESC;

-- Location with Most Cancelation
SELECT 
  Location,
  COUNT(*) AS `Most Cancelled`
FROM bookings
WHERE Booking_Status = 'Cancelled'
GROUP BY  Location
ORDER BY `Most Cancelled` DESC;









