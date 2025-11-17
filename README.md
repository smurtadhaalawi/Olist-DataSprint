# Olist-DataSprint
This project explores and Clean Brazil’s e-commerce ecosystem, focusing on Olist’s marketplace model, seller acquisition, and performance dynamics between resellers and manufacturers. It includes research on e-commerce trends, regulatory requirements, seller evaluation strategies, and a full analytical assessment of seller performance.

## What is e-commerce and the B2B model Olist implements
From the main page of Olist you can see that Olist "is an e-commerce platform that connects small and medium-sized businesses to customers. The platform operates as a marketplace, where merchants can list their products and services and customers can browse and purchase them online."

## Scenario
As Olist expands its marketplace, it faces growing challenges in recruiting reliable sellers. With thousands of new applicants each month, the current onboarding process struggles to filter out sellers who may cause operational issues such as late deliveries, inaccurate product details, poor packaging, and inconsistent pricing—all of which impact customer satisfaction and Olist’s reputation.

To address these risks, Olist plans to introduce a Seller Vetting & Scoring System to evaluate sellers before approval. This system will ensure only compliant, capable, and high-performing sellers join the marketplace.

## The scoring model will consider

### 1. Reviews
Measures customer satisfaction and service consistency. Higher scores reflect strong product quality, responsiveness, and issue resolution.
We had make category for reviews to know seller Performance

<pre><code> IF { FIXED [Seller Id] : AVG([Review Score]) } >= 4
   AND { FIXED [Seller Id] : COUNT([Review Id]) } >= 5 THEN "Top Seller"

ELSEIF { FIXED [Seller Id] : AVG([Review Score]) } < 3
   AND { FIXED [Seller Id] : COUNT([Review Id]) } >= 5 THEN "Bad Seller"

ELSE "Normal"
END </code></pre>

### 2. Product Presentation Quality
Evaluate product images quantity , descriptions length and compliance with standards.
Good presentation reduces returns, increases conversion, and builds buyer trust.

Product Images Quantity:
1. Below min less 1- 2
2. Minimum Required: 3-5 photos
3. Recommended Standard: 6 -7 photos
4. Best Practice 8 to 10 photos
5. more than Needed above 10

Product Descriptions Length
1. Below min less 300
2. Minimum Required: 300 -700
3. Recommended Standard: 700 - 2000
4. Best Practice 2000- 2500
5. more than Needed above 2500

### 3. Delivery Performance
Assesses Delivery speed, on-time delivery rate, late delivery rate and cancellation rate.
Ensures operational reliability and prevents negative customer experiences.

Calculates the number of days between Estimated Delivery Date and Delivered Customer Date:
<pre><code>DATEDIFF('day', [Order Estimated Delivery Date], [Order Delivered Customer Date])</code></pre>

categorize them:
<pre><code>IF [Delay] > 0 THEN "Late"
ELSEIF [Delay] = 0 THEN "On Time"
ELSE "Early"
END</code></pre>

### 4. Revenue, and price Performance
Compares reseller and manufacturer prices, category averages, and historical pricing behavior.

<pre><code>IF [Revenue for each Seller] >= { FIXED : PERCENTILE([Revenue for each Seller], 0.75) } THEN
    "High Revenue"
ELSEIF [Revenue for each Seller] >= { FIXED : PERCENTILE([Revenue for each Seller], 0.25) } THEN
    "Medium Revenue"
ELSE
    "Low Revenue"
END</code></pre>

This calculates how many months have passed since the seller’s last order until Dec 31, 2018:

<pre><code>IF DATEDIFF('month', [Last Order Date ], DATE("2018-12-31")) > 3 THEN
    "Underperforming"
ELSE
    "Active"
END</code></pre>

## Approaches

1. How do sellers perform in terms of revenue, delivery, product descriptions, and review Performance?
2. Who are the most successful sellers?
3. Who are the red-flag (underperforming) sellers?
4.	Are employees hiring/selecting the right sellers?
5.	How do sellers compare across business types?
6.	How do resellers perform compared to manufacturers in terms of revenue, delivery, and review Performance?
7.	Are resellers the most successful sellers?
8.	What is the price gap between resellers and manufacturers?

## Recommendation 
- Evaluate every seller using a unified scoring model based on: Delivery accuracy, Product description completeness, Review performance, and Revenue consistency
- Prioritize High-Performing Sellers:High average review scores, Low late-delivery rates, Accurate product descriptions.
- Strengthen Verification for Low-Scoring Sellers:Require additional documentation, Conduct manual checks, Delay approval until key issues are resolved.
- Provide Training & Guidelines for Product Quality:Teach sellers how to write clear product descriptions, Standardize product image requirements, and Educate sellers on delivery expectations.
- Introduce Continuous Monitoring: Late delivery rates, Cancelled orders, Customer complaint patterns, Changes in monthly performance.
- Improve Employee Selection Decisions with Data: Giving staff access to the seller-scoring dashboard to Reduce subjective or manual decisions.
- Prioritize onboarding sellers with strong sales potential.
- Support resellers after 6 months to stay active.
- Reward employees for quality sellers, not quantity.

## Limitation 
- The dataset covers 2016–2018, so it may not reflect very recent seller behavior.
- Seller type data (manufacturer vs. reseller) appears only in the 2018 Closing Deals file with 825 seller IDs, while the main seller table contains around 3,000 sellers. Therefore, we built our analysis using the full seller table and treated the 825 sellers as a representative sample.
- Revenue Does Not Reflect Profitability
- Delivery Data Missing External Factors

## Reference:
The Data taken from Kaggle:
[https://www.kaggle.com/datasets/olistbr/marketing-funnel-olist?select=olist_closed_deals_dataset.csv]
[https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce]

depend on this articale Jennie Wilson said "An effective e-commerce product description should typically be concise yet informative, aiming for around 150 to 300 words."
[https://www.invensis.net/blog/write-engaging-ecommerce-product-descriptions?utm_source=chatgpt.com]
depend on this articale Miranda Gabbott Said "For fashion and accessories, aim for 5 to 8 images, so customers can get an idea of fit, and For items that are complex and/or expensive, such as furniture and electronics, you should include 6 to 10 images."
[https://omi.so/resources/blog/product-images-best-practices?utm_source=chatgpt.com]
