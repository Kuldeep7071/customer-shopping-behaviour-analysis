# Retail Customer Behavior Analysis

An end-to-end analysis of 3,900 retail shopping records: cleaning and feature engineering in Python, business questions answered in MySQL, and a Matplotlib report that turns the findings into recommendations for marketing and product strategy.

## Business Problem

A retail company wants to understand how customers shop so it can improve sales, satisfaction, and loyalty. Management has seen purchasing patterns shift across demographics, categories, and channels, and wants to know which factors (discounts, reviews, seasons, payment preferences) drive buying decisions and repeat purchases.

> How can the company leverage consumer shopping data to identify trends, improve customer engagement, and optimize marketing and product strategies?

The full brief is in `Business Problem  Document.pdf`.

## Dataset

`customer_shopping_behavior.csv` has **3,900 rows and 18 columns**. Each row is one customer purchase, and every `Customer ID` appears once.

| Column | Description |
|---|---|
| Customer ID, Age, Gender | Customer demographics |
| Item Purchased, Category, Size, Color | What was bought (25 items across 4 categories) |
| Purchase Amount (USD) | Value of the purchase |
| Location | US state |
| Season | Season of purchase |
| Review Rating | Rating from 1 to 5 (37 values missing) |
| Subscription Status | Whether the customer subscribes |
| Shipping Type | Standard, Express, 2-Day, Next Day Air, Free Shipping, Store Pickup |
| Discount Applied, Promo Code Used | Whether a discount or promo code was used |
| Previous Purchases | Count of earlier purchases |
| Payment Method | PayPal, Credit Card, Cash, Debit Card, Venmo, Bank Transfer |
| Frequency of Purchases | Weekly, Fortnightly, Monthly, Quarterly, and so on |

## Project Structure

```
Retail customer behavior/
├── Customer_Shopping_Behavior_Analysis.ipynb      # Cleaning, feature engineering, MySQL load, charts
├── customer_behaviour_analysis_sql_queries.sql    # 10 business questions in SQL
├── customer_shopping_behavior.csv                 # Raw dataset
├── Business Problem  Document .pdf                # Project brief
├── matplotlib_report_charts/                      # 14 exported report charts (PNG)
└── README.md
```

## Tech Stack

- **Python**: pandas, NumPy, Matplotlib
- **Database**: MySQL, accessed through PyMySQL and SQLAlchemy
- **Environment**: Jupyter Notebook

## Methodology

**1. Data preparation (Python)**
- Filled the 37 missing `Review Rating` values with the median rating of each product category.
- Renamed columns to snake_case, and shortened `Purchase Amount (USD)` to `purchase_amount`.
- Dropped `promo_code_used` because it matched `discount_applied` on every row.

**2. Feature engineering**
- `age_group`: four quartile-based groups: Young Adult (18–31), Adult (32–44), Middle-aged (45–57), Senior (58–70).
- `purchase_frequency_days`: converts purchase frequency text (for example, Weekly = 7, Quarterly = 90) into days.
- `customer_segment`: New (1 previous purchase), Returning (2–10), Loyal (more than 10).

**3. SQL analysis**

The cleaned data is loaded into a `customer` table in a `customer_behavior` database. The queries in `customer_behaviour_analysis_sql_queries.sql` answer:

1. Revenue from male vs. female customers
2. Customers who used a discount but still spent above the average purchase
3. Top 5 products by average review rating
4. Average purchase amount: Standard vs. Express shipping
5. Spend and revenue: subscribers vs. non-subscribers
6. Top 5 products by share of purchases with a discount
7. Customer counts for New, Returning, and Loyal segments
8. Top 3 most purchased products within each category
9. Whether repeat buyers (more than 5 previous purchases) are likely to subscribe
10. Revenue contribution by age group

**4. Visualization**

A Matplotlib report covers 14 charts: executive KPIs, revenue by gender, age group, and category, customer segments, top products, ratings, subscription and discount impact, payment methods, shipping, purchase frequency, and previous purchases vs. spend. All charts are saved to `matplotlib_report_charts/`.

## Key Findings

| Metric | Value |
|---|---|
| Total revenue | $233,081 |
| Transactions | 3,900 |
| Average purchase | $59.76 |
| Average review rating | 3.75 / 5 |
| Purchases with a discount | 43% |
| Subscribers | 1,053 (27%) |

- **Clothing leads revenue** at $104,264 (45%); with Accessories it reaches 77%. Blouse is the top product at $10,410.
- **Men generate 68% of revenue** ($157,890), but average spend is nearly identical: $59.54 for men and $60.25 for women.
- **Young Adults (18–31)** are the top age group at $62,143, though all four groups are within about $6.4K of each other.
- **Discounts do not raise spend.** Discounted purchases average $59.28 vs. $60.13 without a discount. Hat (50.0%) and Sneakers (49.7%) have the highest discount rates.
- **Subscribers do not spend more**: $59.49 vs. $59.87. Repeat buyers subscribe at 28%, about the same as the overall 27%.
- **No female customer in the dataset is a subscriber**, compared with 40% of male customers.
- **80% of records are Loyal** and only 2% are New (83 records).
- **Shipping and payment**: 2-Day shipping has the highest average purchase ($60.73) and Standard the lowest ($58.46). The six payment methods are split almost evenly. Fall is the strongest season ($60,018).

## Recommendations

1. **Tighten discounting.** Replace blanket offers with targeted ones and measure the effect.
2. **Rebuild the subscription offer.** Add perks that matter and test them with women, who currently have no subscribers.
3. **Invest in acquisition.** Only 2% of customers are new, so some retention budget could move to winning new buyers.
4. **Back Clothing and Accessories.** Bundle top sellers and run seasonal pushes in Fall.

## How to Run

1. Install the dependencies:
   ```bash
   pip install pandas numpy matplotlib pymysql sqlalchemy jupyter
   ```
2. Start MySQL and create the database:
   ```sql
   CREATE DATABASE customer_behavior;
   ```
3. Open `Customer_Shopping_Behavior_Analysis.ipynb` and replace `your_password` in the connection cells with your MySQL password. Update the host, user, and port if yours differ.
4. Run the notebook top to bottom. It cleans the data, loads the `customer` table, and regenerates the charts.
5. Run the queries in `customer_behaviour_analysis_sql_queries.sql` against the `customer_behavior` database.

The notebook expects the CSV in the same folder as the notebook.

## Notes and Limitations

- Each customer appears once, so "customers" and "transactions" are the same thing in this dataset. Repeat behavior is inferred from `previous_purchases` rather than from multiple rows per customer.
- Loyalty segments are defined by thresholds on `previous_purchases`, which makes most records Loyal. Different thresholds would change the segment sizes.
- The project brief mentions a Power BI dashboard. This project delivers the visual report in Matplotlib instead.
- The complete absence of female subscribers may reflect how the dataset was built rather than real customer behavior, so confirm it against the source data before acting on it.

## Author

Kuldeep
