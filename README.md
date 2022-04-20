# SalesInsights
SQL data analysis of the sales information


--  Show total number of customers


SELECT count(*) FROM sales.customers;


--  Show transactions where currency is US dollars


SELECT * FROM sales.transactions 
WHERE currency="USD"


-- Joining two tables with dates and transactions from 2020 year

SELECT dat.*, tran.* 
FROM sales.date AS dat
 INNER JOIN sales.transactions AS tran
 ON tran.order_date = dat.date
WHERE dat.year = "2020"
Order BY dat.date


-- Finding the revenue for the 2020 year


SELECT SUM(tran.sales_amount) 
FROM sales.date AS dat
  INNER JOIN sales.transactions AS tran
  ON tran.order_date = dat.date
WHERE dat.year = "2020"
Order BY dat.date


-- Selecting the list of products which were sold in the certain market


SELECT distinct(tran.product_code) 
FROM sales.transactions AS tran
WHERE tran.market_code = "Mark001"
Order BY tran.product_code


-- Using CTE's to find revenue change in 2020 to 2019


WITH cte_revenue2020 (revenue, id)
AS
(
SELECT SUM(tran.sales_amount), 1 
FROM sales.date AS dat
INNER JOIN sales.transactions AS tran
ON tran.order_date = dat.date
WHERE dat.year = "2020"
Order BY dat.date
),
cte_revenue2019 (revenue, id)
AS
(
SELECT SUM(tran.sales_amount), 1 
FROM sales.date AS dat
INNER JOIN sales.transactions AS tran
ON tran.order_date = dat.date
WHERE dat.year = "2019"
Order BY dat.date
)
SELECT ROUND(cte_revenue2020.revenue/cte_revenue2019.revenue, 2) AS "RevenueChange%" 
FROM cte_revenue2020
INNER JOIN cte_revenue2019
ON cte_revenue2020.id = cte_revenue2019.id
