# 📊 Electronics Retail Superstore Power BI Dashboard

![032811bestbuy](https://github.com/user-attachments/assets/5877fd2d-37fd-4d62-9b97-804706324685)


## 📌 Project Overview
This project focuses on analyzing sales data from an **Electronics Retail Superstore** that operates **physical stores across multiple countries** while also providing an **online shopping platform**. The Power BI dashboard developed for this project delivers deep insights into **top-performing products, revenue trends, store-wise sales, and online vs. offline purchase patterns**.

By leveraging **Power BI and Excel**, this analysis helps stakeholders understand key revenue drivers and optimize business strategies.

## 🎯 Objectives
The primary goals of this project include:
- **Identifying top revenue-generating products** across all sales channels.
- **Analyzing store-wise and country-wise sales performance** to determine profitable locations.
- **Comparing online vs. offline sales trends** to optimize inventory distribution.
- **Recognizing seasonal sales trends** and peak shopping periods.
- **Evaluating product categories contributing most to total revenue**.

## 🛠️ Technologies Used
- **Power BI** - Data visualization and interactive reporting.
- **SQL** - Data extraction, transformation, and analysis.
- **Excel** - Initial data exploration and preprocessing.

### **Data Source**  
The dataset was sourced from **[Maven Analytics](https://mavenanalytics.io/data-playground?order=date_added%2Cdesc&search=electronic)** and including tables containing information about transactions, products, customers, stores.
- **Sales Transactions** – Records of all coffee sales.  
- **Stores** – Information on existing sales locations.  
- **Customers** – Demographic and behavioral data of consumers.  
- **Products** – Details on different product types and pricing.  

### **Expected Outcomes**  
By analyzing sales patterns and customer behaviors, this project aims to identify the most promising locations for expansion, helping Monday Coffee **maximize revenue, optimize store placement, and enhance customer experience.** 

## 📊 Dataset Schema
| Table         | Description                                      |
|--------------|--------------------------------------------------|
| **Sales**    | Contains transactional sales data               |
| **Stores**   | Details of store locations and their countries  |
| **Products** | Information about products and their attributes |
| **Category** | Hierarchical classification of product types    |
| **Date** | Calendar date for time data analysis        |

## Data Model
![Screenshot (38)](https://github.com/user-attachments/assets/0e670d8e-d2ec-439b-a063-2547b0208b86)

---

## 🧮 Key DAX Measures
- 1. Total Revenue
```DAX
Total Revenue = SUM(Sales[Revenue])
```
- 2. Top product Sales by Country
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
- 3. Revenue Growth %
```DAX
Gross Profit Margin % = 
DIVIDE([Gross Profit], [Revenue], 0)
```
- 4. Average Delivery Time
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
DIVIDE([Revenue], [Customers], 0)
```
- 6. Total Product Delivered (N) Days
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
- 7. Customer Conversion Rate
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

### 1. Dashboard Overview:
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

