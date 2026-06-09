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
This project was completed as part of the AltSchool Africa School of Data — Semester 3 assessment, and represents a demonstration of end-to-end business intelligence skills using Power BI. It covers the full analytical workflow from raw data import and transformation through to data modeling, DAX measure creation, interactive dashboard development, and strategic business reporting.

The AdventureWorks dataset — a widely used Microsoft sample database representing a fictional bicycle manufacturing and retail company — was selected as the foundation for this analysis. The project simulates a real-world business intelligence engagement where a data analyst is tasked with evaluating company performance and delivering actionable strategic insights to leadership.

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
The AdventureWorks dataset is a relational Microsoft SQL Server sample database representing the operations of a fictional bicycle manufacturer. It contains sales transactions, product information, customer records, and territory data spanning the period from 2011 to 2014.

The following tables were imported and used in the analysis:
- **Sales.SalesOrderDetail** — Line-item detail for each sales transaction, including product, quantity, and unit price
- **Sales.SalesOrderHeader** — Order-level information including order date, customer ID, and territory ID
- **Production.Product** — Product master data including product name, class, style, product line, and standard cost
- **Production.ProductSubcategory** — Subcategory classification for each product (e.g. Mountain Bikes, Road Bikes, Helmets)
- **Production.ProductCategory** — High-level category grouping (Bikes, Components, Clothing, Accessories)
- **Sales.Customer** — Customer identifiers linking sales transactions to individual customers
- **Person.Person** — Personal details associated with each customer record
- **Sales.SalesTerritory** — Territory names, regional groupings, and country assignments

**Dataset Summary:**
- Total Orders: 31,465
- Total Units Sold: 275,000
- Total Revenue: $109.85M
- Total Cost: $100.47M
- Total Profit: $9.37M
- Overall Profit Margin: 9%
- Total Customers: 19,820
- Time Period: 2011 – 2014
- Markets: United States, Canada, Australia, United Kingdom, France, Germany

**Dataset Source:**
The AdventureWorks dataset was provided by AltSchool Africa as part of the Semester 3 assignment. The original dataset is also publicly available via [this Google Drive link](https://drive.google.com/file/d/1r8CPa0tVWkr_EJSengzXTXpnUGg3ghll/view?usp=sharing).

**Key Assumptions:**
- StandardCost was used as the cost of goods sold, as a detailed operational cost breakdown was not available in the dataset
- Missing categorical values replaced with "Unknown" were assumed not to significantly distort product or customer segmentation
- OrderDate was used as the point of revenue recognition for all time-series and trend analysis

---

## Tools Used
Power BI

## Importing the Dataset
The AdventureWorks dataset was imported into Power BI Desktop by connecting to the SQL Server database. The following eight tables were selected and loaded into the Power BI environment for transformation and analysis:

Sales.SalesOrderDetail, Sales.SalesOrderHeader, Production.Product, Production.ProductSubcategory, Production.ProductCategory, Sales.Customer, Person.Person, and Sales.SalesTerritory.

Each table was loaded into Power Query Editor as a separate query before cleaning and transformation were applied. After all transformations were completed, the Close & Apply function was used to load the cleaned tables into the Power BI data model.

---

## Data Cleaning and Transformation

All data cleaning and transformation steps were performed in Power Query Editor before loading the data into the model. The following transformations were applied:

**Data Type Corrections**
Column data types were reviewed and corrected where necessary. Notably, ID columns that were incorrectly stored as date format were converted to text to prevent unintended aggregations and ensure accurate relationship matching.

**Categorical Field Transformation**
Several categorical fields contained abbreviated codes that were not meaningful in a business context. These were transformed into readable labels to improve interpretability across all dashboards:
- Product Class: L → Low, M → Medium, H → High
- Product Line: M → Mountain, R → Road, S → Standard, T → Touring
- Product Style: W → Women's, M → Men's, U → Universal

**Handling Null and Missing Values**
All columns were reviewed for null and missing values. Where missing values appeared in categorical fields — such as product line or product style — they were replaced with "Unknown" to prevent null values from being excluded from aggregations or distorting visual outputs. Columns with excessive null values that were not relevant to the analysis were removed entirely.

**Outcome**
After completing all transformation steps, the cleaned tables were loaded into the Power BI data model using the Close & Apply function, ready for relationship building and measure creation.

---

## Data Modeling

Following data import and cleaning, a relational data model was constructed in Power BI's Model View. Relationships between tables were established using primary and foreign keys to enable accurate cross-table analysis.

The following relationships were configured, all set as **one-to-many**:

| From Table | Key | To Table | Key |
|---|---|---|---|
| Sales.SalesOrderDetail | ProductID | Production.Product | ProductID |
| Sales.SalesOrderHeader | CustomerID | Sales.Customer | CustomerID |
| Sales.SalesOrderHeader | TerritoryID | Sales.SalesTerritory | TerritoryID |
| Production.Product | ProductSubcategoryID | Production.ProductSubcategory | ProductSubcategoryID |
| Production.ProductSubcategory | ProductCategoryID | Production.ProductCategory | ProductCategoryID |
| Sales.Customer | PersonID | Person.Person | BusinessEntityID |

This star-schema-style model ensured that all dimensions (products, customers, territories) were correctly linked to the central sales fact tables, enabling accurate slicing and filtering across all dashboard pages.

---

## Data Analysis

With the data model in place, the following core DAX measures were created to serve as the analytical foundation for all dashboard visualisations:

```dax
Total Revenue = SUMX(Sales_SalesOrderDetail, Sales_SalesOrderDetail[OrderQty] * Sales_SalesOrderDetail[UnitPrice])

Total Cost = SUMX(Sales_SalesOrderDetail, Sales_SalesOrderDetail[OrderQty] * RELATED(Production_Product[StandardCost]))

Total Profit = [Total Revenue] - [Total Cost]

Profit Margin (%) = DIVIDE([Total Profit], [Total Revenue], 0)

Total Orders = DISTINCTCOUNT(Sales_SalesOrderHeader[SalesOrderID])
```

The analysis was then structured across four key dimensions, each forming the basis of a dedicated dashboard page:

**1. Sales Overview**
Revenue, quantity sold, total orders, and customer count were evaluated across the full time period. Trends were analysed by year, product class, and product style to understand what was driving volume and revenue growth.

**2. Cost and Profit Analysis**
Total cost and profit were broken down by product line and tracked over time to assess profitability trends. The profit margin by year was examined to determine whether the business was becoming more or less efficient at converting revenue into profit.

**3. Product Performance**
Revenue and profit were analysed across product categories, subcategories, and product lines. The top 10 products by revenue and cost were identified to highlight where performance was concentrated.

**4. Territory Analysis**
Revenue, profit, total orders, and units sold were compared across all territories and countries to identify the strongest and weakest performing markets.

---

## Data Visualization

Four interactive Power BI dashboard pages were developed, each addressing one analytical dimension. The following visualisation types were used across the dashboards:

- **KPI Cards** — for at-a-glance display of Total Revenue, Total Cost, Total Profit, Profit Margin, Total Orders, Total Quantity Sold, and Total Customers
- **Line Charts** — for year-on-year trend analysis of revenue, cost, profit, and profit margin
- **Bar Charts** — for product line, subcategory, and territory performance comparisons
- **Donut Charts** — for proportional breakdowns of revenue and quantity by product class, style, and category
- **Scatter Plot** — for visualising the relationship between order quantity and revenue
- **Map Visual** — for geographic revenue distribution across territories
- **Horizontal Bar Charts** — for top 10 product rankings by revenue and cost

All dashboards were designed with a consistent navy blue colour theme, clear labelling, and intuitive layout to support executive-level readability and decision-making.

> 📁 Dashboard screenshots are available in the repository files below.

---

## Key Insights

### 1. Overall Sales Performance
The company generated **$109.85M in total revenue** from **275,000 units sold** across **31,465 orders**, serving approximately **19,820 customers** between 2011 and 2014. Revenue and sales volume showed consistent growth from 2011 through 2013, peaking in 2013 before declining in 2014. This pattern suggests strong early expansion followed by a slowdown that may reflect market saturation, increased competition, or internal operational challenges.

Universal style products accounted for the highest quantity sold at **56.24%**, indicating that broad-appeal products drive volume. High-class products generated the highest revenue share at **64.27% ($70.59M)**, confirming that premium products contribute disproportionately to revenue despite lower unit volumes.

### 2. Cost and Profit Analysis
Total cost stood at **$100.47M**, resulting in a **total profit of $9.37M** and an **overall profit margin of 9%**. While the company is profitable, the thin margin indicates limited efficiency in either cost management or pricing strategy, with a significant proportion of revenue absorbed by production and operational costs.

However, the profit margin by year tells an encouraging story. After dipping in 2012, profit margin recovered and reached its highest point in 2014 — even as overall sales declined. This indicates that the company became increasingly efficient at converting revenue into profit, demonstrating meaningful operational improvements in its later years. The Mountain product line was the most profitable at **$5.63M**, followed by Road at **$2.82M**.

### 3. Product Performance
The **Bikes category dominated revenue generation**, contributing **$94.65M (86.17%)** of total revenue. Within this category, **Mountain Bikes were the most profitable subcategory at $4.91M**, followed by Road Bikes at **$2.81M**. The Road product line led in total revenue at **$48.30M**, while Mountain followed closely at **$42.46M**.

The top 10 products by revenue were dominated by Mountain-200 variants, with the highest single product generating **$4.40M**. This concentration of performance among a small group of products highlights both a strength — clear product-market fit — and a risk — heavy dependence on a limited product range.

The Standard and Touring product lines contributed significantly lower revenue and profit, suggesting they may not be justifying their place in the portfolio without strategic repositioning.

### 4. Territory Analysis
The **United States was the primary market**, generating **$63.00M** in revenue — approximately 57% of total company revenue. The **Southwest territory led in total units sold (59,110)** and was the highest revenue territory at **$24.18M**, while **Australia generated the highest profit among all territories at $3.43M** — indicating exceptional cost efficiency relative to revenue in that market.

The **Southwest, Northwest, and Canada** territories were the strongest performers across both revenue and order volume. Three territories — **Central, Southeast, and Northeast** — recorded **negative profit**, with Northeast posting the worst loss at **-$0.27M**, pointing to uneven regional performance and potential operational or pricing inefficiencies in these markets.

---

## Recommendations

### 1. Improve Profit Margins Through Cost Optimisation
With an overall profit margin of 9%, there is significant room for improvement. A detailed cost review should be conducted across production, distribution, and regional operations to identify inefficiencies. Pricing strategies for high-volume products — particularly Road Bikes — should be reassessed to improve margin performance without negatively impacting demand.

### 2. Prioritise the Mountain and Road Product Lines
The Mountain and Road lines consistently deliver the strongest revenue and profit outcomes and should receive priority investment in marketing, product development, and inventory management. Expanding these lines — particularly Mountain-200 variants which dominate the top 10 revenue rankings — could further increase overall profitability.

### 3. Review Underperforming Product Lines
The Standard and Touring product lines generate significantly lower revenue and profit relative to their cost base. A strategic review should determine whether repositioning, pricing adjustment, cost reduction, or discontinuation is the most appropriate course of action for these lines.

### 4. Investigate and Address the Post-2013 Revenue Decline
The decline in revenue observed after 2013 requires thorough investigation. Possible contributing factors include increased market competition, shifts in consumer preferences, pricing adjustments, or customer retention challenges. Understanding the root cause is critical to preventing further revenue erosion.

### 5. Strengthen the US and Southwest Strategy
Given that the United States accounts for approximately 57% of total revenue and the Southwest leads in both units sold and revenue, these markets should remain strategic priorities. Continued investment in distribution, marketing, and customer retention in these regions is essential to protecting the company's core revenue base.

### 6. Replicate Australia's Profitability Model
Australia generated the highest profit of all territories at $3.43M despite ranking third in revenue. The operational practices, pricing strategies, and cost structures that drive this efficiency should be analysed and replicated in other markets, particularly those currently experiencing losses.

### 7. Resolve Negative-Profit Territories
The Central, Southeast, and Northeast territories are operating at a loss. A targeted review of pricing, logistics costs, customer demand patterns, and market sizing in these regions should be conducted to restore profitability or inform exit decisions where appropriate.

---

## Conclusion

This analysis of AdventureWorks Cycles sales data (2011–2014) delivered a comprehensive view of company performance across revenue, profitability, product, and territory dimensions. The Bikes category — particularly Mountain and Road product lines — emerged as the clear commercial backbone of the business, generating over 86% of total revenue. The United States and Southwest territory are dominant markets, while Australia demonstrates the strongest profitability per revenue generated.

Despite a modest overall profit margin of 9%, the positive trend in margin improvement from 2013 to 2014 suggests that the business was becoming more operationally efficient. Addressing the post-2013 revenue decline, resolving loss-making territories, and strategically managing the underperforming product lines represent the most impactful near-term opportunities for sustainable growth.

From a technical standpoint, this project demonstrates proficiency in the full Power BI workflow — from raw data import and transformation in Power Query, through relational data modeling and DAX measure development, to the creation of interactive, executive-ready dashboards. It reflects an ability to not only build technically sound analytical solutions, but to translate data findings into structured, actionable business narratives — a skill that sits at the core of effective data and business analysis.

---

## Project Files

| File | Description |
|---|---|
| [View Sales Report (PDF)](./AdventureWorks_Sales_Report.pdf) | Full analytical report including methodology, key insights, and strategic recommendations (2 pages) |
| [Dashboard Screenshot — Sales Overview](./dashboard_sales_overview.png) | Sales overview dashboard showing revenue, units sold, orders, and customer KPIs |
| [Dashboard Screenshot — Cost & Profit](./dashboard_cost_profit.png) | Cost and profit analysis dashboard by product line and year |
| [Dashboard Screenshot — Product Performance](./dashboard_product_performance.png) | Product category, subcategory, and top 10 product revenue dashboard |
| [Dashboard Screenshot — Territory Analysis](./dashboard_territory.png) | Territory and country-level revenue, profit, and orders dashboard |

---

## Contact Information
* Email: aladeloyeesther616@gmail.com
* LinkedIn: [linkedin.com/in/estheraderonke](https://linkedin.com/in/estheraderonke)

## Introduction
This project was completed as part of the AltSchool Africa School of Data — Semester 3 assessment, and represents a demonstration of end-to-end business intelligence skills using Power BI. It covers the full analytical workflow from raw data import and transformation through to data modeling, DAX measure creation, interactive dashboard development, and strategic business reporting.

The AdventureWorks dataset — a widely used Microsoft sample database representing a fictional bicycle manufacturing and retail company — was selected as the foundation for this analysis. The project simulates a real-world business intelligence engagement where a data analyst is tasked with evaluating company performance and delivering actionable strategic insights to leadership.

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

- Evaluate total revenue, cost, profit, and profitability trends across a four-year period (2011–2014)
- Identify top-performing and underperforming product lines, categories, and subcategories
- Analyse sales performance and profitability across global territories and markets
- Uncover patterns in order volume, customer behaviour, and revenue concentration
- Develop interactive, executive-level dashboards that communicate findings clearly and support data-driven decision-making
- Deliver structured strategic recommendations backed by data to guide business growth

---

## About the Dataset

The AdventureWorks dataset is a relational Microsoft SQL Server sample database representing the operations of a fictional bicycle manufacturer. It contains sales transactions, product information, customer records, and territory data spanning the period from 2011 to 2014.

The following tables were imported and used in the analysis:

- **Sales.SalesOrderDetail** — Line-item detail for each sales transaction, including product, quantity, and unit price
- **Sales.SalesOrderHeader** — Order-level information including order date, customer ID, and territory ID
- **Production.Product** — Product master data including product name, class, style, product line, and standard cost
- **Production.ProductSubcategory** — Subcategory classification for each product (e.g. Mountain Bikes, Road Bikes, Helmets)
- **Production.ProductCategory** — High-level category grouping (Bikes, Components, Clothing, Accessories)
- **Sales.Customer** — Customer identifiers linking sales transactions to individual customers
- **Person.Person** — Personal details associated with each customer record
- **Sales.SalesTerritory** — Territory names, regional groupings, and country assignments

**Dataset Summary:**
- Total Orders: 31,465
- Total Units Sold: 275,000
- Total Revenue: $109.85M
- Total Cost: $100.47M
- Total Profit: $9.37M
- Overall Profit Margin: 9%
- Total Customers: 19,820
- Time Period: 2011 – 2014
- Markets: United States, Canada, Australia, United Kingdom, France, Germany

**Key Assumptions:**
- StandardCost was used as the cost of goods sold, as a detailed operational cost breakdown was not available in the dataset
- Missing categorical values replaced with "Unknown" were assumed not to significantly distort product or customer segmentation
- OrderDate was used as the point of revenue recognition for all time-series and trend analysis

---

## Tools Used

- **Power BI Desktop** — Primary tool for data import, transformation, modeling, measure creation, and dashboard development
  - **Power Query Editor** — Data cleaning, transformation, and preparation
  - **DAX (Data Analysis Expressions)** — Creation of calculated measures and KPIs
  - **Data Model View** — Relationship management and schema design
  - **Report View** — Interactive dashboard and visualisation development

---

## Importing the Dataset

The AdventureWorks dataset was imported into Power BI Desktop by connecting to the SQL Server database. The following eight tables were selected and loaded into the Power BI environment for transformation and analysis:

Sales.SalesOrderDetail, Sales.SalesOrderHeader, Production.Product, Production.ProductSubcategory, Production.ProductCategory, Sales.Customer, Person.Person, and Sales.SalesTerritory.

Each table was loaded into Power Query Editor as a separate query before cleaning and transformation were applied. After all transformations were completed, the Close & Apply function was used to load the cleaned tables into the Power BI data model.

---

## Data Cleaning and Transformation

All data cleaning and transformation steps were performed in Power Query Editor before loading the data into the model. The following transformations were applied:

**Data Type Corrections**
Column data types were reviewed and corrected where necessary. Notably, ID columns that were incorrectly stored as date format were converted to text to prevent unintended aggregations and ensure accurate relationship matching.

**Categorical Field Transformation**
Several categorical fields contained abbreviated codes that were not meaningful in a business context. These were transformed into readable labels to improve interpretability across all dashboards:
- Product Class: L → Low, M → Medium, H → High
- Product Line: M → Mountain, R → Road, S → Standard, T → Touring
- Product Style: W → Women's, M → Men's, U → Universal

**Handling Null and Missing Values**
All columns were reviewed for null and missing values. Where missing values appeared in categorical fields — such as product line or product style — they were replaced with "Unknown" to prevent null values from being excluded from aggregations or distorting visual outputs. Columns with excessive null values that were not relevant to the analysis were removed entirely.

**Outcome**
After completing all transformation steps, the cleaned tables were loaded into the Power BI data model using the Close & Apply function, ready for relationship building and measure creation.

---

## Data Modeling

Following data import and cleaning, a relational data model was constructed in Power BI's Model View. Relationships between tables were established using primary and foreign keys to enable accurate cross-table analysis.

The following relationships were configured, all set as **one-to-many**:

| From Table | Key | To Table | Key |
|---|---|---|---|
| Sales.SalesOrderDetail | ProductID | Production.Product | ProductID |
| Sales.SalesOrderHeader | CustomerID | Sales.Customer | CustomerID |
| Sales.SalesOrderHeader | TerritoryID | Sales.SalesTerritory | TerritoryID |
| Production.Product | ProductSubcategoryID | Production.ProductSubcategory | ProductSubcategoryID |
| Production.ProductSubcategory | ProductCategoryID | Production.ProductCategory | ProductCategoryID |
| Sales.Customer | PersonID | Person.Person | BusinessEntityID |

This star-schema-style model ensured that all dimensions (products, customers, territories) were correctly linked to the central sales fact tables, enabling accurate slicing and filtering across all dashboard pages.

---

## Data Analysis

With the data model in place, the following core DAX measures were created to serve as the analytical foundation for all dashboard visualisations:

```dax
Total Revenue = SUMX(Sales_SalesOrderDetail, Sales_SalesOrderDetail[OrderQty] * Sales_SalesOrderDetail[UnitPrice])

Total Cost = SUMX(Sales_SalesOrderDetail, Sales_SalesOrderDetail[OrderQty] * RELATED(Production_Product[StandardCost]))

Total Profit = [Total Revenue] - [Total Cost]

Profit Margin (%) = DIVIDE([Total Profit], [Total Revenue], 0)

Total Orders = DISTINCTCOUNT(Sales_SalesOrderHeader[SalesOrderID])
```

The analysis was then structured across four key dimensions, each forming the basis of a dedicated dashboard page:

**1. Sales Overview**
Revenue, quantity sold, total orders, and customer count were evaluated across the full time period. Trends were analysed by year, product class, and product style to understand what was driving volume and revenue growth.

**2. Cost and Profit Analysis**
Total cost and profit were broken down by product line and tracked over time to assess profitability trends. The profit margin by year was examined to determine whether the business was becoming more or less efficient at converting revenue into profit.

**3. Product Performance**
Revenue and profit were analysed across product categories, subcategories, and product lines. The top 10 products by revenue and cost were identified to highlight where performance was concentrated.

**4. Territory Analysis**
Revenue, profit, total orders, and units sold were compared across all territories and countries to identify the strongest and weakest performing markets.

---

## Data Visualization

Four interactive Power BI dashboard pages were developed, each addressing one analytical dimension. The following visualisation types were used across the dashboards:

- **KPI Cards** — for at-a-glance display of Total Revenue, Total Cost, Total Profit, Profit Margin, Total Orders, Total Quantity Sold, and Total Customers
- **Line Charts** — for year-on-year trend analysis of revenue, cost, profit, and profit margin
- **Bar Charts** — for product line, subcategory, and territory performance comparisons
- **Donut Charts** — for proportional breakdowns of revenue and quantity by product class, style, and category
- **Scatter Plot** — for visualising the relationship between order quantity and revenue
- **Map Visual** — for geographic revenue distribution across territories
- **Horizontal Bar Charts** — for top 10 product rankings by revenue and cost

All dashboards were designed with a consistent navy blue colour theme, clear labelling, and intuitive layout to support executive-level readability and decision-making.

> 📁 Dashboard screenshots are available in the repository files below.

---

## Key Insights

### 1. Overall Sales Performance
The company generated **$109.85M in total revenue** from **275,000 units sold** across **31,465 orders**, serving approximately **19,820 customers** between 2011 and 2014. Revenue and sales volume showed consistent growth from 2011 through 2013, peaking in 2013 before declining in 2014. This pattern suggests strong early expansion followed by a slowdown that may reflect market saturation, increased competition, or internal operational challenges.

Universal style products accounted for the highest quantity sold at **56.24%**, indicating that broad-appeal products drive volume. High-class products generated the highest revenue share at **64.27% ($70.59M)**, confirming that premium products contribute disproportionately to revenue despite lower unit volumes.

### 2. Cost and Profit Analysis
Total cost stood at **$100.47M**, resulting in a **total profit of $9.37M** and an **overall profit margin of 9%**. While the company is profitable, the thin margin indicates limited efficiency in either cost management or pricing strategy, with a significant proportion of revenue absorbed by production and operational costs.

However, the profit margin by year tells an encouraging story. After dipping in 2012, profit margin recovered and reached its highest point in 2014 — even as overall sales declined. This indicates that the company became increasingly efficient at converting revenue into profit, demonstrating meaningful operational improvements in its later years. The Mountain product line was the most profitable at **$5.63M**, followed by Road at **$2.82M**.

### 3. Product Performance
The **Bikes category dominated revenue generation**, contributing **$94.65M (86.17%)** of total revenue. Within this category, **Mountain Bikes were the most profitable subcategory at $4.91M**, followed by Road Bikes at **$2.81M**. The Road product line led in total revenue at **$48.30M**, while Mountain followed closely at **$42.46M**.

The top 10 products by revenue were dominated by Mountain-200 variants, with the highest single product generating **$4.40M**. This concentration of performance among a small group of products highlights both a strength — clear product-market fit — and a risk — heavy dependence on a limited product range.

The Standard and Touring product lines contributed significantly lower revenue and profit, suggesting they may not be justifying their place in the portfolio without strategic repositioning.

### 4. Territory Analysis
The **United States was the primary market**, generating **$63.00M** in revenue — approximately 57% of total company revenue. The **Southwest territory led in total units sold (59,110)** and was the highest revenue territory at **$24.18M**, while **Australia generated the highest profit among all territories at $3.43M** — indicating exceptional cost efficiency relative to revenue in that market.

The **Southwest, Northwest, and Canada** territories were the strongest performers across both revenue and order volume. Three territories — **Central, Southeast, and Northeast** — recorded **negative profit**, with Northeast posting the worst loss at **-$0.27M**, pointing to uneven regional performance and potential operational or pricing inefficiencies in these markets.

---

## Recommendations

### 1. Improve Profit Margins Through Cost Optimisation
With an overall profit margin of 9%, there is significant room for improvement. A detailed cost review should be conducted across production, distribution, and regional operations to identify inefficiencies. Pricing strategies for high-volume products — particularly Road Bikes — should be reassessed to improve margin performance without negatively impacting demand.

### 2. Prioritise the Mountain and Road Product Lines
The Mountain and Road lines consistently deliver the strongest revenue and profit outcomes and should receive priority investment in marketing, product development, and inventory management. Expanding these lines — particularly Mountain-200 variants which dominate the top 10 revenue rankings — could further increase overall profitability.

### 3. Review Underperforming Product Lines
The Standard and Touring product lines generate significantly lower revenue and profit relative to their cost base. A strategic review should determine whether repositioning, pricing adjustment, cost reduction, or discontinuation is the most appropriate course of action for these lines.

### 4. Investigate and Address the Post-2013 Revenue Decline
The decline in revenue observed after 2013 requires thorough investigation. Possible contributing factors include increased market competition, shifts in consumer preferences, pricing adjustments, or customer retention challenges. Understanding the root cause is critical to preventing further revenue erosion.

### 5. Strengthen the US and Southwest Strategy
Given that the United States accounts for approximately 57% of total revenue and the Southwest leads in both units sold and revenue, these markets should remain strategic priorities. Continued investment in distribution, marketing, and customer retention in these regions is essential to protecting the company's core revenue base.

### 6. Replicate Australia's Profitability Model
Australia generated the highest profit of all territories at $3.43M despite ranking third in revenue. The operational practices, pricing strategies, and cost structures that drive this efficiency should be analysed and replicated in other markets, particularly those currently experiencing losses.

### 7. Resolve Negative-Profit Territories
The Central, Southeast, and Northeast territories are operating at a loss. A targeted review of pricing, logistics costs, customer demand patterns, and market sizing in these regions should be conducted to restore profitability or inform exit decisions where appropriate.

---

## Conclusion

This analysis of AdventureWorks Cycles sales data (2011–2014) delivered a comprehensive view of company performance across revenue, profitability, product, and territory dimensions. The Bikes category — particularly Mountain and Road product lines — emerged as the clear commercial backbone of the business, generating over 86% of total revenue. The United States and Southwest territory are dominant markets, while Australia demonstrates the strongest profitability per revenue generated.

Despite a modest overall profit margin of 9%, the positive trend in margin improvement from 2013 to 2014 suggests that the business was becoming more operationally efficient. Addressing the post-2013 revenue decline, resolving loss-making territories, and strategically managing the underperforming product lines represent the most impactful near-term opportunities for sustainable growth.

From a technical standpoint, this project demonstrates proficiency in the full Power BI workflow — from raw data import and transformation in Power Query, through relational data modeling and DAX measure development, to the creation of interactive, executive-ready dashboards. It reflects an ability to not only build technically sound analytical solutions, but to translate data findings into structured, actionable business narratives — a skill that sits at the core of effective data and business analysis.
