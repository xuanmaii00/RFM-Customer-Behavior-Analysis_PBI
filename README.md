# RFM-Based Customer Behavior Analysis_SQL/ Power BI

# 📑 Table of Contents

1. [📌 Background & Overview](#-background--overview)  
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)  
3. [🧠 Design Thinking Process](#-design-thinking-process)  
4. [📊 Key Insights & Visualizations](#-key-insights--visualizations)  
5. [🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)  

---

## 📌 Background & Overview

### Objective:
Adventure Works is a bicycle manufacturer offering a range of bicycles and accessories for various customer segments. With an increasing customer base, it is essential to analyze purchasing behaviors to improve retention, boost engagement, and optimize marketing efforts.
This project focuses on analyzing customer behaviors and segmenting them using RFM (Recency, Frequency, Monetary) analysis. By leveraging design thinking, I aim to identify different customer segments and provide strategic marketing recommendations to enhance retention, conversion, and loyalty.

### 👤 Who is this project for?

✔️ Marketing professionals & strategists  
✔️ Business analysts & data analysts  
✔️ E-commerce & retail decision-makers  

### ❓ Business Questions:

✔️ How can we categorize customers based on purchasing behavior?  
✔️ What strategies can be used to retain high-value customers?  
✔️ How can we re-engage customers who are at risk of churning?  
✔️ What actions should be taken for new or inactive customers?  

### 🎯 Project Outcome:

✔️ **Customer Segmentation**: Identified four key customer groups: Top Value, Need Retention, Need Conversion, No Action.  
✔️ **Strategic Recommendations**: Developed tailored marketing strategies to boost engagement and revenue.  
✔️ **Data-Driven Insights**: Provided actionable insights based on real customer behavior.  

---

## 📂 Dataset Description & Data Structure

### 📌 Data Source:

- **Source**: Internal company database
- **Format**: `.csv`

## 📊 Data Structure & Relationships  

To effectively analyze sales performance and customer behavior, the dataset was structured into a **star schema**, with **Sales.SalesOrderDetail** serving as the **Fact_Sales** table. This central fact table connects to multiple **dimension tables** to enhance data granularity and improve analytical capabilities. The key tables used in the analysis are:  

### **1️⃣ Fact Table: Fact_Sales (Sales.SalesOrderDetail)**  
The **Fact_Sales** table contains detailed sales transactions, storing key metrics such as unit price, discount, and order quantity.  

| **Column Name**        | **Data Type**        | **Description**  |
|------------------------|---------------------|------------------|
| `SalesOrderID`          | int                 | Foreign key to SalesOrderHeader, identifying each sales order.  |
| `SalesOrderDetailID`    | int                 | Unique identifier for each product sold within an order.  |
| `CarrierTrackingNumber` | nvarchar(25)        | Shipment tracking number provided by the shipper.  |
| `OrderQty`             | smallint             | Quantity ordered for each product.  |
| `ProductID`           | int                  | Foreign key referencing the product being sold.  |
| `SpecialOfferID`       | int                  | Promotional code applied to the sale.  |
| `UnitPrice`           | money                | Selling price of a single product.  |
| `UnitPriceDiscount`   | money                | Discount amount applied to the unit price.  |
| `LineTotal`           | numeric(38,6)        | Total revenue per product sold, computed as: **UnitPrice × (1 - UnitPriceDiscount) × OrderQty**.  |
| `rowguid`             | uniqueidentifier     | Unique identifier for each record (used for replication).  |
| `ModifiedDate`        | datetime             | Last modification timestamp.  |


### **2️⃣ Dimension Tables**  

#### **Dim_Customer (Sales.Customer)**  
Stores customer details, allowing segmentation and personalization of marketing strategies.  

| **Column Name**   | **Data Type**  | **Description**  |
|-------------------|--------------|------------------|
| `CustomerID`       | int          | Primary key, uniquely identifying each customer.  |
| `PersonID`        | int          | Foreign key to Person.BusinessEntityID.  |
| `StoreID`         | int          | Foreign key to Store.BusinessEntityID.  |
| `TerritoryID`     | int          | Identifies the customer's territory (links to SalesTerritory).  |
| `AccountNumber`   | varchar(10)  | Unique identifier assigned by the accounting system.  |
| `rowguid`         | uniqueidentifier | Unique identifier for replication.  |
| `ModifiedDate`    | datetime     | Last modification timestamp.  |


#### **Dim_ProductCategory (Production.ProductCategory)**  
Provides a high-level classification of products.  

| **Column Name**      | **Data Type**     | **Description**  |
|----------------------|------------------|------------------|
| `ProductCategoryID`   | int              | Primary key identifying product category.  |
| `Name`               | nvarchar(50)     | Descriptive name of the category.  |
| `rowguid`           | uniqueidentifier  | Unique identifier for replication.  |
| `ModifiedDate`      | datetime         | Last modification timestamp.  |


#### **Dim_ProductSubcategory (Production.ProductSubcategory)**  
Represents a more granular classification of products under each category.  

| **Column Name**      | **Data Type**     | **Description**  |
|----------------------|------------------|------------------|
| `ProductSubcategoryID` | int              | Primary key identifying subcategory.  |
| `ProductCategoryID`   | int              | Foreign key linking to ProductCategory.  |
| `Name`               | nvarchar(50)     | Descriptive name of the subcategory.  |
| `rowguid`           | uniqueidentifier  | Unique identifier for replication.  |
| `ModifiedDate`      | datetime         | Last modification timestamp.  |


#### **Dim_Location (Production.Location)**  
Represents the geographical locations relevant to sales and operations.  

| **Column Name**  | **Data Type**   | **Description**  |
|-----------------|---------------|------------------|
| `LocationID`     | smallint       | Primary key identifying each location.  |
| `Name`          | nvarchar(50)   | Location name/description.  |
| `CostRate`      | smallmoney     | Standard hourly cost of the location.  |
| `Availability`  | decimal(8,2)   | Work capacity (in hours).  |
| `ModifiedDate`  | datetime       | Last modification timestamp.  |


#### **Dim_Segment (Customer Segmentation based on RFM Scores)**  
Customers are classified based on **Recency, Frequency, and Monetary (RFM) scores** into segments such as **Champions, Loyal Customers, At-Risk, and Lost Customers**.  

| **Customer Segment**       | **RFM Score Groups**  | **Characteristics**  |
|---------------------------|----------------------|----------------------|
| Champions                | 555, 554, 544, etc.  | Frequent, high-value customers with recent purchases.  |
| Loyal Customers          | 543, 444, 435, etc.  | Regular customers with good spending patterns.  |
| Potential Loyalists      | 553, 552, 531, etc.  | Recent buyers with medium-to-high spending.  |
| Recent Customers         | 512, 511, 311, etc.  | New customers with low purchase frequency.  |
| Promising               | 525, 524, 523, etc.  | New customers with high spending potential.  |
| Customers Needing Attention | 535, 534, 443, etc. | Previously frequent buyers, recently inactive.  |
| At Risk                 | 255, 254, 245, etc.  | High-value customers who have stopped purchasing.  |
| Can't Lose Them         | 155, 154, 144, etc.  | Previously high-value, now inactive customers.  |
| Lost                    | 111, 112, 121, etc.  | Customers who made very few purchases a long time ago.  |


#### **Dim_Date (Created using DAX)**  
This dimension table enables time-based analysis, helping to track sales trends over different periods.  

| **Column Name**   | **Data Type**  | **Description**  |
|-------------------|--------------|------------------|
| `Date`           | date          | Full date value.  |
| `Year`           | int          | Year extracted from the date.  |
| `Month`         | nvarchar(20)  | Full month name.  |
| `Month Number`   | int          | Numeric month (1-12).  |
| `Year-Month`     | nvarchar(7)  | Concatenated Year-Month (YYYY-MM).  |
| `Quarter`        | nvarchar(2)  | Quarter (Q1-Q4).  |
| `Day`           | int          | Day of the month.  |
| `Weekday`       | nvarchar(20)  | Full name of the weekday.  |
| `Weekday Number` | int          | Numeric weekday (1=Monday, 7=Sunday).  |
| `Is Weekend`    | bit          | Identifies weekends (TRUE/FALSE).  |
| `Week Number`   | int          | Week number of the year.  |
| `Day of Year`   | int          | Day of the year (1-365).  |

## 📊 Measure Table – Key Performance Indicators  

To support **customer segmentation and retention analysis**, I created a **Measure Table** in Power BI that calculates essential metrics based on the dataset.  

| **Measure Name** | **Calculation Logic** | **Purpose** |
|------------------|----------------------|-------------|
| **Total Revenue** | `SUM(Sales[Revenue])` | Calculates the total revenue from all transactions. |
| **Average Order Value (AOV)** | `DIVIDE([Total Revenue], [Total Order Count])` | Determines the average revenue per order. |
| **Customer Lifetime Value (CLV)** | `SUMX(VALUES(Customer[CustomerID]), [Average Order Value] * [Average Frequency])` | Estimates the total value a customer brings over time. |
| **Recency (Days)** | `DATEDIFF(MAX(Sales[TransactionDate]), TODAY(), DAY)` | Measures how recently a customer made a purchase. |
| **Frequency** | `COUNT(Sales[OrderID])` | Counts the number of transactions per customer. |
| **Monetary Value** | `SUM(Sales[Revenue])` | Sums the total revenue per customer. |
| **Churn Rate** | `DIVIDE([Churned Customers], [Total Customers])` | Calculates the percentage of customers who stopped purchasing. |
| **Customer Retention Rate** | `1 - [Churn Rate]` | Determines the percentage of returning customers. |
| **RFM Score** | `RANKX(ALL(Customer), [Recency] + [Frequency] + [Monetary], , DESC, Dense)` | Scores customers based on their **Recency, Frequency, and Monetary Value**. |
| **High-Value Customers** | `IF([RFM Score] >= THRESHOLD, "High-Value", "Others")` | Categorizes customers based on their RFM score. | 


### **Data Relationships & Schema Design**  

The **Fact_Sales** table is at the center of the star schema, connecting to multiple dimension tables via **foreign key relationships**

![Image](https://github.com/user-attachments/assets/6a2412ee-33b1-4120-bbb5-baa564505af9)

---

## 🧠 Design Thinking Process  

### 1️⃣ Empathize – Understanding Stakeholders’ Needs  

To ensure the dashboard meets business needs, I analyzed stakeholder concerns using the **5W1H framework** and an **Empathy Map**.  

#### 🎯 Key Stakeholders  
- **Marketing Professionals & Strategists** – Use the dashboard to refine **campaign strategies** and **improve customer engagement**.  
- **Business & Data Analysts** – Leverage insights to identify **customer trends**, **optimize segmentation**, and support **data-driven decisions**.  
- **E-commerce & Retail Decision-Makers** – Utilize the dashboard to **enhance customer retention**, **allocate budgets efficiently**, and **boost sales performance**.  

#### ⚠️ Challenges Faced  
- Lack of **structured customer insights**, leading to **ineffective targeting** and **wasted marketing spend**.  
- Difficulty in **prioritizing customers**, making it hard to **personalize campaigns**.  
- No **churn prevention strategy**, resulting in **lost revenue**.  

#### 🔍 5W1H Analysis  

| **Question** | **Insight** |
|-------------|------------|
| **Who will use the dashboard?** | **Marketing professionals, business analysts, and e-commerce decision-makers** for **customer segmentation** and **campaign planning**. |
| **What problem does it solve?** | Provides **structured segmentation** to identify **high-value and at-risk customers**. |
| **When & Where is it used?** | - **Monthly meetings** for retention analysis. <br> - **Marketing campaign planning** to refine audience targeting. <br> - **Sales forecasting** to allocate budgets effectively. |
| **Why is it needed?** | - **Improves marketing precision**. <br> - **Optimizes customer retention efforts**. <br> - **Supports data-driven sales strategies**. |
| **How was the problem handled before?** | - **Relying on intuition** and **manual tracking**. <br> - **Running generic campaigns** with low engagement. |

#### 📝 Empathy Map – Understanding Stakeholder Perspectives  

| **Aspect** | **Insights from Stakeholders** |
|-----------|--------------------------------|
| **Thinking & Feeling** | "How do we identify our **most valuable customers**?" <br> "How can we **prevent churn**?" |
| **Seeing** | **Unclear customer segmentation**, **broad marketing efforts**, **low conversion rates**. |
| **Saying & Doing** | "We need **data-driven targeting strategies**." <br> "We should **focus on retaining high-value customers**." |
| **Pain Points** | - **Marketing budget wasted** on low-value customers. <br> - No clear strategy to **reduce churn**. |
| **Gains** | - **Higher CLV** through better segmentation. <br> - **Improved marketing ROI** with precise targeting. <br> - **Reduced churn & increased loyalty**. |

### 2️⃣ Define – Establishing Key Metrics & Growth Formula  

#### 🎯 North Star Metric: Defining Success  
To create a meaningful customer segmentation strategy, I define key success metrics that drive growth. My **North Star Metric (NSM)** is based on **RFM (Recency, Frequency, Monetary) scoring**, providing a structured way to classify customers and track business performance.


#### 📌 Growth Formula  
I define **customer segments** and evaluate their impact across **three dimensions**:

| **North Star Metric 1**  | **North Star Metric 2 (Optional)**  | **Dimension Group**  | **Group 1** | **Group 2** | **Group 3** | **Group 4** |
|--------------------------|-------------------------------------|----------------------|-------------|-------------|-------------|-------------|
| **RFM Score**  | Revenue Contribution by Segment | **Customer Segments** | Top Value | Need Retention | Need Conversion | No Action |
| **RFM Score**  | Regional Customer Trends | **Geographic Region** | Customer behavior patterns by region | | | |
| **RFM Score**  | Product Category Performance | **Product Segments** | Revenue share per product group | | | |


#### 📊 When is Success Achieved?  
Success is measured when I can effectively categorize and analyze customer behavior across different segments.  

| **Dimension**         | **View**                                  | **Description**                                           | **Why?** |
|----------------------|--------------------------------|-------------------------------------------------|--------|
| **Customer Segments** | RFM-Based Customer Groups | Categorizing customers into **4 key segments**: Top Value, Need Retention, Need Conversion, No Action. | Enables targeted strategies for each segment. |
| **Geographic Region** | Regional Revenue Trends | Understanding customer trends in different regions. | Helps optimize marketing campaigns by location. |
| **Product Segments** | Product Performance Metrics | Revenue contribution by different product categories. | Supports inventory planning and product marketing. |

#### 📈 Why Choose These Metrics?  
- **Clear & Measurable**: RFM scoring is easy to understand and implement.  
- **Predictive Power**: Helps anticipate future customer behavior.  
- **Strategic Value**: Provides actionable insights for marketing, sales, and retention strategies.  


### 3️⃣ Ideate – Developing Actionable Insights  

### 📌 Dashboard Structure  
| **Idea Name** | **Layer 0** – High-Level Metrics | **Layer 1** – Breakdown by One Dimension | **Layer 2** – Breakdown by Two Dimensions | **Key Actionable Insights** |
|--------------|---------------------------------|---------------------------------|---------------------------------|---------------------------------|
| **Segments Overview** | RFM-Based Segments, grouped into 4 major action groups | - Revenue contribution by segment  <br> - Yearly revenue trends per segment <br> - Customer count per segment | - Revenue share of each segment within a group | Identify the most valuable segments and tailor marketing strategies accordingly. |
| **Top Value** | Avg. RFM Score of top customer groups | - Avg. cart size & order value per segment <br> - Avg. spending per product category | - Avg. basket value by country | Strengthen loyalty programs and exclusive offers for high-value customers. |
| **Need Retention** | Revenue trend of this group over time | - Key products driving revenue from this group <br> - Revenue trends for top products | - Product count & basket value trends over time <br> - Share of product categories per region | Implement personalized retention campaigns and optimize product offerings. |
| **Need Conversion** | Customer segments with high engagement but low purchase conversion | - Avg. time between visits & purchases <br> - Abandoned cart rates by segment | - Top reasons for cart abandonment (price sensitivity, lack of urgency) <br> - Impact of discount strategies on conversion | Optimize conversion funnel through pricing strategies, urgency triggers, and remarketing. |

### 4️⃣ Prototype – Data Preparation & Visualization

#### 1 Data Selection & Structuring  
- Chose relevant tables and fields from the **data dictionary** based on business objectives.  

#### 2 Data Cleaning & Transformation  
- Handled missing values, duplicates, and inconsistencies.  
- Standardized date formats, categorical variables, and numerical fields.  
- Used **SQL for querying and filtering** data efficiently.  
- Applied **Power Query** for data transformation and merging multiple sources.  

#### 3 Data Modeling    
- Created fact and dimension tables to optimize performance.
-  Established Star Schema model.   

#### 4 Developing Measures  
- **Calculated RFM Scores:**  
  - **Recency (R):** Days since last purchase → `Recency = TODAY() - Last_Purchase_Date`  
  - **Frequency (F):** Total purchases → `Frequency = COUNT(DISTINCT Order_ID)`  
  - **Monetary (M):** Total spending → `Monetary = SUM(Total_Spent)`  
  - **Scoring:**  
    - Used **percentile-based ranking** to assign scores from 1 to 5 for Recency (low = better), Frequency, and Monetary (high = better).  
    - Final RFM Score = `R_Score + F_Score + M_Score`.  
    - Stored results in a **Dim table named `Dim_Customer`**, containing customer details and their RFM scores.  
- Applied **DAX** to compute advanced metrics.such as Customer Lifetime Value (CLV), Average Basket Size, and Revenue per Segment.  

#### 5 Data Exploration & Insights Extraction  
- Used **SQL queries** to identify spending trends, customer behaviors, and purchase patterns.  
- Analyzed customer retention, churn risks, and segment distribution.  

#### 6 Choosing Appropriate Visualizations  
- Selected **Treemaps, Scatter Plot, Line Charts, Pie Charts and Bar Graphs** for intuitive insights.    

#### 7 Dashboard Design & Prototyping  
- Organized reports into logical **views (Customer Overview, Segmentation, Location Analysis, Product Analysis).** 
- Applied **consistent color themes** for clear differentiation.  
- Ensured **user-friendly navigation** for stakeholders.  
---

###  6️⃣ Test and Implementation:
- Gathered feedback on prototype dashboards.  
- Refined metrics, filters, and visualization choices for better clarity.  
- Validated accuracy by cross-checking against raw data.

---  
## 4. 📊 Key Insights & Visualizations  

### 🔹 Customer Segmentation  
I categorize customers into four main groups to optimize engagement and revenue:  

- **🏆 Top Value (Champions, Loyal)**: High-CLV customers who drive most of the revenue and exhibit the best purchasing behavior.  
- **🔄 Need Retention (Cannot Lose Them, At Risk, Need Attention, Hibernating)**: Contribute significantly to revenue but are showing signs of disengagement.  
- **🚀 Need Conversion (Promising, Potential Loyalist, New Customer)**: New or developing customers with strong potential to become loyal.  
- **⚠️ No Action (About to Sleep, Lost Customer)**: A large segment with minimal revenue contribution and very low CLV.  

### 🔹 Visualization
#### Segmentation
![image](https://github.com/user-attachments/assets/74f7db09-04d8-4a6c-bb7e-7f6b234d84ed)
#### Top Value Analysis
![image](https://github.com/user-attachments/assets/ecd344e4-9fe1-410b-a8f8-ab62d4901641)
#### Need Retention Analysis
![image](https://github.com/user-attachments/assets/5b340744-a816-48ab-9693-fddd28a19965)
#### Need Conversion Analysis
![image](https://github.com/user-attachments/assets/be12063f-68c8-4f3f-b21b-7fcc7bc7ade9)  


---

## 🔎 5.Final Conclusion & Recommendations

Based on our insights, we recommend the following:
### 🎯 Recommended Strategies  

#### 🏆 **Top Value – Strengthen Loyalty & Engagement**  
- **Tiered Loyalty Programs**: Offer exclusive perks (VIP service, discounts, early access).  
- **Personalized Rewards**: Provide tailored discounts or free products based on preferences.  
- **Exclusive Events**: Organize product previews, webinars, or VIP experiences.  

#### 🔄 **Need Retention – Re-Engage & Prevent Churn**  
- **Reactivation Offers**: Send time-sensitive discounts or free gifts to inactive customers.  
- **Customer Feedback Loop**: Collect insights to enhance offerings and address concerns.  
- **Lapsed Customer Campaigns**: Automate email follow-ups with incentives.  
- **Retargeting Ads**: Remind customers of previous interests or abandoned carts.  
- **Exclusive Memberships**: Introduce subscription perks like discounts and early sales access.  

#### 🚀 **Need Conversion – Encourage Repeat Purchases**  
- **Follow-Up Offers**: Provide post-first-purchase discounts or free shipping.  
- **Onboarding Series**: Educate new customers with product tips and special deals.  
- **Bundling Offers**: Sell product sets at a discount to increase order value.  
- **Time-Limited Discounts**: Use urgency-based promotions to drive purchases.  

### 📌 Key Takeaways

✔️ **Retain High-Value Customers**: Strengthen loyalty programs and offer VIP perks.  
✔️ **Re-engage At-Risk Customers**: Use personalized promotions and feedback loops.  
✔️ **Convert New Customers**: Provide strong incentives for second-time purchases.  
✔️ **Optimize Marketing Efforts**: Focus advertising budget on segments with the highest ROI potential.  

### 📌 Next Steps:

- Implement loyalty programs with tiered rewards.
- Set up automated re-engagement campaigns for at-risk customers.
- Develop a structured onboarding journey for new customers.
- Continuously analyze customer behavior to refine strategies.

