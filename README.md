# 📊 Unveiling Retail Insights: Analyzing 60,000+ Sales Records of an Electronics Superstore

![Screenshot (40)](https://github.com/user-attachments/assets/40fdfbd9-7aec-477e-a963-1ebd52e6bd40)


## 📌 Project Overview
This project focuses on analyzing sales data from an **Electronics Retail Superstore** that operates **physical stores across multiple countries** while also providing an **online shopping platform**. The Power BI dashboard developed for this project delivers deep insights into **top-performing products, revenue trends, store-wise sales, and online vs. offline purchase patterns
- ![Retail Analytics](https://img.shields.io/badge/domain-retail%20analytics-blue) 

---
## Business Problem 🚩
The electronics superstore operates across multiple locations and an online store, generating vast amounts of transactional data. However, several challenges impact business efficiency and profitability:  

- **Revenue Optimization:** The company lacks clear visibility into which products and locations contribute the most to revenue, making it difficult to allocate resources effectively.  
- **Customer Demand Trends:** Without data-driven insights, predicting customer preferences and demand fluctuations is challenging.  
- **Store Performance Analysis:** Comparing the performance of different stores and identifying underperforming locations is difficult due to fragmented data.  
- **Inventory Management:** Inefficient tracking of high-demand products and slow-moving inventory leads to lost sales opportunities and excess stock.  
- **Sales Channel Effectiveness:** Understanding the sales contribution from physical stores versus online channels is essential for strategic planning.  

By analyzing this retail sales dataset, the project aims to uncover meaningful insights that can help the business improve profitability, streamline operations, and enhance customer satisfaction.  
By leveraging **Power BI and Excel**, this analysis helps stakeholders understand key revenue drivers and optimize business strategies.

## Project Objective 🎯
The primary goals of this project include:
- **Identifying top revenue-generating products** across all sales channels.
- **Analyzing store-wise and country-wise sales performance** to determine profitable locations.
- **Comparing online vs. offline sales trends** to optimize inventory distribution.
- **Recognizing seasonal sales trends** and peak shopping periods.
- **Evaluating product categories contributing most to total revenue**.
---

### **Expected Outcomes**  
The expected outcome of this Project is to develop a well-structured retail sales database, generate insightful business intelligence through Power BI visualizations, and uncover key sales performance metrics—such as top-performing countries, best-selling products, revenue trends.

---

## 🛠️ Technologies Stack
- **Excel** - Initial data exploration and preprocessing.
- **Power BI** - Data Cleaning with Power Query, Data Modelling, DAX, Data visualization and interactive reporting.
- ![PowerBI](https://img.shields.io/badge/Power_BI-F2C811?logo=powerbi&logoColor=black) ![Excel](https://img.shields.io/badge/Excel-217346?logo=microsoft-excel&logoColor=white) ![DAX](https://img.shields.io/badge/DAX-F2C811?logo=powerbi&logoColor=black) ![Power Query](https://img.shields.io/badge/Power_Query-F2C811?logo=powerbi&logoColor=black)

---

### **Data Source**  
The dataset was sourced from **[Maven Analytics](https://mavenanalytics.io/data-playground?order=date_added%2Cdesc&search=electronic)** and including tables containing information about transactions, products, customers, stores.
- **Sales Transactions** – Records of all coffee sales.  
- **Stores** – Information on existing sales locations.  
- **Customers** – Demographic and behavioral data of consumers.  
- **Products** – Details on different product types and pricing.  

## 📊 Dataset Schema
| Table         | Description                                      |
|--------------|--------------------------------------------------|
| **Sales**    | Contains transactional sales data               |
| **Stores**   | Details of store locations and their countries  |
| **Products** | Information about products and their attributes |
| **Category** | Hierarchical classification of product types    |
| **Date** | Calendar date for time data analysis        |

---
## Project Structure

## 1. Data Preparation (Microsoft Excel): 
- Data understanding, exploration, data loading, data importing.
- Check dataset structure using Column Headers & Data Types
- Import the csv. Data from Excel Into Powerbi’s Power Query Editor

##  2. **Data Cleaning & Transformation in Power BI (Power Query)**  

#### **Introduction**  
Data cleaning is a crucial step before analysis, ensuring accuracy, consistency, and completeness. In this project, Power BI’s **Power Query Editor** is used to clean and transform the retail sales dataset. This document outlines the detailed steps taken to prepare the data for analysis.  

#### **Step 1: Removing Duplicates**  
- Identify key columns where duplicates should not exist (e.g., `Order Number` , `ProductKey` , `Storekey` , `Customerkey`).  
- Select the column → Click **Remove Duplicates** in the ribbon.  
- Verify the row count before and after removal.
![Screenshot (48)](https://github.com/user-attachments/assets/ac7e92c7-cdeb-4eb1-b05f-983d52d6aa89)
- *Image Showing Duplicate Removal Process*

#### **Step 2: Standardizing Data Formats**  
- Ensure numerical fields (e.g., `Sales`, `Quantity`) are in **Decimal Number** format.  
- Convert categorical fields (e.g., `Product Name`) to **Text** format.  
- Convert date fields (`Transaction Date`) to **Date** format.  
  - **Home** → **Data Type** → Select **Date**  
- Standardize text formatting: Convert to **Proper Case** (e.g., "laptop" → "Laptop") using:  

  ![Screenshot (50)](https://github.com/user-attachments/assets/5551047f-e697-483d-9044-d9510c545d3a)
  - *Image Showing Format Standardizing Process*
  
#### **Step 3: Handling Missing Values**  
- Identifying Missing Data 
- Click on each column and check for **null** values in the data preview.  
- Use **Filter** to check missing entries. 

#### Step 4: Loading Clean Data into Power BI
Click Close & Apply to load the cleaned dataset into Power BI for visualization.

---
## Data Modelling
The goal was to build a **star schema** to enhance query performance and enable meaningful analysis.

### Defining Relationships
#### **Primary & Foreign Keys**
To establish relationships:
- **Sales Table** → Linked to `Products Table` using `ProductKey`.
- **Sales Table** → Linked to `Stores Table` using `StoreKey`.
- **Sales Table** → Linked to `Customers Table` using `CustomerKey`.
- **Sales Table** → Linked to `Date Table` using `Order Date` as primary relationship and `Delivery Date` as secondary relationship.
![Screenshot (53)](https://github.com/user-attachments/assets/d28ec2c1-178b-4522-bbb5-f4d1930376da)
- *Image Showing Completed Data Model*
 
---

## 🧮 Key DAX Measures
- 1. Total Revenue
```DAX
Total Revenue = 
  SUM(Sales[Revenue])
```
- 2. Revenue Percentage Share
```DAX
Revenue Percentage Share =
    DIVIDE([Revenue], CALCULATE([Revenue], ALL(stores[Country])), 0)
```
- 3. Orders Count
```DAX
Orders = 
    SUM(Sales[Quantity])
```
- 4. Top product Sales In Each Country
```DAX 
Top Country (Product) = 
VAR ProductSalesByCountry =  
    SUMMARIZE(
        'sales',
        'stores'[Country],  
        'sales'[ProductKey],  
        "TotalRevenue", SUMX('sales', 'sales'[Quantity] * RELATED('Products'[Unit Price USD])),  
        "TotalQuantity", SUM('sales'[Quantity])
    )

VAR TopCountry =  
    TOPN(1, ProductSalesByCountry, [TotalRevenue], DESC, [TotalQuantity], DESC, 'stores'[Country], ASC)

RETURN  
    CONCATENATEX(TopCountry, 'stores'[Country], ", ")
```
- 5. Revenue Growth % per Year
```DAX
Revenue Growth % = 
    Gross Profit Margin % = 
    DIVIDE([Gross Profit], [Revenue], 0)
```

- 6. Average Delivery Time For Online Purchases
```DAX
Average Delivery Time (Days) = 
VAR DeliveryDays =
    AVERAGEX(
        FILTER(
            'sales', 
            NOT(ISBLANK('sales'[Delivery Date])) -- Only include delivered orders
        ),
        DATEDIFF('sales'[Order Date], 'sales'[Delivery Date], DAY)
    )
RETURN
    DeliveryDays
```
- 5. Average Customer Spend
```DAX
Average Customer Spend = 
    DIVIDE([Revenue], [Customers], 0)
```

- 7. Total Product Delivered (N) Days
```DAX
Total Quantity Delivered (Last 10 Days) = 
VAR SelectedDays = SELECTEDVALUE('Days Slicer'[Value])
VAR MaxDate = MAX('sales'[Order Date])
VAR MinDate = MaxDate - SelectedDays 

RETURN 
CALCULATE(
    SUM('sales'[Quantity]),
    'sales'[Order Date] >= MinDate && 'sales'[Order Date] <= MaxDate,
    NOT(ISBLANK('sales'[Delivery Date]))
)
```
- 8. Customer Conversion Rate
```DAX
Conversion Rate % = 
    VAR TotalCustomers = DISTINCTCOUNT(Customers[CustomerKey])
    VAR ConvertedCustomers = DISTINCTCOUNT(Sales[CustomerKey])
    
    RETURN DIVIDE(ConvertedCustomers, TotalCustomers, 0)
```
---

![Screenshot (131)](https://github.com/user-attachments/assets/77e57c3f-fff0-413a-8591-a6153b8d86eb)

![Screenshot (132)](https://github.com/user-attachments/assets/3a861475-e309-44f9-b622-424d3a7b943a)


---

### 🧮 Key Analysis Visuals
- Revenue Percentage Share
![Screenshot (54)](https://github.com/user-attachments/assets/8d23e4d0-a323-452c-b7e6-1a51b2c316e6)

***Analysis:** `USA` is the **top-performing country** in terms of revenue with `43%` of Total Revenue.  `Online sales` contributed 20% of the total revenue, showing consistent growth.*

- Sales Pattern
![Screenshot (58)](https://github.com/user-attachments/assets/71ee9dfe-7614-4973-bd23-387b9f432fcf)

***Analysis:** Heatmap Visual shows Sales always decrease by about 35% around March - April and Gradually pickups around May**, showing shopping trends, and Peaks by December - February.*

- Revenue % Growth
![Screenshot (55)](https://github.com/user-attachments/assets/817a821f-7b48-412e-989f-95d9a7e00d69)

***Analysis:** `2019` had the best revenue growth and there's been a continious decline in sales and revenue generated.*

- Average Delivery Time For Online Purchases
![Screenshot (59)](https://github.com/user-attachments/assets/1df97b35-3ad8-4f1f-8a14-e077a07ae662)

***Analysis:** Gradually reduction in delivery days each year, showing improvments in shipping methods and reduction in customer waiting time*

---

## 🏆 Dashboard Visuals
To ensure actionable insights, the following key visuals are included in the Power BI dashboard:
1. **Revenue Breakdown by Product & Category** - Identifies top-performing products.
2. **Top Revenue-Generating Countries & Stores** - Highlights the best locations for sales.
3. **Online vs. Offline Sales Performance** - Compares digital and physical sales trends.
4. **Sales Trends Over Time** - Unveils seasonal trends and purchasing behavior.

---
### Overall Dashboard showing key KPIs and Overall sales perfomance.
![Screenshot (40)](https://github.com/user-attachments/assets/40fdfbd9-7aec-477e-a963-1ebd52e6bd40)

### 2. Product Sales Insights:
#### Dashboard showing Product sales performance for In-store purchases.
![Screenshot (42)](https://github.com/user-attachments/assets/d917da04-6765-4058-ab66-5dcffaade328)

### 3. Category Performance Insights:
#### Dashboard showing revenue by each Category, changes in sales performance, changes in different states performance
![Screenshot (45)](https://github.com/user-attachments/assets/d3aca86f-dcac-4a44-b9da-b32b56be5fc8)

### 4. Customers Details Overview:
![Screenshot (46)](https://github.com/user-attachments/assets/8b1fe3dd-51f3-4a88-8c44-5783a99f824e)

## 🔗 Power BI Dashboard Link
![Electronic Report Image](https://github.com/user-attachments/assets/e6ea5e61-e8ff-414c-9a13-29b3cb3ac118)

Clink The Link to [View the Interactive Retail Sales Dashboard to view Key Visuals](https://app.powerbi.com/reportEmbed?reportId=ae2e2030-6ab3-427c-8e0a-cde0dac20946&autoAuth=true&ctid=3af45fec-8c0e-49be-8467-608b1fd05a35)

---

## 📈 Key Insights
- **WWW Desktop PC2.33 X2330 Black** has generated the highest revenue so far.
- **USA** is the **top-performing country** in terms of revenue with **43%** of Total Revenue
- **Online sales contributed 20% of the total revenue**, showing consistent growth.
- **Sales always decrease by about 35% around March - April and Gradually pickups around May**, largely due to holiday shopping trends.
- **Desktops** were the best-selling product categories across all regions.

---

## 📌 About Me
Hi, I'm Oluwatosin Amosu Bolaji, a Data Analyst with strong skills in Python, SQL, Power BI, and Excel. I turn raw data into actionable insights through automation, data storytelling, and visual analytics. My work is rooted in analytical thinking, strong business acumen, and technical expertise. Whether it's uncovering hidden trends, optimizing workflows, or translating data into compelling stories, I bring clarity and direction to data—helping organizations make smarter, faster decisions.

## 💡 Tools & Tech:
- **Python** (Pandas, NumPy, Matplotlib, Seaborn)
- **SQL** (MsSQL, Postgree, MySQL)
- **Microsoft Power BI**
- **Microsoft Excel**
- 🔹 **Key Skills:** Data wrangling, dashboarding, reporting, and process optimization.
- ![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white) ![SQL](https://img.shields.io/badge/SQL-Server-red?logo=microsoft-sql-server&logoColor=white) ![PowerBI](https://img.shields.io/badge/Power_BI-F2C811?logo=powerbi&logoColor=black) ![Excel](https://img.shields.io/badge/Excel-217346?logo=microsoft-excel&logoColor=white)


#### 🚀 **Always learning. Always building. Data-driven to the core.**  

### 📫 **Let’s connect!**  
- 📩 oluwabolaji60@gmail.com
- 🔗 : [LinkedIn](https://www.linkedin.com/in/oluwatosin-amosu-722b88141)
- 🌐 : [My Portfolio](https://www.datascienceportfol.io/oluwabolaji60) 
- 𝕏 : [Twitter/X](https://x.com/thee_oluwatosin?s=21&t=EqoeQVdQd038wlSUzAtQzw)
- 🔗 : [Medium](https://medium.com/@oluwabolaji60)
- 🔗 : [View my Repositories](https://github.com/Tbrown1998?tab=repositories)


