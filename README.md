# 📊 Electronics Retail Superstore Power BI Dashboard

![032811bestbuy](https://github.com/user-attachments/assets/5877fd2d-37fd-4d62-9b97-804706324685)


## 📌 Project Overview
This project focuses on analyzing sales data from an **Electronics Retail Superstore** that operates **physical stores across multiple countries** while also providing an **online shopping platform**. The Power BI dashboard developed for this project delivers deep insights into **top-performing products, revenue trends, store-wise sales, and online vs. offline purchase patterns

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
This project aims to analyze and optimize the retail sales performance of an electronics superstore by leveraging data analytics. The dataset consists of transactional sales data from multiple store locations and an online platform. The primary objectives of this analysis are:  

1. **Data Cleaning & Transformation** – Ensure data integrity by handling missing values, duplicates, and inconsistencies across multiple tables.  
2. **Exploratory Data Analysis (EDA)** – Identify key sales trends, product performance, and customer purchasing behavior through statistical and visual analysis.  
3. **SQL-Based Business Insights** – Utilize SQL queries to answer critical business questions, such as top-selling products, revenue trends by region, and store performance comparisons.  
4. **Data Modeling & Relationship Mapping** – Establish connections between product categories, sales transactions, warranty claims, and store locations for better analysis.  
5. **Dashboard & Data Visualization** – Create an interactive Power BI dashboard to present insights in a clear and actionable manner, aiding strategic decision-making.

---

## 🎯 Project Goals
The primary goals of this project include:
- **Identifying top revenue-generating products** across all sales channels.
- **Analyzing store-wise and country-wise sales performance** to determine profitable locations.
- **Comparing online vs. offline sales trends** to optimize inventory distribution.
- **Recognizing seasonal sales trends** and peak shopping periods.
- **Evaluating product categories contributing most to total revenue**.

### **Expected Outcomes**  
By analyzing sales patterns and customer behaviors, this project aims to identify the most promising locations for expansion, helping Monday Coffee **maximize revenue, optimize store placement, and enhance customer experience.** 

---

## 🛠️ Technologies Stack
- **Excel** - Initial data exploration and preprocessing.
- **Power BI** - Data Cleaning with Power Query, Data Modelling, DAX, Data visualization and interactive reporting.
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
- Standardizing Data Formats

##  2. **Data Cleaning & Transformation in Power BI (Power Query)**  

#### **Introduction**  
Data cleaning is a crucial step before analysis, ensuring accuracy, consistency, and completeness. In this project, Power BI’s **Power Query Editor** is used to clean and transform the retail sales dataset. This document outlines the detailed steps taken to prepare the data for analysis.  

#### **Step 1: Removing Duplicates**  
- Identify key columns where duplicates should not exist (e.g., `Order Number` , `ProductKey` , `Storekey` , `Customerkey`).  
- Select the column → Click **Remove Duplicates** in the ribbon.  
- Verify the row count before and after removal.
![Screenshot (48)](https://github.com/user-attachments/assets/ac7e92c7-cdeb-4eb1-b05f-983d52d6aa89)
                                            **Image Showing Duplicate Removal Process**

#### **Step 2: Standardizing Data Formats**  
- Ensure numerical fields (e.g., `Sales`, `Quantity`) are in **Decimal Number** format.  
- Convert categorical fields (e.g., `Product Name`) to **Text** format.  
- Convert date fields (`Transaction Date`) to **Date** format.  
  - **Home** → **Data Type** → Select **Date**  
- Standardize text formatting:  
  - Convert to **Proper Case** (e.g., "laptop" → "Laptop") using:  
    ```powerquery
    Text.Proper([Product Name])
    ```
  ![Screenshot (50)](https://github.com/user-attachments/assets/5551047f-e697-483d-9044-d9510c545d3a)
                                            **Image Showing Format Standardizing Process**
#### **Step 3: Handling Missing Values**  
##### **A. Identifying Missing Data**  
- Click on each column and check for **null** values in the data preview.  
- Use **Filter** to check missing entries. 

#### Step 4: Loading Clean Data into Power BI
Click Close & Apply to load the cleaned dataset into Power BI for visualization.

---
## Project Data Modelling
The goal was to build a **star schema** to enhance query performance and enable meaningful analysis.
#### Step 1: Creating Date Table For Time Data Analysis
```DAX
Date = 
VAR StartDate = DATE(2016, 1, 1)  -- Change to your preferred start date
VAR EndDate = TODAY()  -- Uses today's date as the end date
RETURN CALENDAR(StartDate, EndDate)
```
- Extract neccessary columns from `Date` column
![Screenshot (52)](https://github.com/user-attachments/assets/26330727-0417-4a67-85ed-024446e11b65)
                                            **Image Showing Completed Date Table**

### Step 2: Defining Relationships
#### **Primary & Foreign Keys**
To establish relationships:
- **Sales Table** → Linked to `Products Table` using `ProductKey`.
- **Sales Table** → Linked to `Stores Table` using `StoreKey`.
- **Sales Table** → Linked to `Customers Table` using `CustomerKey`.
- **Sales Table** → Linked to `Date Table` using `Order Date` as primary relationship and `Delivery Date` as secondary relationship.
![Screenshot (53)](https://github.com/user-attachments/assets/d28ec2c1-178b-4522-bbb5-f4d1930376da)
                                            **Image Showing Completed Data Model**
 
---

## 🧮 Key DAX Measures
- 1. Total Revenue
```DAX
Total Revenue = SUM(Sales[Revenue])
```
- 2. Revenue Percentage Share
```DAX 
DIVIDE([Revenue], CALCULATE([Revenue], ALL(stores[Country])), 0)
```
![Screenshot (54)](https://github.com/user-attachments/assets/e63d1398-b885-4026-81c6-58369454cae3)
##### Analysis: `USA` is the **top-performing country** in terms of revenue with `43%` of Total Revenue.  `Online sales` contributed 20% of the total revenue, showing consistent growth.

- 3. Orders Count
```DAX 
SUM(Sales[Quantity])
```
![Screenshot (58)](https://github.com/user-attachments/assets/9186ed80-04df-4a09-8b57-bb8602f721c1)
##### Analysis: Heatmap Visual shows Sales always decrease by about 35% around March - April and Gradually pickups around May**, showing shopping trends, and Peaks by December - February.

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
- 5. Revenue Growth %
```DAX
Gross Profit Margin % = 
DIVIDE([Gross Profit], [Revenue], 0)
```
![Screenshot (55)](https://github.com/user-attachments/assets/6cdf2134-2aa4-4d07-925c-4646270572c8)
##### Analysis: `2019` had the best revenue growth and there's been a continious decline in sales and revenue generated.

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
![Screenshot (59)](https://github.com/user-attachments/assets/fc5c1712-e65f-4499-b752-7f3e8425bf27)
##### Analysis: Gradually reduction in delivery days each year, showing improvments in shipping methods and reduction in customer waiting time
- 5. Average Customer Spend
```DAX
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
## 🏆 Dashboard Visuals
To ensure actionable insights, the following key visuals are included in the Power BI dashboard:
1. **Revenue Breakdown by Product & Category** - Identifies top-performing products.
2. **Top Revenue-Generating Countries & Stores** - Highlights the best locations for sales.
3. **Online vs. Offline Sales Performance** - Compares digital and physical sales trends.
4. **Sales Trends Over Time** - Unveils seasonal trends and purchasing behavior.

## Key Visuals
- ### Top Countries by Revenue
![Screenshot (63)](https://github.com/user-attachments/assets/32c12dc1-3add-4a38-8eca-88b1cc153245)

- ### Revenue Share by Purchase Channel
![Screenshot (64)](https://github.com/user-attachments/assets/d6fe266b-f1fa-4973-981c-235529d8a655)

- ### Highest Performing Product Categories
![Screenshot (62)](https://github.com/user-attachments/assets/68ae9fe3-4677-4e17-8c70-98264ee099bc)

- ### Revenue Across Each Month
![Screenshot (65)](https://github.com/user-attachments/assets/8652aa39-2b0c-4b48-8649-803634b46032)

---
### 1. Full Dashboard Overview:
- **Overall Dashboard showing key KPIs and Overall sales perfomance.**
![Screenshot (40)](https://github.com/user-attachments/assets/40fdfbd9-7aec-477e-a963-1ebd52e6bd40)

- **Overall Dashboard showing the Filter Pane and Active Filter on Dashboard**
![Screenshot (41)](https://github.com/user-attachments/assets/a7d17023-3a68-4226-8bd7-ee291f289231)

### 2. Product Sales Insights:
- **Dashboard showing Product sales performance for In-store purchases.**
![Screenshot (42)](https://github.com/user-attachments/assets/d917da04-6765-4058-ab66-5dcffaade328)

- **Dashboard showing Product sales performance for Online purchases also with the average delivery days.**
![Screenshot (43)](https://github.com/user-attachments/assets/f6a15f05-d6a0-4b74-8c9d-9ede8534db99)

### 3. Category Performance Insights:
- **Dashboard showing revenue by each Category, changes in sales performance, changes in different states performance**
![Screenshot (45)](https://github.com/user-attachments/assets/d3aca86f-dcac-4a44-b9da-b32b56be5fc8)

### 4. Category Performance Insights:
![Screenshot (46)](https://github.com/user-attachments/assets/8b1fe3dd-51f3-4a88-8c44-5783a99f824e)
---

## 📈 Key Insights
- **WWW Desktop PC2.33 X2330 Black** has generated the highest revenue so far.
- **USA** is the **top-performing country** in terms of revenue with **43%** of Total Revenue
- **Online sales contributed 20% of the total revenue**, showing consistent growth.
- **Sales always decrease by about 35% around March - April and Gradually pickups around May**, largely due to holiday shopping trends.
- **Desktops** were the best-selling product categories across all regions.

## 🚀 How to Use This Project
1. **Clone the repository** using the following command:
```bash
git clone https://github.com/OluwatosinAmosu/Electronics-Retail-Dashboard.git
```
2. Open the **Electronics_Sales.pbix** file in **Power BI**.
3. Explore the interactive visuals to uncover insights.

## 🔗 Related Projects
If you found this project interesting, check out these related analytics projects:
- [E-commerce Sales Analysis](#)
- [Retail Customer Segmentation](#)

---

## 📌 About Me
Hi, I'm Oluwatosin Amosu Bolaji, a Data Analyst skilled in SQL, Power BI, and Excel. I enjoy turning complex datasets into actionable insights through data visualization and business intelligence techniques.

- **🔹 Key Skills:** Data Analysis | SQL Queries | Power BI Dashboards | Data Cleaning | Reporting
- **🔹 Passionate About:** Data storytelling, problem-solving, and continuous learning

- **📫 Let's connect!**
- 🔗 [Linkedin](www.linkedin.com/in/oluwatosin-amosu-722b88141) | 🌐 [Portfolio](https://github.com/Tbrown1998?tab=repositories) | 📩 oluwabolaji60@gmail.com

