# RFM Customer Behavior Analysis_SQL/ Power BI

# 📑 Table of Contents

1. [📌 Background & Overview](#-background--overview)  
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)  
3. [🧠 Design Thinking Process](#-design-thinking-process)  
4. [📊 Key Insights & Visualizations](#-key-insights--visualizations)  
5. [🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)  

---

# 📌 Background & Overview

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

# 📂 Dataset Description & Data Structure

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

# 🧠 Design Thinking Process  

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


###  6️⃣ Test and Implementation:
- Gathered feedback on prototype dashboards.  
- Refined metrics, filters, and visualization choices for better clarity.  
- Validated accuracy by cross-checking against raw data.

---  
# 4. 📊 Key Insights & Visualizations  

## 🔹 Customer Segmentation  
I categorize customers into four main groups to optimize engagement and revenue:  

- **🏆 Top Value (Champions, Loyal)**: High-CLV customers who drive most of the revenue and exhibit the best purchasing behavior.  
- **🔄 Need Retention (Cannot Lose Them, At Risk, Need Attention, Hibernating)**: Contribute significantly to revenue but are showing signs of disengagement.  
- **🚀 Need Conversion (Promising, Potential Loyalist, New Customer)**: New or developing customers with strong potential to become loyal.  
- **⚠️ No Action (About to Sleep, Lost Customer)**: A large segment with minimal revenue contribution and very low CLV.  

## 🔹 Visualization
## 📊 Overview  
![Image](https://github.com/user-attachments/assets/7336d8a2-e202-4ee2-bd36-0e20123fa233)  

### 1️⃣ Business Performance Overview  
- Total revenue: $20.06M  
- Total customers: 19.12K  
- Average order value (AOV): $1.71K  
- 100% active customers → No inactive buyers  
- Repeat purchase rate: 10%  

✅ **Implication:**  
- The business is generating significant revenue with a strong customer base.  
- The repeat purchase rate is low, suggesting opportunities for retention strategies.  

### 2️⃣ Recency Distribution – Customer Engagement  
- Customers mostly repurchase within 50 days.  
- Average recency: ~2.59 months (~78 days).  

✅ **Actionable Insights:**  
- Shorten the time between repeat purchases through email campaigns & loyalty rewards.  

### 3️⃣ Frequency Distribution – Purchase Behavior  
- Most customers purchase only once or twice.  
- Average purchase frequency: 1.294 times per year.  

✅ **Growth Opportunity:**  
- Target low-frequency buyers with personalized discounts to increase order frequency.  

### 4️⃣ Monetary Distribution – High-Value Segments  
- Revenue is concentrated in the $50K-$100K range.  
- High-value customers contribute disproportionately to total revenue.  

✅ **Strategic Focus:**  
- Upsell premium products to high-value customers.  
- Offer bundles, financing, or VIP perks to encourage larger transactions.  

### 5️⃣ Customer Segments by RFM Scores  
✅ **Retention Plan:**  
- High R-score customers (5, 4) are engaged.  
- Low R-score customers (1, 2) need reactivation strategies (email re-engagement, special offers).  

✅ **Growth Strategy:**  
- Convert first-time buyers into repeat customers with loyalty programs & email nurturing.  
- Higher F-score segments (4, 5) are the best retention targets for personalized upsells.  

✅ **Revenue Maximization Plan:**  
- Upsell & cross-sell strategies for M-score 5 & 4 customers.  
- Low M-score buyers need pricing incentives & promotional bundles to increase order value.


## 📊 Segmentation
![Image](https://github.com/user-attachments/assets/82b89754-1ac8-4fe0-82db-aa739e562072)

### 1️⃣ Key Metrics  
- Average recency: 190 days  
- Average frequency: 1.6 purchases  
- Average monetary value: $5.75K  

✅ Implication:  
- Customers take a long time to repurchase (high recency).  
- Most customers have low purchase frequency.  
- A small segment contributes significantly to revenue.  

### 2️⃣ RFM Customer Segments  
- Top Value: High spenders with frequent purchases.  
- Need Retention: At risk of churning, require engagement.  
- Need Conversion: First-time buyers needing conversion efforts.  
- No Action: Low-value, infrequent buyers.  

✅ Actionable Insights:  
- Champions & Loyalists: Maintain engagement with exclusive offers.  
- At-Risk & Need Attention: Use reactivation emails & discounts.  
- Lost & Hibernating: Reconnect through special promotions.  

### 3️⃣ Revenue Contribution by Group  
- Top Value customers generate 64.3% of total revenue.  
- Need Retention accounts for 26.9%.  
- Need Conversion contributes 8.8%.  
- No Action segment has minimal impact.  

✅ Growth Opportunity:  
- Prioritize Top Value customers for upselling premium products.  
- Convert Need Retention customers with loyalty rewards.  
- Offer discounts to Need Conversion customers to encourage repeat purchases.  

### 4️⃣ Recency, Frequency & Monetary Analysis  
- Top Value customers repurchase within 105 days on average.  
- Need Retention takes longer (293 days).  
- Frequency is highest (5x) for Top Value customers.  
- Average order value is $69K for Top Value vs. $5K for Need Retention.  

✅ Strategic Focus:  
- Reduce recency for Need Retention customers with reminder emails & incentives.  
- Encourage more frequent purchases through product recommendations.  
- Increase order value with bundle deals & VIP perks for loyal buyers.  

## 📢 Top Value Customer Analysis  
![Image](https://github.com/user-attachments/assets/fb3f4244-456c-402c-9062-0a2df72057a6)

### 1️⃣ Overview  
- Total revenue: $12.90M  
- Total customers: 1.04K  
- Average order value (AOV): $9.26K  
- Active customers: 100%  
- Repeat purchase rate: 34%  

### 2️⃣ Sales Performance  
#### Sales trend  
- Revenue peaked at $5.8M in February (+71% YoY).  
- Decline in June, indicating potential seasonal impact.  

#### Sales by country  
- United Kingdom: Highest growth (+65.5%).  
- Australia: Fastest-growing market (+244.1%).  
- Canada & Central: Declining revenue (-25% to -39%).  

### 3️⃣ Customer Value by Country  
- Southwest & Northwest: Highest average order value.  
- United Kingdom & Germany: Moderate frequency, lower value.  
- France & Australia: Lower frequency and order value.  

### 4️⃣ Product Performance  
#### Top revenue products  
- Touring bikes & road bikes lead in sales.  
- Mountain bikes generate significant revenue.  

#### Top growth products  
- Touring bikes: Fastest-growing product category.  
- Mountain frames & road bikes: Steady growth.  

### 5️⃣ Key Takeaways  
✅ Prioritize Australia & UK for expansion due to high growth.  
✅ Address decline in Canada & Central with targeted promotions.  
✅ Leverage high-value segments in Southwest & Northwest.  
✅ Boost sales of touring bikes with marketing campaigns.  

## 📢 Customer Retention Analysis  
![Image](https://github.com/user-attachments/assets/812c2593-4a94-42e8-8dd1-adf6859dce3e)

### 1️⃣ Overview  
- Total revenue: $1.77M  
- Total customers: 6.13K  
- Average order value (AOV): $1.19K  
- Active customers: 100%  
- Repeat purchase rate: 10%  

### 2️⃣ Sales Performance  
#### Sales trend  
- Revenue peaked at $0.77M in January (+17% YoY).  
- Declined sharply after February, with a -55% drop in June.  

#### Sales by country  
- Germany & UK: Some growth (+33.8%, +30.3%).  
- Australia: Only market with positive YoY growth (+14.7%).  
- Canada & Southwest: Significant revenue loss (-84.3%, -66.7%).  

### 3️⃣ Customer Value by Country  
- Northeast: Highest average order value.  
- Australia, France & UK: Strong customer value.  
- Canada & Central: Low order value and purchase frequency.  

### 4️⃣ Product Performance  
#### Top revenue products  
- Mountain bikes, touring bikes & road bikes generate the most revenue.  
- Jerseys & helmets contribute smaller but steady sales.  

#### Top growth products  
- Touring bikes: Highest growth rate (~400%).  
- Helmets: Emerging as a high-growth product.  

### 5️⃣ Key Takeaways  
✅ Improve retention efforts in Canada & Southwest.  
✅ Explore growth opportunities in Australia & Germany.  
✅ Boost sales of high-growth products like helmets & touring bikes.  
✅ Investigate why repeat purchases remain low despite high active customer rates.  

## 📢 Customer Retention Analysis  
![Image](https://github.com/user-attachments/assets/aa41c632-3c6f-4653-b5e3-a19422435f97)
### Overview  
- Total revenue: $20.06M  
- Total customers: 19.12K  
- Average order value (AOV): $1.71K  
- Active customers: 100%  
- Repeat purchase rate: 10%  

### Sales Performance  
#### Sales trend  
- Strong start in January ($4.3M, +105% YoY).  
- Peak revenue in March ($7.2M, +64% YoY).  
- Decline in June, hitting $0M revenue.  

#### Sales by country  
- Strongest growth: Australia (+90.4%) and Northwest (+29%).  
- Canada (-19%) and Central (-35%) showed declining revenue.  

### Customer Value by Country  
- Central & Northeast: Highest average order value.  
- UK & Canada: Low order value and purchase frequency.  

### Product Performance  
#### Top revenue products  
- Road bikes, touring bikes, and mountain bikes generate the most revenue.  
- Mountain frames & touring frames contribute smaller but steady sales.  

#### Top growth products  
- Touring bikes lead with the highest growth rate.  
- Road bikes & mountain bikes show consistent growth.  

### Key Takeaways  
- Address declining performance in Canada & Central.  
- Leverage strong growth in Australia & Northwest.  
- Promote high-growth products like touring bikes.  
- Explore strategies to improve repeat purchases.  



## 🔎 5. Final Conclusion & Recommendations  

Based on our insights, the following strategies are recommended to drive retention, engagement, and revenue growth.  

### 🎯 Recommended Strategies  

#### 🏆 **Maximizing High-Value Customers** (Top Value)  
**Insight:** Central and Northeast regions have the highest average order value, while road bikes and touring bikes generate the most revenue.   
- **Loyalty & Engagement Tactics:**  
- **VIP Loyalty Program**: Offer tiered benefits (e.g., exclusive discounts, early access) to reward repeat customers.  
- **High-Value Product Bundles**: Create special offers combining top revenue products (e.g., road & touring bikes) to drive upsells.  
- **Exclusive Events & Webinars**: Engage premium customers with special previews, early product launches, and expert-led sessions.  

#### 🔄 **Re-Engaging At-Risk Customers** (Need Retention)  
**Insight:** Canada and Central regions show declining revenue (-19%, -35%), and repeat purchase rates are low (10%).  
- **Retention & Recovery Strategies:**  
- **Reactivation Discounts**: Send personalized discounts or free shipping offers to inactive customers.  
- **Customer Feedback Loop**: Collect insights through surveys to identify pain points and refine offerings.  
- **Lapsed Customer Campaigns**: Automate follow-ups with tailored incentives for customers who haven't purchased in a while.  
- **Geo-Specific Promotions**: Deploy region-focused campaigns in Canada and Central to counter declining sales.  

#### 🚀 **Encouraging Repeat Purchases** (Need Conversion)  
**Insight:** Most buyers purchase only once, and touring bikes show the highest growth potential.  
- **Conversion-Boosting Tactics:**  
- **Post-Purchase Incentives**: Offer time-limited discounts or loyalty points to drive second-time purchases.  
- **Personalized Recommendations**: Suggest complementary products (e.g., accessories for road bikes) based on previous purchases.  
- **Bundling Offers**: Promote high-growth products like touring bikes with accessories or maintenance kits.  
- **Limited-Time Promotions**: Implement urgency-driven campaigns (e.g., flash sales, seasonal discounts) to increase conversion rates.  

### 📌 Key Takeaways  

✔️ **Retain High-Value Customers** → Strengthen loyalty programs & offer tiered benefits.  
✔️ **Re-Engage At-Risk Customers** → Target Canada & Central with personalized campaigns.  
✔️ **Convert First-Time Buyers** → Use incentives, product bundling & retargeting ads.  
✔️ **Optimize Marketing Spend** → Focus on segments with the highest purchase potential.  

## **📌 Final Conclusion & Recommendations – Overview**  

| **Focus Area** | **Key Insights** | **Recommended Actions** |  
|--------------|----------------|----------------|  
| **Revenue Growth** | $20.06M total revenue, strong sales in Australia & Northwest. | Expand high-value product promotions in top-performing regions. |  
| **Customer Retention** | 10% repeat purchase rate, declining sales in Canada & Central. | Launch reactivation campaigns & geo-targeted promotions. |  
| **Purchase Frequency** | Majority are one-time buyers, touring bikes show strong growth. | Implement follow-up incentives & personalized product recommendations. |  
| **Customer Segmentation** | High-value customers spend more; repeat buyers drive profitability. | Develop CLV-based targeting to maximize retention. |  


## **🚀 Next Steps for the Business**  
1️⃣ **Boost repeat purchases** → Implement loyalty rewards, post-purchase discounts & follow-up campaigns.  
2️⃣ **Improve retention in weak regions** → Focus on Canada & Central with geo-specific offers.  
3️⃣ **Increase revenue per customer** → Promote high-value bundles & premium loyalty perks.  
4️⃣ **Enhance conversion strategies** → Retarget first-time buyers & optimize personalized recommendations.  

