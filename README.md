![Shape2](RackMultipart20221024-1-b0jkgv_html_9e54ad8735b0307b.gif) ![Shape1](RackMultipart20221024-1-b0jkgv_html_2db1e607b58cef7.gif)

**Date:** 10/23/2022 9:25:37 PM UTC

![](RackMultipart20221024-1-b0jkgv_html_c65792e49d8bc6ba.png) ![Shape4](RackMultipart20221024-1-b0jkgv_html_b899cc170e6fa41e.gif) ![Picture 3](RackMultipart20221024-1-b0jkgv_html_26f415b3f29357f6.gif) ![](RackMultipart20221024-1-b0jkgv_html_a04cf3a6b963ef7e.png)[![](RackMultipart20221024-1-b0jkgv_html_25905c78001374b1.png)](https://www.linkedin.com/in/anas-aljarrah/) ![Shape3](RackMultipart20221024-1-b0jkgv_html_759575f9e8c35675.gif)

[View in Power BI](https://app.powerbi.com/groups/me/reports/c37d2bd9-5773-41cb-9cbe-550293122765?pbi_source=PowerPoint)

_Created By:_

Anas Al Jarrah

# Data Analyst Portfolio Project – Sales Management

# Scenario:

An executive sales report for sales managers was required in the business request for this data analytics project. So, by following the data process and tools (SQL, Power BI) to complete delivery and guarantee that acceptance criteria were upheld throughout the project based on the request that was made by the company.

![](RackMultipart20221024-1-b0jkgv_html_31b43e5061f64210.png)

# Business Request & User Stories

**Business Demand Overview:**

- Reporter: Steven – Sales Manager
- Value of Change: Visual dashboards and improved Sales reporting or follow up or sales force
- Necessary Systems: Power BI, CRM System
- Other Relevant Info: Budgets have been delivered in Excel for 2021

**User Stories:**

| No # | As a (role) | I want (request / demand) | So that I (user value) | Acceptance Criteria |
| --- | --- | --- | --- | --- |
| 1 | Sales Manager | To get a dashboard overview of internet sales | Can follow better which customers and products sells the best | A Power BI dashboard which updates data once a day |
| 2 | Sales Representative | A detailed overview of Internet Sales per Customers | Can follow up my customers that buys the most and who we can sell ore to | A Power BI dashboard which allows me to filter data for each customer |
| 3 | Sales Representative | A detailed overview of Internet Sales per Products | Can follow up my Products that sells the most | A Power BI dashboard which allows me to filter data for each Product |
| 4 | Sales Manager | A dashboard overview of internet sales | Follow sales over time against budget | A Power Bi dashboard with graphs and KPIs comparing against budget. |

# Data Cleansing & Transformation (SQL)

The following tables were retrieved using SQL to generate the appropriate data model for conducting analysis and meeting the business needs outlined in the user stories. Sales budgets, one data source, were sent to the process in Excel format and afterwards joined in the data model.

The SQL statements for data cleansing and transformation are listed below.

**DIM\_Calender:**

![Shape5](RackMultipart20221024-1-b0jkgv_html_b2a9fcf8b97d7309.gif)

--Cleansed DIM\_Date Table --
SELECT
 [DateKey],
 [FullDateAlternateKey] AS Date,
--[DayNumberOfWeek],
 [EnglishDayNameOfWeek] AS Day,
--[SpanishDayNameOfWeek],
--[FrenchDayNameOfWeek],
--[DayNumberOfMonth],
--[DayNumberOfYear],
--[WeekNumberOfYear],
 [EnglishMonthName] AS Month,
**Left** ([EnglishMonthName], 3) AS MonthShort, --Useful **for** front end date navigation and front end graphs.
--[SpanishMonthName],
--[FrenchMonthName],
 [MonthNumberOfYear] AS MonthNo,
 [CalendarQuarter] AS Quarter,
 [CalendarYear] AS Year --[CalendarSemester],
--[FiscalQuarter],
--[FiscalYear],
--[FiscalSemester]
FROM
 [AdventureWorksDW2019].[dbo].[DimDate]
WHERE
 CalendarYear \>=2019

**Output:**

So now you can see, we have the day table, we have the date key, which I can connect to our facts so that I can visualize stuff over time. You have to date the day. If I want to use, if I want to look at sales for the different days in the way, If I want to look at sales throughout the week month short, etc., I have all the columns that I need.

![Shape6](RackMultipart20221024-1-b0jkgv_html_afddcec011d52d2a.gif)

--Cleansed DIM\_Customers Table --
SELECT
 c.customerkey AS CustomerKey,
--,[GeographyKey]
--,[CustomerAlternateKey]
--,[Title]
 c.firstname AS [First Name],
--,[MiddleName]
 c.lastname AS [Last Name],
 c.firstname + ' '+lastname AS [Full Name],
--Combined First and Last Name
--,[NameStyle]
--,[BirthDate]
--,[MaritalStatus]
--,[Suffix]
 CASE c.gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gender,
--,[EmailAddress]
--,[YearlyIncome]
--,[TotalChildren]
--,[NumberChildrenAtHome]
--,[EnglishEducation]
--,[SpanishEducation]
--,[FrenchEducation]
--,[EnglishOccupation]
--,[SpanishOccupation]
--,[FrenchOccupation]
--,[HouseOwnerFlag]
--,[NumberCarsOwned]
--,[AddressLine1]
--,[AddressLine2]
--,[Phone]
 c.datefirstpurchase AS DateFirstPurchase,
--,[CommuteDistance]
 g.city AS [Customer City] --Joined **in** Customer City from Geography Table
FROM
 [AdventureWorksDW2019].[dbo].[DimCustomer] as c
 LEFT JOIN dbo.dimgeography AS g ON g.geographykey =c.geographykey
ORDER BY
 CustomerKey ASC --Ordered List by CustomerKey

**DIM\_Customer:**

**Output:**

what I've done is I've gone in; I've used a case statement. So basically, if whatever the value in the column equals M, then use male, or else or when it is F, then do female, I just think it gives me a better column for later analysis. If I were to look at sales per different genders, foreign product male and female, I think it sounds better than M or F. Also, what I've done is I've done **a left join** so you can see I have given the din customer a different name as C so you can see I have C dot something on each column which comes from the customer table. But on this one I have a G which comes to my different table and that is from my geography table. what I wanted to do is I wanted to join in the customer city which city that's the customer belong to. I ordered by under customer key ascending, just a sort them by the ascending. When the customers were put into the CRM system.

![Shape7](RackMultipart20221024-1-b0jkgv_html_716f4360d2c94bbe.gif)

--Cleansed DIM\_Products Table --
SELECT
 p.[ProductKey],
 p.[ProductAlternateKey] AS ProductItemCode,
--,[ProductSubcategoryKey],
--,[WeightUnitMeasureCode]
--,[SizeUnitMeasureCode]
 p.[EnglishProductName] AS [Product Name],
 ps.EnglishProductSubcategoryName AS [Sub Category], --Joined **in** from Sub Category Table
 pc.EnglishProductCategoryName AS [Product Category], --Joined **in** from Category Table
--,[SpanishProductName]
--,[FrenchProductName]
--,[StandardCost]
--,[FinishedGoodsFlag]
 p.[Color] AS [Product Color],
--,[SafetyStockLevel]
--,[ReorderPoint]
--,[ListPrice]
 p.[Size] AS [Product Size],
--,[SizeRange]
--,[Weight]
--,[DaysToManufacture]
 p.[ProductLine] AS [Product Line],
--,[DealerPrice]
--,[Class]
--,[Style]
 p.[ModelName] AS [Product Model Name],
--,[LargePhoto]
 p.[EnglishDescription] AS [Product Description],
--,[FrenchDescription]
--,[ChineseDescription]
--,[ArabicDescription]
--,[HebrewDescription]
--,[ThaiDescription]
--,[GermanDescription]
--,[JapaneseDescription]
--,[TurkishDescription]
--,[StartDate],
--,[EndDate],
**ISNULL** (p.Status, 'Outdated') AS [Product Status]
FROM
 [AdventureWorksDW2019].[dbo].[DimProduct] as p
 LEFT JOIN dbo.DimProductSubcategory AS ps ON ps.ProductSubcategoryKey =p.ProductSubcategoryKey
 LEFT JOIN dbo.DimProductCategory AS pc ON ps.ProductCategoryKey =pc.ProductCategoryKey
order by
 p.ProductKey asc

**DIM\_Products:**

**Output:**

Here we did calendar with products table. Basically, more or less the same as the previous one. I have done. The biggest difference here is I have joined in two different things because I remember that Stephen was interested in looking at sales for product. So, I've gone in, I have found the product key product adding code, I have renamed that is to ask statement and then notice I have joined in the subcategory and the category because he might be interested in looking at sales per subcategory or per the main category and he wants to look at it in different ways. As well, I'm trying to pick the columns that I think I can use some sort of analysis later. Does he want to look at sales bar product color? Not sure. But it's, you know, it could be interesting product size, maybe in the pipeline, product line, product model name, product description category, I think is a given. And then on this one, the I've done a data cleansing step. After look at the example column, you can see it says currents. Now there will be some that says, no, and I've just kind of taken it for granted that if it's saying, no, it's not correct. So, what I did is I did a, is null function. So, it's looking at the pieces. If it's no, then put this in there and what it does is given me outdated or a current in the column, which I can use. If I want to filter on the product status, for whatever reasons, I'm just trying to think ahead of what could be useful. Once again, I've left joined as before. Join to get in the private subcategory based on the private separate category in the dim product subcategory, and in the product table, and of course, didn't try to category can probably category key and try to get product category key and join on those, which is allowed me to fetch the data. This these two columns and I use P as product. Then I guess I use PC as product category and PS private subcategory.

![Shape8](RackMultipart20221024-1-b0jkgv_html_fbf684788b0ae105.gif)

--Cleansed FACT\_InternetSales Table --
SELECT
 [ProductKey],
 [OrderDateKey],
 [DueDateKey],
 [ShipDateKey],
 [CustomerKey],
--,[PromotionKey]
--,[CurrencyKey]
--,[SalesTerritoryKey]
 [SalesOrderNumber],
--[SalesOrderLineNumber],
--,[RevisionNumber]
--,[OrderQuantity],
--,[UnitPrice],
--,[ExtendedAmount]
--,[UnitPriceDiscountPct]
--,[DiscountAmount]
--,[ProductStandardCost]
--,[TotalProductCost]
 [SalesAmount] --,[TaxAmt]
--,[Freight]
--,[CarrierTrackingNumber]
--,[CustomerPONumber]
--,[OrderDate]
--,[DueDate]
--,[ShipDate]
FROM
 [AdventureWorksDW2019].[dbo].[FactInternetSales]
WHERE
**LEFT** (OrderDateKey, 4) \>= **YEAR** ( **GETDATE** ()) -2--Ensures we always only bring two years of date from extraction.
ORDER BY
 OrderDateKey ASC

**FACT\_InternetSales:**

**Output:**

What I've done here is this this function is a adjust date difference. So, it gets today's date. It finds out based on what date right now. What is today's date. And the year, when I wrap it in year, that's going to turn that date into a year value. Also, what I've done is that I've taken, I've taken the left of the order date key. And if you look at the order that key, the left value is the year. I only I fetched the left part before first Numbers in the order that key. And I say if whatever this function is larger than or equal to the 4/4, the fourth first numbers in order key, then return the role Just a little bit. It's a different type of, it's a different type of function. The advantage of doing this though, is that they if when we go into 2020 to the function will automatically update so that we get then we get 2020 and 2021 and you don't have to go in manually into the sequel code and change it.

# Power BI:

This section displays the data models imported and connected to form a dashboard for sales department to provide better insights about their data and visualization for better understanding.

# Data Model:

Below is a screenshot of the data model after cleansed and prepared tables were imported into Power BI. This data model also shows how FACT\_Budget has been connected to FACT\_InternetSales and other DIM tables within the model.

![](RackMultipart20221024-1-b0jkgv_html_623a688d99f26158.png)

# ![Shape9](RackMultipart20221024-1-b0jkgv_html_425bbb16e3c3502.gif)

Microsoft Power BI

Sales Management Dashboard:

# ![](RackMultipart20221024-1-b0jkgv_html_aa3476f349f5266d.png) ![](RackMultipart20221024-1-b0jkgv_html_54406b66b405a7f9.png) ![](RackMultipart20221024-1-b0jkgv_html_24fb5a5daf76d0e5.png) ![Shape10](RackMultipart20221024-1-b0jkgv_html_2cec9bca45673145.gif)

[View in Power BI](https://app.powerbi.com/groups/me/reports/c37d2bd9-5773-41cb-9cbe-550293122765?pbi_source=PowerPoint)
