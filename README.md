### RFM Analysis_SQL/ Power BI

# 📑 Table of Contents

1. 📌 Background & Overview
2. 📂 Dataset Description & Data Structure
3. 🧠 Design Thinking Process
4. 📊 Key Insights & Visualizations
5. 🔎 Final Conclusion & Recommendations

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

---

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

---

#### **Dim_ProductCategory (Production.ProductCategory)**  
Provides a high-level classification of products.  

| **Column Name**      | **Data Type**     | **Description**  |
|----------------------|------------------|------------------|
| `ProductCategoryID`   | int              | Primary key identifying product category.  |
| `Name`               | nvarchar(50)     | Descriptive name of the category.  |
| `rowguid`           | uniqueidentifier  | Unique identifier for replication.  |
| `ModifiedDate`      | datetime         | Last modification timestamp.  |

---

#### **Dim_ProductSubcategory (Production.ProductSubcategory)**  
Represents a more granular classification of products under each category.  

| **Column Name**      | **Data Type**     | **Description**  |
|----------------------|------------------|------------------|
| `ProductSubcategoryID` | int              | Primary key identifying subcategory.  |
| `ProductCategoryID`   | int              | Foreign key linking to ProductCategory.  |
| `Name`               | nvarchar(50)     | Descriptive name of the subcategory.  |
| `rowguid`           | uniqueidentifier  | Unique identifier for replication.  |
| `ModifiedDate`      | datetime         | Last modification timestamp.  |

---

#### **Dim_Location (Production.Location)**  
Represents the geographical locations relevant to sales and operations.  

| **Column Name**  | **Data Type**   | **Description**  |
|-----------------|---------------|------------------|
| `LocationID`     | smallint       | Primary key identifying each location.  |
| `Name`          | nvarchar(50)   | Location name/description.  |
| `CostRate`      | smallmoney     | Standard hourly cost of the location.  |
| `Availability`  | decimal(8,2)   | Work capacity (in hours).  |
| `ModifiedDate`  | datetime       | Last modification timestamp.  |

---

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

---

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

---

### **Data Relationships & Schema Design**  
The **Fact_Sales** table is at the center of the star schema, connecting to multiple dimension tables via **foreign key relationships**
---

## 🧠 Design Thinking Process  

### 1️⃣ Empathize – Understanding Stakeholders’ Needs  

To build an effective customer segmentation dashboard, we first engaged with key stakeholders to understand their challenges and expectations. Using the **5W1H framework** and an **Empathy Map**, we captured insights that shape our approach to **RFM-based segmentation**.  

#### 🎯 Primary Stakeholder:  
**Sales & Marketing Managers** – The end users of the dashboard, responsible for designing targeted strategies to improve retention, revenue, and engagement.  

#### ⚠️ Key Challenge:  
There is no structured approach to understanding customer behavior and value, making it difficult to personalize marketing efforts and allocate budgets effectively.  

---

### 📌 5W1H Analysis  

| **Question** | **Insight** |
|-------------|------------|
| **Who will use the dashboard?** | Sales & Marketing Managers looking to identify valuable customers and optimize retention & acquisition strategies. |
| **What problem does it solve?** | Provides customer segmentation based on RFM (Recency, Frequency, Monetary) to prioritize high-value customers and re-engage at-risk ones. |
| **When and where will it be used?** | - In monthly strategy meetings to assess customer behavior.<br>- During marketing campaign planning to define targeting strategies.<br>- In sales forecasting and budgeting to allocate resources efficiently. |
| **Why is it needed?** | - Marketing struggles with broad, ineffective targeting.<br>- Sales lacks insights on which customers to nurture or recover.<br>- Customer retention is low due to untailored engagement. |
| **How has the problem been handled before?** | - Marketing used generalized campaigns without personalized offers.<br>- Sales relied on manual customer tracking, leading to inefficiencies.<br>- Decisions were based on intuition rather than data, causing missed opportunities. |

---

### 🗺️ Empathy Map – Understanding Stakeholder Perspectives  

| **Aspect** | **Insights from Stakeholders** |
|-----------|--------------------------------|
| **Thinking & Feeling** | "How do we identify customers with the highest potential?"<br>"Which groups need different engagement approaches?"<br>"How do we prevent customer churn?" |
| **Seeing** | Fragmented customer transaction data without clear segmentation.<br>Generic marketing campaigns with low conversion rates. |
| **Saying & Doing** | "We need more precise targeting for promotions."<br>"We should focus on retaining our most valuable customers."<br>"Churn prediction would help us act before losing customers." |
| **Pain Points** | - Marketing budget is wasted on low-engagement customers.<br>- No clear understanding of which customers contribute most to revenue.<br>- Difficulty in crafting personalized retention strategies. |
| **Gains** | - Improved customer lifetime value (CLV) through better segmentation.<br>- Increased marketing ROI with more precise targeting.<br>- Data-driven insights to reduce churn and improve loyalty. |

---

### 🔄 Stakeholder Journey – How Insights Drive Action  

| **Step** | **Description** |
|---------|---------------|
| **Step 1** | Marketing & Sales teams recognize a need for data-driven customer segmentation. |
| **Step 2** | They identify pain points: ineffective targeting, low retention, and unclear customer value. |
| **Step 3** | The Data Analytics team proposes an RFM-based segmentation approach. |
| **Step 4** | Collaboration ensures the dashboard provides actionable insights, such as high-value customer lists and churn risks. |
| **Step 5** | The dashboard is tested and refined based on real business use cases. |
| **Step 6** | Sales & Marketing teams integrate the insights into campaign planning and retention strategies. |

---
### ⚒️ Main Process:

1️⃣ **Data Cleaning & Preprocessing**:

- Removed missing values, duplicates, and inconsistent data.
- Converted date formats and standardized categorical variables.

2️⃣ **Exploratory Data Analysis (EDA):**

- Analyzed spending patterns across different customer groups.
- Identified trends in purchase frequency and customer lifetime value (CLV).

3️⃣ **Customer Segmentation Using RFM Analysis:**

- Categorized customers into segments based on their Recency, Frequency, and Monetary values.
- Applied clustering techniques to validate segment definitions.

4️⃣ **Strategy Development:**

- Proposed marketing and retention strategies for each customer segment.

---

## 📊 Key Insights & Visualizations

### 🔍 Dashboard Preview

1️⃣ **Customer Distribution by Segment**  
✔️ Visualized how customers are distributed across different RFM segments.  
✔️ Identified which segments contribute the most to revenue.  

2️⃣ **Customer Spending Behavior Over Time**  
✔️ Highlighted spending trends and seasonal fluctuations.  
✔️ Showed peak sales periods and customer purchase frequency.  

3️⃣ **Retention & Churn Analysis**  
✔️ Identified customers at risk of churn and their characteristics.  
✔️ Provided insights on how to re-engage lost customers.  

### 📌 Analysis 1: Customer Segments

- **Observation**: The majority of revenue comes from ‘Champions’ and ‘Loyal’ customers.
- **Recommendation**: Implement tiered loyalty programs to retain these high-value customers.

### 📌 Analysis 2: At-Risk Customers

- **Observation**: A significant portion of revenue is lost due to customers shifting into the ‘At Risk’ segment.
- **Recommendation**: Use re-engagement campaigns with personalized offers and exclusive discounts.

### 📌 Analysis 3: New Customer Conversion

- **Observation**: Many new customers make one-time purchases and don’t return.
- **Recommendation**: Offer onboarding discounts, product bundles, and email sequences to drive repeat purchases.

---

## 🔎 Final Conclusion & Recommendations

Based on our insights, we recommend the following:

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

This structured approach ensures data-driven decision-making and maximizes customer lifetime value. 🚀

## II/ Visualization
### 1. Segmentation
![image](https://github.com/user-attachments/assets/74f7db09-04d8-4a6c-bb7e-7f6b234d84ed)
### 2. Top Value Analysis
![image](https://github.com/user-attachments/assets/ecd344e4-9fe1-410b-a8f8-ab62d4901641)
### 3. Need Retention Analysis
![image](https://github.com/user-attachments/assets/5b340744-a816-48ab-9693-fddd28a19965)
### 4. Need Conversion Analysis
![image](https://github.com/user-attachments/assets/be12063f-68c8-4f3f-b21b-7fcc7bc7ade9)
## III/ Insights and Recommendations
We can gather the customer segments into 4 main groups to take action:  
- **Top value**: Champions, Loyal -> contribute most of the revenue to the company, has supreme CLV and show the best buying behavior
- **Need retention**: Cannot Loose Them, At Risk, Need Attention, Hibernating -> contribute quite considerably to the company revenue, has quite hight CLV and we are loosing them
- **Need Conversion**: Promising, Potential Loyalist, New Customer -> new users that contribute quite noteworthy can be turned into Champions or Loyal
- **No action**: About to sleep, Lost Customer -> consist of quite large number of customers but do not contribute much to the company revenue and has extremely low CLV

From the above groups, I suggest the strategy for each group:
- **Top value**: Implement exclusive loyalty programs or rewards to recognize and appreciate their continued support
    + Tiered Loyalty Programs: Reward customers based on spending, with higher tiers unlocking exclusive perks like early product access, VIP service, or discounts.
    + Personalized Rewards: Offer discounts or free items based on customers' favorite products.
    + Exclusive Events: Host webinars, product previews, or in-person events to strengthen brand relationships
- **Need retention**: Use personalized communication and offers to re-engage these customers.
    + Reactivation Offers: Create offers that are sent exclusively to customers who have not made a purchase recently. These offers could include a significant discount or a free gift with their next purchase that align with their previous behavior.
    + Customer Feedback Loop: Reach out to these customers to ask for feedback on their experience with the brand. Use their responses to improve products or services and communicate the changes made based on their input.
    + Lapsed Customer Campaigns: Set up automated email campaigns that trigger when a customer hasn't interacted with the brand for a certain period.
    + Retargeting Ads: Use social media platforms to retarget these customers with ads that feature products they’ve shown interest in or remind them of items left in their cart.
    + Exclusive Memberships: Offer a subscription or membership program with perks such as regular discounts, early access to sales, or free shipping.
- **Need Conversion**: Focus resources on this group, provide introductory offers or discounts to encourage second-time purchases and increase engagement, spending with the brand.
    + Follow-Up Offers: Send a discount or bonus after the first purchase to encourage a second one. This could be a discount, free shipping, or a bonus item to encourage them to return quickly.
    + Onboarding Series: Develop an onboarding email series that welcomes new customers and educates them about products or services. Include tips, product recommendations, and special offers to guide them towards their next purchase.
    + Bundling Offers: Offer bundles of products at a discounted price to encourage these customers to try multiple items. This can exposes them to a wider range of the company offerings.
    + Time-Limited Discounts: Provide time-limited discounts that create a sense of urgency for these customers to make their next purchase.
