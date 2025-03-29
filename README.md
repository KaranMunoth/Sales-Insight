# **AtliQ Hardware Sales Insights Dashboard**

## **Project Overview**
This project focuses on analyzing the sales performance of **AtliQ Hardware**, an India-based company supplying computer hardware and peripherals to various retailers across the country. The primary goal is to build a **Power BI dashboard** to provide real-time, data-driven insights for better decision-making.

## **Problem Statement**
AtliQ Hardware’s **Sales Director** struggles to track sales effectively in a dynamically growing market. The company's overall sales are declining, and insights from **Regional Managers (North, South, and Central India)** are mostly verbal, lacking data-backed proof. This lack of visibility into key trends makes strategic decision-making difficult.

### **Proposed Solution**
To address this issue, we aim to develop a **Power BI dashboard** that:
- Automates data collection and visualization
- Provides real-time sales insights
- Enables data-driven decision-making

---

## **Project Execution - AIMS Grid**
AIMS Grid is a project management framework consisting of four key components:

1. **Purpose**: Uncover sales insights that were previously hidden, automate data collection, and support informed decision-making.
2. **Stakeholders**:
   - Sales Director  
   - Marketing Team  
   - Customer Service Team  
   - Data and Analytics Team  
   - IT Department  
3. **End Result**: A fully automated dashboard providing quick and updated insights to support data-driven decision-making.
4. **Success Criteria**:
   - A dashboard displaying the latest sales order insights
   - A 10% reduction in total business costs through better decision-making
   - Elimination of manual data collection, saving 20% of business time

**(Add Flowchart of Project Execution Here)**

---

## **Data Analysis using MySQL**

### **Data Import**
1. The SQL database dump file (`db_dump.sql`) is used to import data into MySQL Workbench.
2. Data is loaded into MySQL for further analysis.

**(Add Screenshot of MySQL Data Import Here)**

### **Initial Data Exploration**
- Checking for anomalies in the dataset:
  ```sql
  SELECT * FROM sales.market;
  ```
  - Found incorrect values in the `market` table.
  ```sql
  SELECT * FROM sales.transactions;
  ```
  - Found negative sales amounts and transactions in USD that needed conversion.

---

## **SQL Queries for Sales Insights**

Below are SQL queries used to analyze sales data:

### **General Queries**
```sql
-- Retrieve all customer records
SELECT * FROM sales.customers;

-- Count total number of customers
SELECT count(*) FROM sales.customers;
```

### **Market-Specific Queries**
```sql
-- Transactions in Chennai Market (Market Code: Mark001)
SELECT * FROM sales.transactions WHERE market_code='Mark001';

-- Unique product codes sold in Chennai
SELECT DISTINCT product_code FROM sales.transactions WHERE market_code='Mark001';
```

### **Revenue Analysis**
```sql
-- Total revenue in 2020
SELECT SUM(sales.transactions.sales_amount)
FROM sales.transactions
INNER JOIN sales.date ON sales.transactions.order_date = sales.date.date
WHERE sales.date.year=2020;
```

---

## **Data Cleaning & ETL (Extract, Transform, Load)**

### **Steps Taken:**
1. **Connected MySQL database to Power BI**
2. **Loaded data into Power BI** to establish relationships and form a star schema

**(Add Screenshot of Star Schema in Power BI Here)**

3. **Data Transformation using Power Query**
   - **Filtering Market Table**: Removed null and incorrect values
   - **Filtering Transactions Table**: Eliminated negative and zero values
   - **Currency Conversion**: Converted USD to INR

```powerquery
=Table.AddColumn(#"Filtered Rows", "norm_sales_amount",
 each if [currency] = "USD" then [sales_amount]*75 else [sales_amount])
```

4. **Handled Duplicate Currency Values**
```sql
-- Count transactions in INR
SELECT count(*) FROM sales.transactions WHERE sales.transactions.currency="INR";

-- Count transactions in USD
SELECT count(*) FROM sales.transactions WHERE sales.transactions.currency="USD";
```
---

## **Data Modeling**
After cleaning and transforming the dataset, we structured it into a **data model** for seamless reporting.

**(Add Screenshot of Data Model Here)**

---

## **Data Analysis (DAX Queries in Power BI)**

### **Key Measures Used**
```dax
-- Profit Margin Percentage
Profit Margin % = DIVIDE([Total Profit Margin],[Revenue],0)

-- Revenue Calculation
Revenue = SUM('sales transactions'[sales_amount])

-- Revenue Comparison with Last Year
Revenue LY = CALCULATE([Revenue],SAMEPERIODLASTYEAR('sales date'[date]))
```

### **Profit Target Analysis**
```dax
-- Define Profit Target Range
Profit Target1 = GENERATESERIES(-0.05, 0.15, 0.01)

-- Compute Profit Target Value
Profit Target Value = SELECTEDVALUE('Profit Target1'[Profit Target])

-- Calculate Difference from Target
Target Diff = [Profit Margin %]-'Profit Target1'[Profit Target Value]
```

---

## **Dashboard & Visualizations**

### **Power BI Dashboard Highlights**
- **Real-time sales insights by region**
- **Profit margin analysis**
- **Comparison of sales trends over time**
- **Impact of data-driven decisions on revenue growth**

**(Add Screenshots of Power BI Dashboard Here)**

---

## **Conclusion**
This project successfully delivered a **comprehensive sales dashboard** for AtliQ Hardware, enabling the Sales Director to make **informed, data-driven decisions**. By automating data collection and visualization, the company can:
- Identify growth opportunities
- Reduce manual effort in sales tracking
- Optimize revenue and profit margins

