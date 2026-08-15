# DSA3050-PowerBI-LisaMbaire-672034
# Dataset Source 
https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce
# Dataset Description
The Brazilian E-Commerce Public Dataset by Olist is a real, anonymized dataset containing approximately 100,000 customer orders made through the Olist Store between 2016 and 2018. The dataset provides a comprehensive view of e-commerce transactions in Brazil, including information on customers, orders, products, sellers, payments, shipping, customer reviews, and geographical locations. The data is organized into multiple interconnected tables, allowing individual orders to be analyzed from different perspectives, such as sales performance, customer behavior, product characteristics, payment methods, delivery performance, and customer satisfaction.
# Business Problem
Olist is a marketplace that connects thousands of small and medium-sized sellers to customers across Brazil. While the platform has experienced significant growth, the company faces challenges in:

1. **Delivery Performance:** Customers in different regions experience varying delivery times, impacting satisfaction and repeat purchases
2. **Customer Satisfaction:** Understanding what drives high vs. low review scores across product categories and regions
3. **Operational Efficiency:** Identifying bottlenecks in the order fulfillment process from purchase to delivery
4. **Revenue Optimization:** Understanding which product categories, sellers, and regions generate the most revenue
5. **Seller Performance:** Identifying top and underperforming sellers to provide targeted support

**Business Goals:**
- Improve delivery performance by identifying regions with chronic delays
- Increase customer satisfaction by understanding factors that influence review scores
- Optimize product mix by analyzing category performance
- Support sellers with data-driven insights on their performance
- Reduce order cancellations by analyzing patterns in canceled orders

# Power Query Transformations
### SECTION B: POWER QUERY TRANSFORMATIONS

I performed 8 significant transformations to clean and prepare the Olist e-commerce dataset for analysis.

---

#### Transformation 1: Changing Data Types

**Problem:** All columns were imported as text by default, preventing calculations and date-based analysis.

**Transformation:** Used the "Change Type" option in Power Query Editor to set appropriate data types:
- Numeric columns (price, freight_value, payment_value) → **Decimal Number**
- Date columns (order_purchase_timestamp, order_delivered_customer_date) → **Date/Time**
- ID columns → **Text**
- Review scores → **Whole Number**

**Reason:** Correct data types are essential for:
- Performing mathematical calculations (SUM, AVERAGE)
- Using time intelligence functions (YEAR, MONTH, QUARTER)
- Establishing relationships between tables
- Creating accurate visualizations

**Result:** All columns now have the correct data type, enabling calculations and date-based analysis.

#### Transformation 2: Trimming Text Columns

**Problem:** Many text fields had leading or trailing spaces that could cause issues when matching values during merges or filtering.

**Transformation:** Selected all text columns (order_id, customer_id, seller_id, product_id, payment_type, etc.) and used **Transform → Trim** to remove extra spaces.

**Reason:** Trimming ensures consistent text values, which is critical for:
- Successful table merges using text keys
- Accurate filtering and grouping
- Preventing duplicate values that look different but are actually the same

**Result:** All text fields are now clean with no leading or trailing spaces.

#### Transformation 3: Standardizing Categorical Values

**Problem:** The `order_status` column contained inconsistent values such as "delivered", "Delivered", "DELIVERED" which would appear as separate categories in analysis.

**Transformation:** Used **Transform → Replace Values** to standardize all status values to Proper Case:
- "delivered" → "Delivered"
- "shipped" → "Shipped"
- "canceled" → "Canceled"
- "processing" → "Processing"
- "invoiced" → "Invoiced"
- "approved" → "Approved"
- "created" → "Created"

Also standardized payment types using **Transform → Format → UPPERCASE**:
- "credit_card" → "CREDIT_CARD"
- "boleto" → "BOLETO"
- "voucher" → "VOUCHER"

**Reason:** Consistent categorical values are essential for:
- Accurate grouping in charts and tables
- Clear slicer and filter options
- Professional-looking visualizations

**Result:** All categorical fields now have consistent formatting.

#### Transformation 4: Handling Missing (Null) Values

**Problem:** Several columns contained null values that would cause errors in calculations and analysis:
- `review_comment_title` had null values
- `review_comment_message` had null values
- `product_category_name` had null values
- `customer_city` and `customer_state` had null values

**Transformation:** Used **Replace Values** to replace null with descriptive text:
- `review_comment_title` null → "No Title"
- `review_comment_message` null → "No Review"
- `product_category_name` null → "Unknown"
- `customer_city` null → "Unknown City"
- `customer_state` null → "Unknown State"

**Reason:** Null values can cause:
- Errors in visualizations and calculations
- Incomplete analysis results
- Issues with filtering and grouping
- Poor user experience in dashboards

**Result:** All null values have been handled, allowing complete analysis without errors.

#### Transformation 5: Extracting Date Parts

**Problem:** The `order_purchase_timestamp` contained complete date/time information, but analysis required Year, Month, and Quarter as separate fields for time-based analysis.

**Transformation:** Used **Add Column → Date → Year/Month/Quarter** to extract components:
- Extracted **Year** from order_purchase_timestamp
- Extracted **Month Name** from order_purchase_timestamp
- Extracted **Month Number** from order_purchase_timestamp
- Extracted **Quarter** from order_purchase_timestamp

**Reason:** Extracted date parts enable:
- Year-over-year comparison
- Monthly trend analysis
- Seasonal pattern identification
- Custom time-based filtering in dashboards

**Result:** Four new columns (Order_Year, Order_Month_Name, Order_Month_Number, Order_Quarter) enable flexible time-based analysis.

#### Transformation 6: Created Conditional Columns

**Problem:** Numeric review scores (1-5) needed to be categorized into meaningful groups for easier interpretation by business users.

**Transformation:** Used **Add Column → Conditional Column** to create `Review Category`:
- If review_score = 5 → "Excellent"
- Else If review_score = 4 → "Good"
- Else If review_score = 3 → "Average"
- Else If review_score = 2 → "Poor"
- Else → "Very Poor"
#### Transformation 7: Created `On Time Delivery` conditional column:

**Reason:** Conditional columns:
- Make data more understandable for non-technical users
- Enable grouping and segmentation in analysis
- Simplify visualization and dashboard design

**Result:** Two new categorical columns make analysis more intuitive.

#### Transformation 8: Merging Queries (Table Joins)

**Problem:** Data was spread across 7 different CSV files (orders, order items, products, translation, customers, payments, reviews, sellers), making analysis impossible without combining them.

**Transformation:** Used **Merge Queries** to combine all tables into one fact table (`FactOrders`):
- **Merge 1:** Orders + Order Items (using `order_id`)
- **Merge 2:** Order Items + Products (using `product_id`)
- **Merge 3:** Products + Translation (using `product_category_name`)
- **Merge 4:** Orders + Customers (using `customer_id`)
- **Merge 5:** Orders + Payments (using `order_id`)
- **Merge 6:** Orders + Reviews (using `order_id`)
- **Merge 7:** Order Items + Sellers (using `seller_id`)

**Join Kind:** Left Outer join used throughout to ensure all records were kept.

**Reason:** Merging enables:
- Analysis across all business dimensions (product, customer, seller, time)
- Complete order lifecycle tracking
- Comprehensive business intelligence

**Challenge:** Some orders had no matching items, payments, or reviews (data quality issues). Left Outer join preserved all orders for analysis.

**Result:** A single fact table containing all necessary data for comprehensive analysis.

# Data Modelling
# DAX & Business Calculations
# Power BI Dashboards
