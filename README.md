# Elevate-Customer-Satisfaction-Revolutionize-Supply-Chain-with-SQL-Driven-Inventory-Optimization
![clay-banks-Ox6SW103KtM-unsplash](https://github.com/user-attachments/assets/a0cb9a77-65c7-4e24-a823-57019b61d157)
## Table Of Content
- [Project Overview](#project-overview)
- [Importance of Inventory Optimization](#importance-of-inventory-optimization)
- [Aim of the Project](#aim-of-the-project)
- [Data Description](#data-description)
- [Tech Stack](#tech-stack)
- [Project Scope](#project-scope)
- [Code Section](#code-section)
- [Feedback Loop](#feedback-loop)
- [General insights](general-insights)
- [Recommendations](#recommendations)



## Project Overview
### Inventory Management Challenges:
TechElectro Inc. faces a series of intricate inventory management challenges that impede its operational efficiency and customer satisfaction:
 
### A. Overstocking: 
The company frequently finds itself burdened with excessive inventory of certain products, resulting in substantial capital tied up in unsold goods and limited storage capacity.
### B. Understocking: 
Conversely, high-demand products regularly suffer from stockouts, leading to missed sales opportunities and irate customers unable to access their desired items.
### C. Customer Satisfaction: 
These inventory-related issues have a direct and detrimental effect on customer satisfaction and loyalty. Customers endure delays, frequent stockouts, and frustration when they cannot find the products they seek.

## Rationale for the Project
Inventory optimization refers to the process of efficiently managing a company's inventory to strike the right balance between supply and demand. The goal is to minimize carrying costs while ensuring that products are readily available to meet customer needs. Transforming customer satisfaction through SQL-powered inventory optimization is a strategic approach that uses SQL (Structured Query Language) and data analysis techniques to enhance customer satisfaction by efficiently managing inventory.
 
## Importance of Inventory Optimization
- Implementing a comprehensive inventory optimization system powered by MySQL is imperative for TechElectro Inc. due to several compelling reasons
- Cost Reduction: Efficient inventory management through MySQL can significantly reduce carrying costs associated with overstocked items, freeing up capital for strategic investments.
- Enhanced Customer Satisfaction: By maintaining optimal inventory levels, TechElectro Inc. ensures that its products are readily available, elevating the overall customer experience and fostering loyalty.
- Competitive Advantage: Streamlined inventory management empowers TechElectro Inc. to respond swiftly to market fluctuations and shifting customer demands, providing a competitive edge.
- Profitability: Improved inventory control through MySQL optimization leads to reduced waste and improved cash flow, directly impacting profitability.
## Aim of the Project
The primary objectives of this project are to implement a sophisticated inventory optimization system utilizing MySQL and address the identified business challenges effectively. The project aims to achieve the following goals:
 
- Optimal Inventory Levels: Utilize MySQL optimization techniques to determine the optimal stock levels for each product SKU, thereby minimizing overstock and understock situations.
  
 - Data-Driven Decisions: Enable data-driven decision-making in inventory management by leveraging MySQL analytics to reduce costs and enhance customer satisfaction.

## Data Description

I will be working with 3 datasets and they are as follows:

### Sales Data
✓ Product ID: Unique product identifier.
✓ Sales Date: Date of product sale (Date)
✓ Sales Quantity (Units): Number of units sold (Units).
✓ Product Cost (USD per Unit): Cost per product unit in USD (USD per Unit).
 

### Product Information Data

✓ Product ID: Unique product identifier.
✓ Product Category: Product type.
✓ Promotions: Indicator of promotions.
 
### External Information Data
✓ Sales Date: Date of product sale (Date)
✓ GDP (Gross Domestic Product) (USD): Economic data in USD (USD).
✓ Inflation Rate (%): Percentage change in prices.
✓ Seasonal Factor (Dimensionless): Index for seasonal effects.

## Tech Stack
Tool– MySQL will be used for:

- For performing mathematical operations over data

- For Data Analysis and Manipulation

## Project Scope
### A. Exploratory Data Analysis (EDA):
Leverage MySQL for EDA, conducting advanced analytics and statistical analysis to explore data patterns, correlations, and descriptive statistics without relying on data visualization.
 
### B. Optimal Inventory Levels: 
Utilize MySQL optimization techniques and algorithms to determine optimal inventory levels for each product SKU.
 
### C. Documentation and Recommendations:
Develop comprehensive documentation of the project, encompassing MySQL scripts, methodologies, and user guides.
 
### D. Deployment: 
Deploy the MySQL-powered inventory optimization system, ensuring seamless integration with TechElectro Inc.'s existing supply chain management systems.
 
### E. Exploratory Data Analysis: 
Explore the data to understand its characteristics and discover patterns.
 
### F. Data Transformation: 
Prepare the data for analysis by transforming, encoding, or normalizing it.
 
### G. Data Analysis: 
Analyze data to understand pattern in order to generate insights that will be visualized.
 
### H. Data Visualization:
Create visual representations to communicate insights effectively.
 
### I. Interpretation and Insight Generation:
Extract meaningful insights and interpret the results.

## Code Section
```sql
-- preliminaries- creation of schema/Database
CREATE SCHEMA tech_electro;
USE tech_electro;
```
```sql
-- Data Exploration
SHOW TABLES;
SELECT * FROM product_information;
SELECT * FROM external_factors;
SELECT * FROM sales_data;
```
```sql
-- Understanding the structure of the data sets 
SHOW COLUMNS FROM external_factors;
DESCRIBE product_information;
DESC sales_data;
```
```sql
-- DATA CLEANING
-- Changing to the right data type for all columns 
-- external factors 
-- Sales Date DATE, GDP DECIMAL (15,2),InflationRate Decimal 
-- (5,2), SeasonalFcator Decimal (5,2)
```
```sql
-- SET SQL_SAFE_UPDATES = 0; ---tirning off safe updates
```
```sql
ALTER TABLE external_factors
ADD COLUMN New_Sales_Date DATE;
```
```sql
UPDATE external_factors
SET New_Sales_Date = CASE
    WHEN Sales_Date LIKE '__/__/____' THEN DATE_FORMAT(STR_TO_DATE(Sales_Date, '%d/%m/%Y'), '%Y-%m-%d')
    WHEN Sales_Date LIKE '__/_/____' THEN DATE_FORMAT(STR_TO_DATE(Sales_Date, '%d/%c/%Y'), '%Y-%m-%d')
    WHEN Sales_Date LIKE '_/__/____' THEN DATE_FORMAT(STR_TO_DATE(Sales_Date, '%e/%m/%Y'), '%Y-%m-%d')
    WHEN Sales_Date LIKE '_/_/____' THEN DATE_FORMAT(STR_TO_DATE(Sales_Date, '%e/%c/%Y'), '%Y-%m-%d')
    ELSE NULL  -- Handle any unexpected formats or NULL values
END;
```
```sql
ALTER TABLE external_factors
DROP Sales_Date;
```
```sql
ALTER TABLE external_factors
RENAME COLUMN New_Sales_Date TO Sales_Date;
```
```sql
-- GDP column
ALTER TABLE external_factors
MODIFY GDP DECIMAL (15,2);
```
```sql
-- InflationRate Column 
ALTER TABLE external_factors
MODIFY `Inflation Rate` Decimal (5,2);
```
```sql
-- SeasonalFcator Column
ALTER TABLE external_factors 
MODIFY `Seasonal Factor` Decimal (5,2);
```
```sql
SHOW COLUMNS FROM external_factors;
```
```sql
-- Product_information
-- Product ID INT NOT NULL, Product Category TEXT, Promotions ("yes","no")
```
```sql
SHOW COLUMNS FROM product_information;
```
```sql
ALTER TABLE product_information
ADD COLUMN New_Promotions ENUM('Yes','No');
```
```sql
UPDATE product_information
SET New_Promotions = CASE
	WHEN LTRIM(RTRIM(Promotions)) = 'Yes' THEN 'Yes'
	WHEN LTRIM(RTRIM(Promotions)) = 'No'  THEN 'No'
	ELSE NULL
END;
```
```sql
ALTER TABLE product_information
DROP Promotions;
```
```sql
ALTER TABLE product_information
RENAME COLUMN New_Promotions TO Promotions;
```
```sql
DESCRIBE product_information;
```
```sql
-- sales_data
ALTER TABLE sales_data
ADD COLUMN New_Sales_Date DATE;
```
```sql
UPDATE sales_data
SET New_Sales_Date = CASE
    WHEN `Sales Date` LIKE '__/__/____' THEN DATE_FORMAT(STR_TO_DATE(`Sales Date`, '%d/%m/%Y'), '%Y-%m-%d')
    WHEN `Sales Date` LIKE '__/_/____' THEN DATE_FORMAT(STR_TO_DATE(`Sales Date`, '%d/%c/%Y'), '%Y-%m-%d')
    WHEN `Sales Date` LIKE '_/__/____' THEN DATE_FORMAT(STR_TO_DATE(`Sales Date`, '%e/%m/%Y'), '%Y-%m-%d')
    WHEN `Sales Date` LIKE '_/_/____' THEN DATE_FORMAT(STR_TO_DATE(`Sales Date`, '%e/%c/%Y'), '%Y-%m-%d')
    ELSE NULL  -- Handle any unexpected formats or NULL values
END;
```
```sql
ALTER TABLE sales_data
DROP `Sales Date`;
```
```sql
ALTER TABLE sales_data
RENAME COLUMN New_Sales_Date TO Sales_Date;
```
```sql
SHOW COLUMNS FROM sales_data;
```
```sql
-- Identifying missing values using the 'ISNULL' Functions
-- external_factors
```
```sql
SELECT 
	SUM(CASE WHEN Sales_Date IS NULL THEN 1 ELSE 0 END) AS missing_Sales_Date,
    SUM(CASE WHEN GDP IS NULL THEN 1 ELSE 0 END) AS missing_GDP,
    SUM(CASE WHEN `Inflation Rate` IS NULL THEN 1 ELSE 0 END) AS `missing_Inflation Rate`,
    SUM(CASE WHEN `Seasonal Factor` IS NULL THEN 1 ELSE 0 END) AS `missing_Seasonal Factor`
FROM 
	external_factors;
```
```sql
-- product_information
SELECT 
	SUM(CASE WHEN `Product ID` IS NULL THEN 1 ELSE 0 END) AS `missing_Product ID`,
    SUM(CASE WHEN `Product Category` IS NULL THEN 1 ELSE 0 END) AS `missing_Product Category`,
    SUM(CASE WHEN Promotions IS NULL THEN 1 ELSE 0 END) AS `missing_Promotions`
FROM 
	product_information;
```
```sql
-- sales_data

SELECT 
	SUM(CASE WHEN `Product ID` IS NULL THEN 1 ELSE 0 END) AS `missing_Product ID`,
    SUM(CASE WHEN `Sales_Date` IS NULL THEN 1 ELSE 0 END) AS `missing_Sales_Date`,
    SUM(CASE WHEN `Inventory Quantity` IS NULL THEN 1 ELSE 0 END) AS `missing_Inventory Quantity`,
    SUM(CASE WHEN `Product Cost` IS NULL THEN 1 ELSE 0 END) AS `missing_Product Cost`
FROM 
	sales_data;
```
```sql
 -- Check for duplicates using "GROUP BY" and "HAVING"  clauses and remove them if necessary

-- external_factors
SELECT 
	Sales_Date,
	COUNT(*) AS Count
FROM
	external_factors
GROUP BY
	Sales_Date
HAVING 
	Count > 1;
```
```sql
 -- Number of duplicates in external_factors 
  SELECT 
	Count(*)
FROM 
	(SELECT 
	Sales_Date,
	COUNT(*) AS Count
FROM
	external_factors
GROUP BY
	Sales_Date
HAVING 
	Count > 1) AS Dup;
```
```sql
-- product_information    
SELECT 
	`Product ID`,
	COUNT(*) AS Count
FROM
	product_information
GROUP BY
	`Product ID`
HAVING 
	Count > 1;
```
```sql
-- Number of duplicates in product_information
SELECT 
	Count(*)
FROM 
	(SELECT 
	`Product ID`,
	COUNT(*) AS Count
FROM
	product_information
GROUP BY
	`Product ID`
HAVING 
	Count > 1) AS Dup;
```
```sql
-- sales_data
SELECT 
	Sales_Date,
	COUNT(*) AS Count
FROM
	sales_data
GROUP BY
	Sales_Date
HAVING 
	Count > 1;
```
```sql
 -- Number of duplicates in sales_data
    
SELECT 
	COUNT(*)
FROM
	(SELECT 
	Sales_Date,
	COUNT(*) AS Count
FROM
	sales_data
GROUP BY
	Sales_Date
HAVING 
	Count > 1) AS Dup;
```
```sql
-- Dealing with duplicates for external factors and products_data
-- external factors  

DELETE e1 FROM external_factors e1
INNER JOIN (
	SELECT Sales_Date,
ROW_NUMBER() OVER (PARTITION BY Sales_Date) AS rn
FROM external_factors
) e2 ON e1.Sales_Date = e2.Sales_Date
WHERE e2.rn > 1;
```
```sql
-- product_information
DELETE p1 FROM product_information p1
INNER JOIN (
	SELECT `Product ID`,
ROW_NUMBER() OVER (PARTITION BY `Product ID` ORDER BY `Product ID` ) AS rn
FROM product_information
) p2 ON p1.`Product ID` = p2.`Product ID`
WHERE p2.rn > 1;
```
```sql
-- DATA INTERGRATION
--  sales_data and product_information first

CREATE VIEW Sales_product_data AS
SELECT
	s.`Product ID`,
    s.`Inventory Quantity`,
    s.`Product Cost`,
    s.Sales_Date,
    p.`Product Category`,
    p.Promotions
FROM 
    sales_data AS s
JOIN 
	product_information AS p
ON 
	s.`Product ID`= p.`Product ID`;
```
```sql
-- sales_data and external_factors

CREATE VIEW Inventory_data AS 
SELECT
	sales_data.`Product ID`,
    sales_data.`Inventory Quantity`,
    sales_data.`Product Cost`,
    sales_data.Sales_Date,
	product_information.`Product Category`,
	product_information.Promotions,
    external_factors.GDP,
    external_factors.`Inflation Rate`,
    external_factors.`Seasonal Factor`
FROM 
	sales_data 
JOIN 
	 product_information 
ON 
	sales_data.`Product ID`= product_information.`Product ID`
JOIN
	external_factors 
ON
	sales_data.Sales_Date = external_factors .Sales_Date;
```
```sql
-- Descriptive Analysis
-- Basic statistics:
-- Average sales (calculated as the product of "inventory quantity" and "product cost").alter

SELECT 
	`Product ID`,
    ROUND(AVG(`Inventory Quantity` * `Product Cost`),2) AS avg_sales
FROM 
	inventory_data
GROUP BY
	`Product ID`
ORDER BY 
    avg_sales DESC;
```
```sql
-- Median stock levels (i.e., "inventory quantity") 
SELECT
	`Product ID`, 
    AVG(`Inventory Quantity`) AS median_stock
FROM (
	SELECT 
		`Product ID`,
        `Inventory Quantity`,
ROW_NUMBER() OVER(PARTITION BY `Product ID`order by  `Inventory Quantity`) AS row_num_asc,
ROW_NUMBER() OVER(PARTITION BY `Product ID`order by  `Inventory Quantity`) AS row_num_desc
FROM
	inventory_data) AS subquery
WHERE row_num_asc IN (row_num_desc, row_num_desc - 1, row_num_desc + 1)
GROUP BY 
	`Product ID`;
```
```sql
-- Product perfromance metrics (total sales per product)
SELECT 
	`Product ID`,
    ROUND(SUM(`Inventory Quantity` * `Product Cost`)) AS total_sales
FROM 
	inventory_data
GROUP BY 
	`Product ID`
ORDER BY 
	total_sales DESC;
```
```sql
-- Identify high-demand products based on average sales
-- we want to check if there are any highly demanded products that wont have any stock 
-- in order to understand if there is understocking of highly demanded products 

WITH HighDemandProducts AS (
SELECT 
	`Product ID`, 
	AVG(`Inventory Quantity`) AS avg_sales
FROM 
	inventory_data
GROUP BY 
	`Product ID`
HAVING 
	avg_sales > (
					SELECT
							AVG(`Inventory Quantity`) * 0.95 
					FROM 
							sales_data
                            )
		)
-- calculate stockout frequency for high demand products
SELECT
	i.`Product ID`,
    COUNT(*) AS stockout_frequency
FROM 
	inventory_data AS i
WHERE
	i.`Product ID` IN (SELECT `Product ID` FROM HighDemandProducts)
    AND i.`Inventory Quantity` = 0
GROUP BY 
    i.`Product ID`;
 ```
```sql
-- none of the high demand products have experienced understocking which is a positive sign for inventory management 
-- Highly demanded products have been adequately stocked and have not placed stock out situations. 

-- Influence of external factors    
-- GDP   

SELECT 
	`Product ID`,
    AVG(CASE WHEN `GDP` > 0 THEN `Inventory Quantity` ELSE NULL END) AS avg_sales_positive_gdp,
    AVG(CASE WHEN `GDP` <= 0 THEN `Inventory Quantity` ELSE NULL END) AS avg_sales_non_positive_gdp
    FROM 
		inventory_data
	GROUP BY 
		`Product ID`
	HAVING
		avg_sales_positive_gdp IS NOT NULL;
```
```sql
--  inflation
    SELECT 
	`Product ID`,
    AVG(CASE WHEN `Inflation Rate` > 0 THEN `Inventory Quantity` ELSE NULL END) AS avg_sales_positive_Inflation_Rate,
    AVG(CASE WHEN `Inflation Rate` <= 0 THEN `Inventory Quantity` ELSE NULL END) AS avg_sales_non_positive_Inflation_Rate
    FROM 
		inventory_data
	GROUP BY 
		`Product ID`
	HAVING
		avg_sales_positive_Inflation_Rate IS NOT NULL;
```
```sql
-- OPITIMIZING INVENTORY 
-- Deterrmine the optimal reorder point for each product based on historical sales data and external factors.
-- Reorder point = lead time demand + safety stock
-- Lead time demand = Rolling average sales * Lead Time 
-- Reorder point = Rolling Average Sales * Lead Time + Z * Lead Time^-2 * standard deviation of demand 
-- Safety stock = z * Lead Time ^ -2 * standard deviation of demand 
-- z=1.645
-- A constant Lead time of 7 days for all products. 
-- We aim for a 95% service level.

WITH InventoryCalculations AS (
		SELECT 
			`Product ID`,
            AVG(rolling_avg_sales) AS avg_rolling_sales,
            AVG(rolling_variance) AS avg_rolling_variance
		FROM (
			SELECT
				`Product ID`,
                AVG(daily_sales) OVER (PARTITION BY `Product ID` ORDER BY Sales_Date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS rolling_avg_sales,
                AVG(squared_diff) OVER (PARTITION BY `Product ID` ORDER BY Sales_Date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS rolling_variance
			FROM (
				SELECT 
					`Product ID`,
                    Sales_Date,
                    `Inventory Quantity` * `Product Cost` AS daily_sales,
                    (`Inventory Quantity` * `Product Cost` - AVG(`Inventory Quantity` * `Product Cost`) OVER (PARTITION BY `Product ID` ORDER BY Sales_Date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW))
                    * (`Inventory Quantity` * `Product Cost` - AVG(`Inventory Quantity` * `Product Cost`) OVER (PARTITION BY `Product ID` ORDER BY Sales_Date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)) AS squared_diff
                    FROM
						inventory_data
                        )subquery
                        )subquery2
							GROUP BY
								`Product ID`
					)
                    SELECT 
						`Product ID`,
                        avg_rolling_sales * 7 AS lead_time_demand,   
                        1.645 * (avg_rolling_sales * 7) AS safety_stock,
                         (avg_rolling_sales * 7) + (1.645 * (avg_rolling_sales * 7)) AS reorder_point
					FROM
						InventoryCalculations;
```
```sql
-- Create the inventory_optimization table
CREATE TABLE inventory_optimization (
	`Product ID` INT,
    Reorder_point DOUBLE
);
```
```sql
-- Step 2: create the stored procedure to recalculate reorder point 
DELIMITER //
CREATE PROCEDURE RecalculateReorderPoint(productID INT)
BEGIN
	DECLARE avgRollingSales DOUBLE;
    DECLARE avgRollingVariance DOUBLE;
    DECLARE leadTimeDemand DOUBLE;
    DECLARE safetyStock DOUBLE;
    DECLARE reorderPoint DOUBLE;
SELECT 
            AVG(rolling_avg_sales),
            AVG(rolling_variance)
INTO
			avgRollingSales, 
            avgRollingVariance
		FROM (
			SELECT
				`Product ID`,
                AVG(daily_sales) OVER (PARTITION BY `Product ID` ORDER BY Sales_Date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS rolling_avg_sales,
                AVG(squared_diff) OVER (PARTITION BY `Product ID` ORDER BY Sales_Date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS rolling_variance
			FROM (
				SELECT 
					`Product ID`,
                    Sales_Date,
                    `Inventory Quantity` * `Product Cost` AS daily_sales,
                    (`Inventory Quantity` * `Product Cost` - AVG(`Inventory Quantity` * `Product Cost`) OVER (PARTITION BY `Product ID` ORDER BY Sales_Date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW))
                    * (`Inventory Quantity` * `Product Cost` - AVG(`Inventory Quantity` * `Product Cost`) OVER (PARTITION BY `Product ID` ORDER BY Sales_Date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)) AS squared_diff
                    FROM
						inventory_data
                        ) InnerDerived
                        ) OuterDerived;
					SET leadTimeDemand = avgRollingSales * 7;
					SET safetyStock = 1.645 * SQRT(avgRollingVariance * 7);		
					SET reorderPoint = leadTimeDemand + safetyStock;
INSERT INTO inventory_optimization (product_ID, Reorder_Point)
VALUES (productID, reorderPoint)
ON DUPLICATE KEY UPDATE Reorder_Point = reorderPoint;
END//
DELIMITER ;
```
```sql
-- Step 3: make inventory_data a permanent table
CREATE TABLE Inventory_table AS 
SELECT
		*
FROM
		Inventory_data;
```
```sql
-- Step 4 Create the Trigger
DELIMITER //
CREATE TRIGGER AfterInsertUnifiedTable
AFTER INSERT ON inventory_table
FOR EACH ROW
BEGIN 
	CALL RecalculateReorderPoint(NEW.`Product ID`);
END //
DELIMITER ;
```
```sql
-- OVERSTOCKING AND UNDERSTOCKING 
WITH RollingSales AS(
	SELECT
		`Product ID`,
        Sales_Date,
		AVG(`Inventory Quantity` * `Product Cost`) OVER (PARTITION BY `Product ID` ORDER BY  Sales_Date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS rolling_avg_sales
	FROM 
			inventory_table
		),
	-- Calculate the number of days a product was out of stock
    StockoutDays AS (
		SELECT
			`Product ID`,
            COUNT(*) AS stockout_days
		FROM 
			inventory_table
		WHERE 
			`Inventory Quantity`= 0
		GROUP BY 
			`Product ID`
		)
        -- Join the above CTEs with the main table to get the results 
        SELECT
			f.`Product ID`,
            AVG(f.`Inventory Quantity` * f.`Product Cost`) AS avg_inventory_value,
            AVG(rs.rolling_avg_sales) AS avg_rolling_sales,
            COALESCE(sd.stockout_days,0) AS stockout_days
		FROM
			inventory_table f
		JOIN
			RollingSales rs
		ON
			f.`Product ID` = rs.`Product ID` AND f.Sales_Date = rs.Sales_Date
        LEFT JOIN
			StockoutDays sd 
		ON 
			f.`Product ID` = sd.`Product ID`
		GROUP BY 
			f.`Product ID`, sd.stockout_days;
```
```sql
-- We identify various obverstocked products, if the product inventory value remains constant and
--  matches its sales value, but the actual sales is zero or low it might suggest that the product is not being sold 
--  but is constantly stocked which results in overstocking 
-- Most of the business's products are being overstocked which is a concern for the business 
-- Since there no product in this dataset that has an inventory positive of zero despite having sales 
-- This means there are no identified stockouts based on the given data. 
```
```sql
-- Monitor and Adjust 
	DELIMITER //
CREATE PROCEDURE 
	MonitorInventoryLevels()
BEGIN
SELECT 
	`Product ID`,
    AVG(`Inventory Quantity`) AS AvgInventory
FROM 
	inventory_table
GROUP BY
	`Product ID`
ORDER BY
	AvgInventory DESC;
END//
DELIMITER ;
```
```sql
-- Monitor Sales Trends 
DELIMITER //
CREATE PROCEDURE MonitorSalesTrends()
BEGIN 
SELECT 
		`Product ID`,
        Sales_Date,
		AVG(`Inventory Quantity` * `Product Cost`) OVER (PARTITION BY `Product ID` ORDER BY  Sales_Date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS RollingAvgSales
	FROM 
			inventory_table
	ORDER BY
			`Product ID`,
			Sales_Date;
END //
DELIMITER ;
```
```sql
-- Monitor stockout frequencies 
 DELIMITER //
CREATE PROCEDURE MonitorStockouts()
BEGIN 
SELECT 
	`Product ID`,
    COUNT(*) AS StockoutDays
FROM 
	inventory_table
WHERE
	`Inventory Quantity` = 0
GROUP BY
	`Product ID`
ORDER BY
	StockoutDays DESC;
END //
DELIMITER ;
```

## Feedback Loop
### Feedback loop Establishment
- Feedback portal: develop an online platform for stakeholders to easily submit feedback on inventory performance and challenges 
- Review meetings: Organize periodic sessions to discuss inventory system performance and gather direct insights.
- System monitoring: use established SQL procedures to track system metrics, with deviations from expectations flagged for review.
  
### Refinement based on feedback
- Feedback Analysis: Regularly compile and scrutinize feedback to identify recurring themes or pressing issues. 
- Action Implementation: prioritize and act on the feedback to adjust reorder points, safety stock levels or overall processes.
- Change communication: Inform stakeholders about changes, underscoring the value of their feedback and ensuring transparency.
  
## General insights
- Inventory Discrepancies: The initial stage of analysis revealed significant discrepancies in inventory levels, with instances of both understocking and overstocking.
- This instances were contributing to capital insufficiencies and customer dissatisfaction. 
- Sales Trends and External Influences: The analysis indicated that sales trends were notably influenced by various external factors.
- Recognizing these patterns provides an opportunity to forcast demand more accurately.
- Suboptimal inventory levels: Through the inventory optimization analysis, it was evident that the existing inventory levels were not optimized for current sales trends.
- Products that had either overstocking or understocking issues were identified.

## Recommendations 
- Implement Dynamic Inventory Management: The company should move from a static to a dynamic inventory management system. 
- Adjsuting inventory levels based on real-time sales trends, seasonality and external factors 
- Optimize reorder points and safety stocks: Utilize the reorder points and safety stocks calculated during the analysis to minimize stockouts and reduce excess inventory.
- Regularly review these metrics to ensure they align with current market conditions.
- Enhance pricing strategies: conduct a through review of product pricing strategies, especially for products identified as unprofitable.
- Consider factors such as competitor pricing, market demand, and product acquisation costs

