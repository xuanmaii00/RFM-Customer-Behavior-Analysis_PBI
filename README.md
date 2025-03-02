# RFM Customer Behavior Analysis_Power BI

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

To effectively analyze sales performance and customer behavior, the dataset was structured into a **star schema**, with **Sales.SalesOrderDetail** serving as the **Fact_Sales** table. This central fact table connects to multiple **dimension tables** to enhance data granularity and improve analytical capabilities. 

The **Fact_Sales** table is at the center of the star schema, connecting to multiple dimension tables via **foreign key relationships**

![Image](https://github.com/user-attachments/assets/6a2412ee-33b1-4120-bbb5-baa564505af9)

---

# 🧠 Design Thinking Process  

### 1️⃣ Empathize – Understanding Stakeholders’ Needs  

##### 🔍 5W1H Analysis 
![Image](https://github.com/user-attachments/assets/ee0c09c3-0873-4c8c-b921-8d42706e8161)


##### 📝 Empathy Map – Understanding Stakeholder Perspectives  
![Image](https://github.com/user-attachments/assets/b43c085e-1134-4f8e-8f4a-4a9bcabf7607)


### 2️⃣ Define – Establishing Key Metrics & Growth Formula  

#### 🎯 North Star Metric: Defining Success  

![Image](https://github.com/user-attachments/assets/1f47d771-a844-4358-8c9c-4e3060c6a0ca)

#### 📌 Define Point Of View  

![Image](https://github.com/user-attachments/assets/bc4497a6-19e8-4c1e-9db6-624a26ca71ca)

#### 📈 Why Choose These Metrics?  
- **Clear & Measurable**: RFM scoring is easy to understand and implement.  
- **Predictive Power**: Helps anticipate future customer behavior.  
- **Strategic Value**: Provides actionable insights for marketing, sales, and retention strategies.  


### 3️⃣ Ideate – Developing Actionable Insights  

![Image](https://github.com/user-attachments/assets/b47d3115-28ac-427d-8be5-5c76aab335d2)


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

## 📢 Customer Conversion Analysis  
![Image](https://github.com/user-attachments/assets/424119f7-eb23-468f-9326-7387db72c27b)
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

---

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

