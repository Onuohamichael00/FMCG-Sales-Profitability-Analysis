# 📈 FMCG Sales & Profitability Analysis

## 📌 Project Overview

Fast-Moving Consumer Goods (FMCG) companies operate in highly competitive and fast-paced markets where profitability depends on efficient sales performance, inventory management, distribution efficiency, pricing, promotions, and accurate demand planning.

This project analyzes a multi-country FMCG transactional dataset covering **2021–2023**. The analysis transforms raw operational data into a structured Business Intelligence solution using **Microsoft Excel, Power Query, Power Pivot, and DAX**.

The project focuses on understanding:

- Sales and revenue performance
- Product and brand profitability
- Pricing and promotional effectiveness
- Inventory availability
- Stockout performance
- Supply chain efficiency
- Market and channel performance
- Year-over-year business trends

The final output is an interactive Excel-based analytical solution designed to support data-driven business decision-making.

---

# 🎯 Business Objectives

The project was designed to:

- Analyze sales and revenue performance across countries, cities, stores, and sales channels.
- Identify top-performing and underperforming products, brands, categories, and subcategories.
- Evaluate the impact of discounts and promotional campaigns on sales and profitability.
- Monitor inventory levels and identify products affected by stockouts.
- Assess operational performance using inventory and lead-time metrics.
- Examine sales trends across monthly, weekly, weekday, and weekend periods.
- Develop a scalable star-schema data model for analytical reporting.
- Build interactive dashboards and KPI reports for executive-level monitoring.
- Generate actionable insights and recommendations to improve profitability, inventory management, and operational efficiency.

---

# 🗃️ Dataset

The project uses the **FMCG Multi-Country Sales Dataset** sourced from Kaggle.

The dataset contains information relating to:

- Products and brands
- Sales performance
- Product categories and subcategories
- Countries and cities
- Stores and sales channels
- Discounts and promotions
- Inventory availability
- Supplier lead times
- Purchase costs
- Profitability metrics

The original CSV file is approximately **200 MB**, so it is not stored directly in this GitHub repository.

### 🔗 Original Dataset

[**View / Download the FMCG Multi-Country Sales Dataset on Kaggle**](https://www.kaggle.com/datasets/robertocarlost/fmcg-multi-country-sales-dataset/data)

---

# 🛠️ Tools & Technologies

- **Microsoft Excel**
- **Power Query**
- **Power Pivot**
- **DAX**
- **Pivot Tables**
- **Pivot Charts**
- **Data Modelling**
- **Star Schema**
- **Time Intelligence**

---

# 🧹 Data Exploration, Cleaning & Transformation

The original dataset was relatively clean and well-structured as a single flat file.

The transformation process therefore focused primarily on preparing the data for scalable analytical reporting and restructuring it into a star-schema model.

Key steps included:

- Exploring the dataset structure and granularity.
- Validating data types.
- Reviewing key fields and business metrics.
- Standardizing date and weekday calculations.
- Creating a dedicated calendar table.
- Preserving transactional records at their original grain.
- Preparing dimension tables with unique keys.
- Restructuring the flat dataset into a star-schema data model.
- Retaining transactional inventory fields such as stock on hand and stockout flags in the fact table.

---

# 🧩 Data Modelling

The original dataset was provided as a single flat transactional table. To make the dataset more suitable for analytical reporting, I transformed the flat structure into a **star-schema data model using Power Query and Power Pivot**.

This involved identifying the transactional fact table and **creating separate dimension tables** containing descriptive attributes used to analyze the transactions.

## Fact Table

### `FactSales`

`FactSales` serves as the central transactional table and contains measures and transactional attributes including:

- Date
- SKU ID
- Store ID
- Supplier ID
- Units Sold
- Gross Sales
- Net Sales
- Discount
- Promotion Flag
- Stock on Hand
- Stockout Flag
- Purchase Cost
- Lead Time
- Profitability metrics

## Dimension Tables Created

### `DimProduct`

I created a product dimension from the original transactional data to provide descriptive product attributes.

It contains:

- SKU ID
- SKU Name
- Category
- Subcategory
- Brand
- List Price

### `DimStore`

I created a store dimension to provide geographic and sales-channel attributes.

It contains:

- Store ID
- Country
- City
- Channel

### `DimDate`

I created a dedicated calendar/date dimension to support time-based analysis and DAX time intelligence.

It contains:

- Date
- Year
- Month Number
- Month Name
- Day of Month
- Day Name
- Quarter
- Weekday
- Week of Year
- IsWeekend

The calendar table covers:

**01/01/2021 – 31/12/2023**

## Why the Dimension Tables Were Created

Creating separate dimension tables allowed the model to:

- Reduce unnecessary duplication of descriptive information.
- Create clear relationships between business entities and transactions.
- Improve filtering and aggregation in Pivot Tables and Pivot Charts.
- Support scalable analysis across products, stores, countries, channels, and dates.
- Enable DAX time-intelligence calculations.
- Maintain a proper star-schema structure. 

## Relationship Structure

The dimension tables were connected to the central `FactSales` table through one-to-many relationships:

```text
DimDate[Date]       → FactSales[date]

DimProduct[sku_id]  → FactSales[sku_id]

DimStore[store_id]  → FactSales[store_id]
```
All relationships are one-to-many, with the dimension tables on the "one" side and FactSales on the "many" side.

The model uses a single-directional filter flow:

Dimension Tables → FactSales

No direct relationships were created between dimension tables in order to preserve the star-schema structure.

A separate supplier dimension was intentionally excluded because the available supplier information did not contain sufficient stable descriptive attributes.

---

# 📅 Time Intelligence

A dedicated `DimDate` calendar table was created to support time-based analysis and DAX time-intelligence calculations.

The calendar table covers the full analysis period:

**01/01/2021 – 31/12/2023**

The date dimension includes:

- Date
- Year
- Month Number
- Month Name
- Day of Month
- Day Name
- Quarter
- Weekday
- Week of Year
- IsWeekend

The date table was used to analyze business performance across different time periods and to calculate year-over-year performance.

### Weekday & Weekend Classification

The dataset's weekday numbering was standardized using:

```excel
=WEEKDAY(date,3)
````

This returns Monday as `0` through Sunday as `6`.

ISO week numbers were created using:

```excel
=ISOWEEKNUM(date)
```

A weekend classification was also created to distinguish Saturdays and Sundays from weekdays.

### DAX Time Intelligence

The `DimDate` table was used with DAX `DATEADD` and `CALCULATE` functions to create previous-year measures.

For example:

```DAX
PY Net Sales =
CALCULATE(
    [Total Net Sales],
    DATEADD(Dimdate[Date], -1, YEAR)
)
```

This allowed the dashboard to compare current-year performance against the previous year and calculate YoY changes for metrics such as:

* Net Sales
* Profit
* Profit Margin
* Promo Sales
* Promo Dependency
* Promo Units Sold
* Stockouts
* Stockout Rate
* Average Lead Time
* Average Stock on Hand
* Total Units Sold

# 📊 Key Performance Indicators

The analysis was structured around four key areas of business performance: sales and profitability, promotions and pricing, inventory and supply chain, and year-over-year performance.

## 💰 Sales & Profitability

- Total Net Sales
- Profit
- Profit Margin %
- Total Units Sold
- Average Order Value (AOV)
- Average Selling Price (ASP)
- Average Items Bought per Transaction
- Total Transactions

## 🏷️ Promotions & Pricing

- Promo Sales
- Promo Dependency %
- Promo Units Sold
- Discount Impact %
- Non-Promo Sales
- Year-over-Year Promotional Performance

## 📦 Inventory & Supply Chain

- Stock Out Count
- Stockout Rate %
- Average Stock on Hand
- Average Lead Time
- Year-over-Year Inventory Performance

## 📈 Year-over-Year Performance

Year-over-year measures were developed to evaluate changes in business performance across different years.

These comparisons were applied to:

- Net Sales
- Profit
- Profit Margin
- Units Sold
- Promo Sales
- Promo Units Sold
- Promo Dependency
- Discount Impact
- Stockout Count
- Stockout Rate
- Average Lead Time
- Average Stock on Hand

These KPIs formed the foundation of the four interactive dashboards and allowed performance to be analyzed across products, brands, categories, countries, stores, sales channels, and time periods.

# 🧮 DAX Measures

The analytical model was powered by DAX measures created in Power Pivot. These measures were used to calculate sales, profitability, promotional performance, inventory metrics, and year-over-year performance.

Below are selected DAX measures used in the project. **The complete set of DAX measures can be viewed in the completed Excel workbook.**

## 💰 Core Sales & Profitability Measures

```DAX
1. Total Net Sales =
SUM(fact_sales[net_sales])

2. Total Gross Sales
Total Gross Sales =
SUM(fact_sales[gross_sales])

3. Total Transactions
Total Transactions =
COUNTROWS(fact_sales)

4. Total Units Sold
Total Units Sold =
SUM(fact_sales[units_sold])

5. Total Cost
Total Cost =
SUMX(
    fact_sales,
    fact_sales[units_sold] * fact_sales[purchase_cost]
)

6. Total Discount
Total Discount =
[Total Gross Sales] - [Total Net Sales]

7. Profit
Profit =
[Total Net Sales] - [Total Cost]

8. Profit Margin %
Profit Margin % =
DIVIDE(
    [Profit],
    [Total Net Sales],
    0
)

9. Average Order Value (AOV)
AOV =
DIVIDE(
    [Total Net Sales],
    [Total Transactions],
    0
)

10. Average Items Bought per Transaction
Avg Items Bought per Transaction =
DIVIDE(
    [Total Units Sold],
    [Total Transactions],
    0
)

11. Average Selling Price (ASP)
Avg Selling Price =
DIVIDE(
    [Total Net Sales],
    [Total Units Sold],
    0
)

12. Average Lead Time
Avg Lead Time =
AVERAGE(fact_sales[lead_time_days])

13. Average Stock on Hand
Avg Stock on Hand =
AVERAGE(fact_sales[stock_on_hand])

14. Discount Impact %
Discount Impact % =
DIVIDE(
    [Total Discount],
    [Total Gross Sales],
    0
)

15. Net Sales %
Net Sales % =
DIVIDE(
    [Total Net Sales],
    [ALL Net Sales],
    0
)

16. Profit %
Profit % =
DIVIDE(
    [Profit],
    [ALL Profit],
    0
)
```

## 🏷️ Promotion Measures
```DAX
17. Promo Sales
Promo Sales =
CALCULATE(
    [Total Net Sales],
    FILTER(
        fact_sales,
        fact_sales[promo_flag] = 1
    )
)
18. Promo Units Sold
Promo Units Sold =
CALCULATE(
    [Total Units Sold],
    fact_sales[promo_flag] = 1
)
19. Promo Dependency %
Promo Dependency % =
DIVIDE(
    [Promo Sales],
    [Total Net Sales],
    0
)
20. Non-Promo Sales
Non Promo Sales =
[Total Net Sales] - [Promo Sales]
```

## 📦 Inventory & Supply Chain Measures
```DAX
21. Stock Out Count
Stock Out Count =
COUNTROWS(
    FILTER(
        fact_sales,
        fact_sales[stock_out_flag] = 1
    )
)

22. Stockout Rate %
Stockout Rate % =
DIVIDE(
    [Stock Out Count],
    [Total Transactions],
    0
)
```

## 📅 Previous-Year Measures

Previous-year measures were created using CALCULATE and DATEADD to shift the date context by one year.
```DAX
23. Previous-Year Net Sales
PY Net Sales =
CALCULATE(
    [Total Net Sales],
    DATEADD(Dimdate[Date], -1, YEAR)
)

24. Previous-Year Profit
PY Profit =
CALCULATE(
    [Profit],
    DATEADD(Dimdate[Date], -1, YEAR)
)

25. Previous-Year Profit Margin
PY Profit Margin =
CALCULATE(
    [Profit Margin %],
    DATEADD(Dimdate[Date], -1, YEAR)
)

26. Previous-Year Promo Sales
PY Promo Sales =
CALCULATE(
    [Promo Sales],
    DATEADD(Dimdate[Date], -1, YEAR)
)

27. Previous-Year Promo Dependency %
PY Promo Dependency % =
CALCULATE(
    [Promo Dependency %],
    DATEADD(Dimdate[Date], -1, YEAR)
)

28. Previous-Year Promo Units Sold
PY Promo Units Sold =
CALCULATE(
    [Promo Units Sold],
    DATEADD(Dimdate[Date], -1, YEAR)
)

29. Previous-Year Stock Out Count
PY Stock Out Count =
CALCULATE(
    [Stock Out Count],
    DATEADD(Dimdate[Date], -1, YEAR)
)

30. Previous-Year Stockout Rate
PY Stockout Rate =
CALCULATE(
    [Stockout Rate %],
    DATEADD(Dimdate[Date], -1, YEAR)
)

31. Previous-Year Average Lead Time
PY Avg Lead Time =
CALCULATE(
    [Avg Lead Time],
    DATEADD(Dimdate[Date], -1, YEAR)
)

32. Previous-Year Average Stock on Hand
PY Avg Stock on Hand =
CALCULATE(
    [Avg Stock on Hand],
    DATEADD(Dimdate[Date], -1, YEAR)
)

33. Previous-Year Discount Impact
PY Discount Impact =
CALCULATE(
    [Discount Impact %],
    DATEADD(Dimdate[Date], -1, YEAR)
)

34. Previous-Year Total Units Sold
PY Total Units Sold =
CALCULATE(
    [Total Units Sold],
    DATEADD(Dimdate[Date], -1, YEAR)
)
```

## 📈 Year-over-Year Measures
```DAX
35. YoY Net Sales Growth
YOY Net Sales Growth =
DIVIDE(
    [Total Net Sales] - [PY Net Sales],
    [PY Net Sales],
    0
)

36. YoY Profit Growth
YOY Profit Growth =
DIVIDE(
    [Profit] - [PY Profit],
    [PY Profit],
    0
)

37. YoY Average Lead Time
YOY Avg Lead Time =
DIVIDE(
    [Avg Lead Time] - [PY Avg Lead Time],
    [PY Avg Lead Time],
    0
)

38. YoY Average Stock on Hand
YOY Avg Stock on Hand =
DIVIDE(
    [Avg Stock on Hand] - [PY Avg Stock on Hand],
    [PY Avg Stock on Hand],
    0
)

39. YoY Promo Sales
YOY Promo Sales =
DIVIDE(
    [Promo Sales] - [PY Promo Sales],
    [PY Promo Sales],
    0
)

40. YoY Promo Units Sold
YOY Promo Units Sold =
DIVIDE(
    [Promo Units Sold] - [PY Promo Units Sold],
    [PY Promo Units Sold],
    0
)

41. YoY Promo Dependency
YOY Promo Dependency =
[Promo Dependency %] - [PY Promo Dependency %]

42. YoY Profit Margin Difference
YOY Profit Margin Difference =
[Profit Margin %] - [PY Profit Margin]

43. YoY Discount Impact
YOY Discount Impact =
[Discount Impact %] - [PY Discount Impact]
```

# 📊 Interactive Dashboards

The final analysis was presented through four interactive dashboard pages, each focusing on a different area of FMCG business performance.

## 1️⃣ Executive Overview & Product Performance

This dashboard provides a high-level view of overall business performance and product-level performance across the 2021–2023 period.

### Key KPIs

- Total Net Sales
- Profit
- Profit Margin %
- Total Units Sold
- Year-over-Year Performance

### Key Analysis

- Monthly Net Sales and Profit trends
- Sales performance by Product Category
- Sales performance by Brand
- Product-level performance
- Revenue contribution
- Profitability performance
- Year-over-Year changes

### Dashboard Preview

[![Executive Overview & Product Performance Dashboard](Dashboard%20visuals/Overview_Dashboard%201.JPG)](Dashboard%20visuals/Overview_Dashboard%201.JPG)

### 🔗 View Dashboard

[**Click here to view the Executive Overview & Product Performance Dashboard**](Dashboard%20visuals/Overview_Dashboard%201.JPG)


## 2️⃣ Executive Overview & Market Performance

This dashboard provides a deeper view of business performance across markets, stores, sales channels, and products for the 2021–2023 period.

### Key KPIs

- Total Net Sales
- Profit
- Profit Margin %
- Total Units Sold
- Year-over-Year Performance

### Key Analysis

- Monthly demand trends
- Top-performing stores by Net Sales
- Sales performance by Country
- Sales performance by Sales Channel
- Most profitable products
- Market performance
- Customer demand patterns

### Dashboard Preview

[![Executive Overview & Market Performance Dashboard](Dashboard%20visuals/Overview_Dashboard%202.JPG)](Dashboard%20visuals/Overview_Dashboard%202.JPG)

### 🔗 View Dashboard

[**Click here to view the Executive Overview & Market Performance Dashboard**](Dashboard%20visuals/Overview_Dashboard%202.JPG)


## 3️⃣ Pricing & Promotions Analysis

This dashboard evaluates the effectiveness of pricing strategies and promotional activities across the FMCG business.

### Key KPIs

- Promo Sales
- Promo Dependency %
- Promo Units Sold
- Discount Impact %
- Year-over-Year Performance

### Key Analysis

- Promotional sales performance
- Promotional dependency by Product Category
- Promotional dependency by Country
- Promotional dependency by Brand
- Discount impact on sales
- Promotional unit performance
- Pricing and promotional efficiency
- Year-over-Year changes

### Dashboard Preview

[![Pricing & Promotions Dashboard](Dashboard%20visuals/Promotions_Dashboard.JPG)](Dashboard%20visuals/Promotions_Dashboard.JPG)

### 🔗 View Dashboard

[**Click here to view the Pricing & Promotions Dashboard**](Dashboard%20visuals/Promotions_Dashboard.JPG)


## 4️⃣ Inventory Availability & Supply Chain Performance

This dashboard focuses on inventory availability and supply chain efficiency across the FMCG business.

### Key KPIs

- Stock Out Count
- Stockout Rate %
- Average Stock on Hand
- Average Lead Time
- Year-over-Year Performance

### Key Analysis

- Stockout patterns by Product Category
- Stockout patterns by Product
- Stockout performance by Store
- Stockout performance by Country
- Supplier performance
- Average Lead Time
- Average Stock on Hand
- Product availability
- Supply chain performance
- Year-over-Year changes

### Dashboard Preview

[![Inventory Availability & Supply Chain Dashboard](./Dashboard%20visuals/Supply%20chain_Dashboard.JPG)](https://github.com/Onuohamichael00/FMCG-Sales-Profitability-Analysis/blob/main/Dashboard%20visuals/Supply%20chain_Dashboard.JPG)

### 🔗 View Dashboard

[**Click here to view the Inventory Availability & Supply Chain Dashboard**](https://github.com/Onuohamichael00/FMCG-Sales-Profitability-Analysis/blob/main/Dashboard%20visuals/Supply%20chain_Dashboard.JPG)


# 🔎 Key Business Insights

## 💰 Overall Business Performance

Across the 2021–2023 period, the business generated:

| KPI | Result |
|---|---:|
| Net Sales | **$472.95M** |
| Profit | **$182.21M** |
| Profit Margin | **38.53%** |
| Units Sold | **65.12M** |

Overall performance remained relatively stable across the three years, with **2022 recording the strongest overall performance** and **2023 showing a slight decline**.

### Year-over-Year Performance

**2022 vs 2021**

- Net Sales: **+0.16%**
- Profit: **+0.21%**
- Profit Margin: **+0.02 percentage points**
- Units Sold: **-0.02%**

**2023 vs 2022**

- Net Sales: **-0.16%**
- Profit: **-0.26%**
- Profit Margin: **-0.04 percentage points**
- Units Sold: **-0.10%**

The relatively small changes in units sold compared with changes in revenue and profit suggest that **pricing, basket size, and customer spending behavior played an important role in overall performance**.

## 📈 2022 Growth Drivers & 2023 Decline Drivers

### 🚀 What Drove Performance in 2022?

Although total units sold decreased slightly in 2022, the business achieved higher net sales and profit compared with 2021.

This indicates that growth was driven more by **improved value per transaction and operational efficiency** rather than increased sales volume.

Key drivers included:

- **Higher Average Selling Price (ASP)** — customers generated more revenue per unit sold.
- **Higher Average Order Value (AOV)** — customers spent more per transaction.
- **Higher Average Items Bought per Transaction** — customers purchased more items within each transaction.
- **Improved inventory availability** — lower stockout levels helped reduce missed sales opportunities.
- **Lower reliance on promotions** — performance was less dependent on promotional sales.
- **Improved profitability** — profit increased slightly faster than net sales, resulting in a small improvement in profit margin.

Overall, **2022 demonstrated that the business could improve financial performance without relying on higher sales volume**.

### 📉 What Drove the Decline in 2023?

In 2023, both net sales and profit declined compared with 2022.

The decline was associated with weaker customer spending and reduced revenue efficiency.

Key factors included:

- **Lower Average Selling Price (ASP)**
- **Lower Average Order Value (AOV)**
- **Fewer items purchased per transaction**
- **Lower units sold**
- **Reduced promotional effectiveness**
- **Slight deterioration in profit margin**

While the decline in total units sold was relatively small, the reduction in ASP, AOV, and basket size had a greater impact on overall revenue and profitability.

### 💡 Key Takeaway

The comparison between 2022 and 2023 highlights an important business lesson:

> **Revenue growth does not necessarily require higher sales volume. Improving customer spending per transaction, pricing, product mix, and operational efficiency can have a significant impact on profitability.**

The business should therefore focus on **value-driven growth** by increasing AOV and basket size, maintaining effective pricing, improving product performance, and reducing unnecessary dependence on promotions.

## 🏷️ Brand Performance

Brand performance varied significantly across revenue, profitability, sales volume, and promotional dependency.

### 🥇 Top-Performing Brand — BrandD

**BrandD** was the strongest-performing brand across several key metrics.

| KPI | BrandD |
|---|---:|
| Net Sales | **$93.24M** |
| Revenue Contribution | **19.71%** |
| Profit | **$36.81M** |
| Profit Margin | **39.48%** |
| Units Sold | **13.02M** |
| Promo Dependency | **3.86%** |

BrandD combined **high revenue, strong profitability, high sales volume, and low promotional dependency**, making it one of the most efficient brands in the portfolio.

### 🥈 BrandB

BrandB was another strong performer, demonstrating a similar pattern of **strong financial performance and relatively low dependence on promotions**.

Its performance suggests that brands can achieve strong revenue and profitability without relying heavily on promotional activity.

### 🏷️ BrandC — High Promotional Performance but Lower Margin

BrandC generated the **highest promotional sales at $12.15M**.

However, its profit margin was comparatively lower at **37.80%**.

This indicates that while promotions contributed significantly to BrandC's sales, the additional promotional activity did not translate into the strongest profitability.

### ⚠️ BrandE — Underperforming Brand

BrandE recorded the weakest overall performance in terms of revenue and profit contribution.

With **10.92M units sold**, the brand generated substantial sales volume but did not translate this volume into equally strong financial performance.

This suggests an opportunity to investigate:

- Pricing strategy
- Product mix
- Profit margins
- Promotional dependency
- Customer demand
- Product-level performance

### 💡 Key Takeaway

The brand analysis shows that **high sales volume alone does not guarantee strong profitability**.

BrandD's combination of high revenue, strong margins, and low promotional dependency demonstrates the value of **efficient pricing and sustainable demand**, while BrandC highlights the need to evaluate whether promotional sales are generating sufficient profitability.

Underperforming brands such as BrandE should be reviewed at the product level to identify opportunities for **pricing optimization, product rationalization, and improved profitability**.

## 📊 Category Performance

Category-level analysis revealed clear differences in revenue contribution, profitability, sales volume, and promotional dependency.

### 🥇 Top Revenue Category — Snacks

**Snacks** was the highest-performing category by revenue.

| KPI | Snacks |
|---|---:|
| Net Sales | **$130.40M** |
| Revenue Contribution | **27.52%** |
| Profit | **$49.46M** |
| Profit Margin | **38.01%** |
| Units Sold | **13.90M** |
| Promo Dependency | **13.90%** |

Snacks generated the largest share of overall revenue and profit, making it a major contributor to the business's financial performance.

However, its relatively high promotional dependency indicates that a meaningful portion of its sales was generated through promotional activity.

### 💰 Dairy — Strong Margin Performance

The **Dairy** category recorded the highest profit margin at **39.50%**, while maintaining a relatively low promotional dependency of **4.28%**.

This suggests that Dairy was able to generate strong profitability without relying heavily on promotions.

### 🏷️ Promotional Dependency

Promotional reliance varied across categories.

- **Snacks:** 13.90% promo dependency
- **Beverages:** 12.82% promo dependency
- **Dairy:** 4.28% promo dependency

The difference suggests that some categories may have stronger underlying demand and pricing power, while others require greater promotional support to generate sales.

### 💡 Key Takeaway

The category analysis highlights the importance of balancing **revenue growth with profitability and promotional efficiency**.

Snacks should remain a key focus because of its strong revenue and profit contribution, while its promotional dependency should be monitored to ensure future growth does not become overly reliant on discounts.

Categories such as Dairy demonstrate the potential of achieving **strong margins with lower promotional dependence**, providing a useful benchmark for improving category-level profitability.
