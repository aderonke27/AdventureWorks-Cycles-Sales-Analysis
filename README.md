# AdventureWorks Cycles Sales Analysis
![Power BI](https://img.shields.io/badge/Tool-Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Domain](https://img.shields.io/badge/Domain-Sales%20Analytics%20%7C%20Business%20Intelligence-blue)

## Table of Contents
1. [Introduction](#introduction)
2. [Project Description](#project-description)
3. [Project Aim](#project-aim)
4. [About the Dataset](#about-the-dataset)
5. [Tools Used](#tools-used)
6. [Importing the Dataset](#importing-the-dataset)
7. [Data Cleaning and Transformation](#data-cleaning-and-transformation)
8. [Data Modeling](#data-modeling)
9. [Data Analysis](#data-analysis)
10. [Data Visualization](#data-visualization)
11. [Key Insights](#key-insights)
12. [Recommendations](#recommendations)
13. [Conclusion](#conclusion)
14. [Project Files](#project-files)
15. [Contact Information](#contact-information)

## Introduction
This project was completed as part of the AltSchool Africa School of Data 3rd Semester assessment. It covers the full analytical workflow from raw data import and transformation through to data modeling, DAX measure creation, interactive dashboard development, and strategic business reporting using Power BI.

The AdventureWorks dataset was used as the foundation for this analysis. The project simulates a real-world business intelligence engagement where a data analyst evaluates company performance and delivers actionable strategic insights to leadership.

## Project Description
AdventureWorks Cycles is a multinational bicycle manufacturing company that sells products across the United States and internationally. This analysis examines four years of sales data (2011–2014) across multiple dimensions — revenue generation, cost and profitability, product performance, and territory analysis — to produce a comprehensive view of business performance.

The project was conducted from a sales analytics and business strategy perspective, with a focus on answering four core business questions:
* Sales Overview — How much was sold, and how did sales trend over time?
* Cost and Profit Analysis — How efficiently is the business converting revenue into profit?
* Product Performance — Which products and categories drive the most value?
* Territory Analysis — Where are sales coming from, and which markets are most profitable?

The deliverable was a structured analytical report accompanied by four interactive Power BI dashboards, each addressing one of the above dimensions.

## Project Aim
The aim of this project was to conduct a comprehensive sales performance analysis of AdventureWorks Cycles using Power BI, with the following specific objectives:
* Evaluate total revenue, cost, profit, and profitability trends across a four-year period (2011–2014)
* Identify top-performing and underperforming product lines, categories, and subcategories
* Analyse sales performance and profitability across global territories and markets
* Uncover patterns in order volume, customer behaviour, and revenue concentration
* Develop interactive, executive-level dashboards that communicate findings clearly and support data-driven decision-making
* Deliver structured strategic recommendations backed by data to guide business growth

## About the Dataset
The AdventureWorks dataset is a relational Microsoft SQL Server sample database representing the operations of a fictional bicycle manufacturer. It contains sales transactions, product information, customer records, and territory data spanning 2011 to 2014.

The following eight tables were imported and used in the analysis:
* Sales.SalesOrderDetail — Line-item detail for each sales transaction, including product, quantity, and unit price
* Sales.SalesOrderHeader — Order-level information including order date, customer ID, and territory ID
* Production.Product — Product master data including product name, class, style, product line, and standard cost
* Production.ProductSubcategory — Subcategory classification for each product (e.g. Mountain Bikes, Road Bikes, Helmets)
* Production.ProductCategory — High-level category grouping (Bikes, Components, Clothing, Accessories)
* Sales.Customer — Customer identifiers linking sales transactions to individual customers
* Person.Person — Personal details associated with each customer record
* Sales.SalesTerritory — Territory names, regional groupings, and country assignments

**Dataset Source:**
The AdventureWorks dataset was provided by AltSchool Africa as part of the Semester 3 assignment. The original dataset is also publicly available via [this Google Drive link](https://drive.google.com/file/d/1r8CPa0tVWkr_EJSengzXTXpnUGg3ghll/view?usp=sharing).

**Key Assumptions:**
* StandardCost was used as the cost of goods sold, as a detailed operational cost breakdown was not available in the dataset
* Missing categorical values replaced with "Unknown" were assumed not to significantly distort product or customer segmentation
* OrderDate was used as the point of revenue recognition for all time-series and trend analysis

## Tools Used
Power BI

## Importing the Dataset
The AdventureWorks dataset was imported into Power BI Desktop by connecting to the SQL Server database. The following eight tables were selected and loaded into the Power BI environment for transformation and analysis: Sales.SalesOrderDetail, Sales.SalesOrderHeader, Production.Product, Production.ProductSubcategory, Production.ProductCategory, Sales.Customer, Person.Person, and Sales.SalesTerritory.

Each table was loaded into Power Query Editor as a separate query before cleaning and transformation were applied. After all transformations were completed, the Close & Apply function was used to load the cleaned tables into the Power BI data model.

## Data Cleaning and Transformation
All data cleaning and transformation steps were performed in Power Query Editor before loading the data into the model.

**Data Type Corrections**
Column data types were reviewed and corrected where necessary. ID columns that were incorrectly stored as date format were converted to text to prevent unintended aggregations and ensure accurate relationship matching.

**Categorical Field Transformation**
Several categorical fields contained abbreviated codes that were not meaningful in a business context. These were expanded into readable labels to improve interpretability across all dashboards:
- Product Class: L → Low, M → Medium, H → High
- Product Line: M → Mountain, R → Road, S → Standard, T → Touring
- Product Style: W → Women, M → Men, U → Universal

**Handling Null and Missing Values**
All columns were reviewed for null and missing values. Where missing values appeared in categorical fields — such as product line or product style — they were replaced with "Unknown" to prevent null values from distorting aggregations or visual outputs. Columns with excessive null values that were not relevant to the analysis were removed entirely.

After completing all transformation steps, the cleaned tables were loaded into the Power BI data model using the Close & Apply function, ready for relationship building and measure creation.

## Data Modeling
Following data import and cleaning, a relational data model was constructed in Power BI's Model View. Relationships between tables were established using primary and foreign keys to enable accurate cross-table analysis.

The following relationships were configured, all set as one-to-many:
| From Table | Key | To Table | Key |
|---|---|---|---|
| Sales.SalesOrderDetail | ProductID | Production.Product | ProductID |
| Sales.SalesOrderHeader | CustomerID | Sales.Customer | CustomerID |
| Sales.SalesOrderHeader | TerritoryID | Sales.SalesTerritory | TerritoryID |
| Production.Product | ProductSubcategoryID | Production.ProductSubcategory | ProductSubcategoryID |
| Production.ProductSubcategory | ProductCategoryID | Production.ProductCategory | ProductCategoryID |
| Sales.Customer | PersonID | Person.Person | BusinessEntityID |

This star-schema-style model ensured that all dimensions — products, customers, and territories — were correctly linked to the central sales fact tables, enabling accurate slicing and filtering across all dashboard pages.


## Data Analysis
With the data model in place, the following core DAX measures were created to serve as the analytical foundation for all dashboard visualisations:

```dax
Total Revenue = SUMX(Sales_SalesOrderDetail, Sales_SalesOrderDetail[OrderQty] * Sales_SalesOrderDetail[UnitPrice])

Total Cost = SUMX(Sales_SalesOrderDetail, Sales_SalesOrderDetail[OrderQty] * RELATED(Production_Product[StandardCost]))

Total Profit = [Total Revenue] - [Total Cost]

Profit Margin (%) = DIVIDE([Total Profit], [Total Revenue], 0)

Total Orders = DISTINCTCOUNT(Sales_SalesOrderHeader[SalesOrderID])
```

The analysis was structured across four key dimensions, each forming the basis of a dedicated dashboard page:

**1. Sales Overview**
Revenue, quantity sold, total orders, and customer count were evaluated across the full time period. Trends were analysed by year, product class, and product style to understand what was driving volume and revenue growth.

**2. Cost and Profit Analysis**
Total cost and profit were broken down by product line and tracked over time to assess profitability trends. The profit margin by year was examined to determine whether the business was becoming more or less efficient at converting revenue into profit.

**3. Product Performance**
Revenue and profit were analysed across product categories, subcategories, and product lines. The top 10 products by revenue and cost were identified to highlight where performance was concentrated.

**4. Territory Analysis**
Revenue, profit, total orders, and units sold were compared across all territories and countries to identify the strongest and weakest performing markets.

## Data Visualization
Four interactive Power BI dashboard pages were developed, each addressing one analytical dimension.
Sales overview dashboard showing revenue, units sold, orders, and customer KPIs
Cost and profit analysis dashboard by product line and year
Product Performance dashboard by product category, subcategory, and top 10 product revenue
Territory Analysis dashboard by country-level revenue, profit, and orders

## Key Insights
### 1. Overall Sales Performance
The company generated **$109.85M in total revenue** from **275,000 units sold** across **31,465 orders**, serving approximately **19,820 customers** between 2011 and 2014. Revenue and sales volume grew consistently from 2011 through 2013, peaking in 2013 before declining in 2014 — a pattern that may reflect market saturation, increased competition, or internal operational challenges worth investigating.

Universal style products accounted for the highest quantity sold at **56.24%**, indicating that broad-appeal products drive volume. High-class products, however, generated the highest revenue share at **64.27% ($70.59M)**, confirming that premium products contribute disproportionately to revenue despite lower unit volumes. The scatter plot analysis further confirmed a positive relationship between order size and revenue, with most transactions concentrated at smaller order quantities.

### 2. Cost and Profit Analysis
Total cost stood at **$100.47M**, resulting in a **total profit of $9.37M** and an **overall profit margin of 9%**. While the company is profitable, the thin margin points to significant cost absorption — over 91% of revenue goes toward production and operational costs.

That said, the profit margin trend is encouraging. After dipping in 2012, the margin recovered and reached its highest point in 2014 — even as overall revenue declined. This shows the company became more efficient at converting revenue into profit in its later years, which is a meaningful sign of operational improvement. The **Mountain product line led profitability at $5.63M**, followed by Road at **$2.82M**.

### 3. Product Performance
The **Bikes category dominated revenue**, contributing **$94.65M — 86.17% of total revenue**. Within Bikes, **Mountain Bikes were the most profitable subcategory at $4.91M**, followed by Road Bikes at **$2.81M**. The Road product line led in total revenue at **$48.30M**, with Mountain close behind at **$42.46M**.

The top 10 products by revenue were dominated by Mountain-200 variants, with the single highest-earning product generating **$4.40M**. This concentration is both a strength — clear product-market fit — and a risk, as heavy dependence on a limited product range creates vulnerability if that line is disrupted.

The Standard and Touring product lines contributed significantly lower revenue and profit and may not be justifying their position in the portfolio without strategic repositioning.

### 4. Territory Analysis
The **United States was the primary market**, generating **$63.00M** — approximately 57% of total company revenue. The **Southwest territory led in both units sold (59,110) and revenue ($24.18M)**, while **Australia stood out as the most profitable territory at $3.43M** despite ranking third in revenue — a sign of exceptional cost efficiency in that market.

The Southwest, Northwest, and Canada were the strongest performers overall. However, three territories — **Central, Southeast, and Northeast** — recorded **negative profit**, with the Northeast posting the worst loss at **-$0.27M**, pointing to uneven regional performance that needs attention.

## Recommendations
### 1. Improve Profit Margins Through Cost Optimisation
With an overall profit margin of 9%, a detailed cost review across production, distribution, and regional operations is overdue. The Road product line in particular generates the highest revenue ($48.30M) but a relatively lower profit ($2.82M) — suggesting a pricing or cost structure inefficiency worth addressing.

### 2. Prioritise the Mountain and Road Product Lines
These two lines consistently deliver the strongest revenue and profit outcomes. Priority investment in marketing, product development, and inventory management — particularly for Mountain-200 variants — should be maintained and expanded to protect and grow the company's most valuable assets.

### 3. Review Underperforming Product Lines and Categories
The Standard and Touring lines, along with the Clothing and Accessories categories, contribute disproportionately low revenue and profit. A strategic review should determine whether repositioning, pricing adjustment, cost reduction, or portfolio rationalisation is the right path forward.

### 4. Investigate the Post-2013 Revenue Decline
The revenue decline after 2013 needs a proper root cause analysis. Possible factors include market saturation, competitive pressure, changes in customer buying behaviour, or internal operational shifts. Understanding the driver is critical before the trend worsens.

### 5. Strengthen the US and Southwest Market Strategy
With the US accounting for 57% of total revenue and the Southwest leading across revenue and units, these markets must remain the strategic core. Continued investment in distribution, customer retention, and marketing in these regions is essential.

### 6. Replicate Australia's Profitability Model
Australia generated $3.43M profit from $10.66M revenue — a margin of roughly 32%, compared to the company average of 9%. The cost structures and operational practices driving that efficiency should be studied and applied to other markets, particularly those in loss.

### 7. Resolve Loss-Making Territories
Central, Southeast, and Northeast are all operating at a loss. A targeted review of pricing, logistics costs, and demand patterns in these territories is needed. Where losses cannot be reversed within a reasonable period, restructuring or exit should be considered.

## Conclusion
This analysis of AdventureWorks Cycles sales data (2011–2014) revealed a business with strong revenue capacity but thin margins, clear product strengths, and uneven geographic performance. The Bikes category — particularly Mountain and Road — is the commercial backbone, generating over 86% of total revenue. The United States and Southwest remain the dominant markets, while Australia quietly outperforms everyone on profitability.

The post-2013 revenue decline is the most important challenge facing the business. But the improving profit margin in that same period is a meaningful positive signal — showing that operational efficiency was improving even as top-line growth slowed. Addressing the revenue decline, resolving loss-making territories, and rationalising the underperforming product lines are the clearest paths to stronger, more sustainable performance.

From a technical standpoint, this project demonstrates the full Power BI workflow — from raw data import and Power Query transformation, through relational data modeling and DAX measure creation, to interactive executive dashboards. It also reflects the ability to move beyond charts and numbers to deliver structured, business-focused narratives that support real decisions — which is ultimately what data analysis is for.

## Project Files
Full analytical report including methodology, key insights, and strategic recommendations
[View Sales Report (PDF)](./AdventureWorks_Sales_Report.pdf)

## Contact Information
* Email: aladeloyeesther616@gmail.com
* LinkedIn: [linkedin.com/in/estheraderonke](https://linkedin.com/in/estheraderonke)
