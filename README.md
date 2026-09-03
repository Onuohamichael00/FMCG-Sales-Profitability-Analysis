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

## 🌍 Geographic Performance

The geographic analysis revealed significant differences in sales performance, profitability, promotional reliance, and inventory availability across countries.

🥇 Italy — Strongest Overall Market

Italy was the top-performing country in both net sales and profit.

This indicates that Italy was the strongest market in terms of both revenue generation and profitability, making it an important market for maintaining and potentially expanding business activity.

However, Italy also recorded the highest stockout counts, despite its strong sales performance. This suggests that demand in the market may be outpacing inventory availability.

📦 Netherlands — Low Sales Despite Strong Availability

The Netherlands showed the opposite pattern.
| Metric                |          Netherlands |
| --------------------- | -------------------: |
| Net Sales             | **$12.52M — Lowest** |
| Stockouts             |           **Lowest** |
| Average Stock on Hand |      **2nd highest** |
| Promo Dependency      | **12.39% — Highest** |

Despite having strong inventory availability and relatively high stock levels, the Netherlands generated the lowest net sales.

This suggests that the market's weak performance is unlikely to be primarily caused by product availability.

The high promotional dependency also indicates that the market may require significant promotional support to generate sales.

💰 Austria — Strong Profitability

Austria recorded the highest profit margin at 38.64%, despite having only moderate sales performance.

This indicates relatively strong profitability efficiency compared with the size of its revenue contribution.

Austria therefore provides an example of a market where margin quality is strong even without being the largest revenue contributor.

📊 Mid-Tier Markets

Germany, Spain, France, and Poland demonstrated relatively stable mid-tier performance.

These markets did not stand out as either the strongest or weakest performers, suggesting that they may represent opportunities for incremental growth through improvements in pricing, promotions, distribution, or demand generation.

💡 Key Takeaway

The geographic analysis reveals an important operational distinction:

Strong inventory availability does not necessarily translate into strong sales performance.

The Netherlands provides the clearest example: it had the lowest sales, despite having the lowest stockout counts and second-highest average stock on hand. Meanwhile, Italy had strong sales and profit but also experienced the highest stockout counts.

This suggests that inventory should be allocated according to demand rather than distributed uniformly across markets.

🎯 Business Implication

The business should:

1. Prioritize Italy for stronger inventory coverage because of its high demand.
2. Investigate the Netherlands' weak demand, focusing on pricing, product-market fit, and customer adoption rather than simply increasing inventory.
3. Maintain focus on Austria's margin efficiency and identify what pricing or product factors are supporting its profitability.
4. Evaluate the growth potential of stable mid-tier markets such as Germany, Spain, France, and Poland.

## 🛒 Sales Channel Performance

Sales performance varied considerably across the different sales channels, with Hypermarkets contributing the largest share of revenue.

### 🥇 Hypermarkets — Largest Revenue Contributor

**Hypermarkets** generated the highest net sales at **$258.74M**, making them the dominant sales channel in the business.

This indicates that Hypermarkets were the primary channel for generating sales volume and revenue across the analysis period.

### 🛍️ E-commerce — Significant Revenue Contribution

**E-commerce** generated **$112.73M** in net sales, making it the second-largest contributor among the sales channels.

Its strong contribution highlights the importance of digital channels within the overall sales strategy.

### 💰 Supermarkets — Strongest Margin

Although Supermarkets did not generate the highest revenue, they recorded the **highest profit margin at 38.56%**.

This demonstrates that revenue contribution and profitability are not necessarily the same across channels.

### 🏪 Convenience Stores

**Convenience Stores** represented the smallest contribution to overall sales.

This suggests an opportunity to assess whether the channel has potential for growth through improved product assortment, pricing, availability, or targeted promotions.

### 💡 Key Takeaway

The channel analysis shows that the business should evaluate sales channels using both **revenue contribution and profitability**, rather than focusing on sales alone.

Hypermarkets remain the primary revenue driver, while Supermarkets demonstrate stronger margin efficiency. E-commerce also represents a significant source of revenue and should remain an important part of the business's growth strategy.

## 🏷️ Promotions & Pricing Performance

Promotional activity contributed to sales, but the analysis showed that promotions were **not the primary driver of overall revenue growth**.

### 📊 Overall Promotional Performance

Across 2021–2023:

| KPI | Result |
|---|---:|
| Promo Sales | **$49.40M** |
| Promo Dependency | **10.44%** |
| Discount Impact | **~2.4%** |

Promo dependency remained relatively stable across the three years:

- **2021:** 10.47%
- **2022:** 10.46%
- **2023:** 10.41%

Discount impact also remained stable:

- **2021:** 2.41%
- **2022:** 2.45%
- **2023:** 2.44%

This indicates that the business did not become significantly more dependent on promotions over time. 

### 🏷️ Category-Level Promotional Dependency

**Snacks** recorded the highest promotional dependency at **13.90%**, followed by **Beverages at 12.82%**.

Snacks also recorded the highest discount impact at **3.20%**.

This suggests that some of the business's highest-revenue categories rely more heavily on promotional activity to generate sales.

### 💡 Key Takeaway

Promotions are useful for supporting sales, but the relatively stable **10.44% promotional dependency** shows that they are not the main source of overall revenue.

The analysis therefore suggests that the business should focus on **organic demand, pricing efficiency, and customer spending behavior**, while using promotions more strategically rather than relying on continuous discounting.


## 📦 Inventory & Supply Chain Performance

The supply chain analysis focused on stock availability, stockout patterns, inventory levels, and lead times across products, stores, and countries.

### 📉 Stockout Performance

Across 2021–2023, the business recorded:

| KPI | Result |
|---|---:|
| Total Stockouts | **33.11K** |
| Stockout Rate | **3.01%** |
| Average Stock on Hand | **~299** |

Stockouts gradually declined over the three-year period:

- **2021:** 11.10K stockouts
- **2022:** 11.05K stockouts (**-0.53%**)
- **2023:** 10.97K stockouts (**-0.72%**)

This indicates a gradual improvement in product availability. :contentReference[oaicite:0]{index=0}

### 🏪 Country-Level Availability

There were notable differences between inventory availability and sales performance across countries.

- **Italy** recorded the highest stockout counts despite being the strongest market by sales and profit.
- **Netherlands** recorded the lowest stockout counts and maintained high inventory availability, but generated the lowest sales.
- Average stock on hand remained relatively stable at approximately **299** across the three years.

This demonstrates that **strong inventory availability does not necessarily translate into strong sales performance**. :contentReference[oaicite:1]{index=1}

### 📦 Category-Level Stockouts

**Snacks and Beverages** recorded the highest stockout rates.

Because these categories are also major revenue contributors, stockouts in these areas could create missed sales opportunities and should receive greater attention in inventory planning. :contentReference[oaicite:2]{index=2}

### 💡 Key Takeaway

The supply chain was generally stable, with stockouts gradually declining over time. However, inventory availability was not always aligned with customer demand.

The contrast between **Italy's strong demand and higher stockouts** and the **Netherlands' high inventory availability but weak sales** highlights the need for a more **demand-driven inventory allocation strategy**.

Rather than simply increasing inventory across all markets, the business should prioritize inventory based on **demand patterns, sales potential, and stockout risk**.


## 📈 Demand Patterns

Demand patterns varied across years, products, categories, countries, and sales channels.

### 🛒 Customer Purchasing Behavior

The analysis showed that changes in revenue were not always driven by changes in the number of units sold.

In 2022, the business achieved higher net sales despite a slight decline in total units sold. This suggests that customers were generating more value per transaction through:

- Higher Average Selling Price (ASP)
- Higher Average Order Value (AOV)
- Higher average items purchased per transaction

In 2023, these indicators declined, contributing to the reduction in overall net sales.

### 📦 Product & Category Demand

Demand was concentrated across the strongest-performing product categories, with **Snacks** contributing the largest share of revenue.

The analysis also showed that sales volume and profitability varied considerably between categories. High-volume categories therefore require monitoring not only for demand but also for their ability to convert demand into profitable sales.

### 🌍 Geographic Demand

Demand also varied across markets.

**Italy** was the strongest market by sales and profit, indicating strong customer demand. However, its higher stockout levels suggest that inventory availability may not always have kept pace with demand.

In contrast, the **Netherlands** maintained strong inventory availability but recorded the lowest sales. This suggests that its weaker performance was more likely related to demand, pricing, product mix, or promotional effectiveness rather than inventory shortages.

### 🛍️ Channel Demand

Demand differed significantly across sales channels.

- **Hypermarkets** generated the highest revenue, indicating strong customer demand through this channel.
- **E-commerce** represented a significant portion of total sales and highlights the importance of digital purchasing behavior.
- **Supermarkets** generated lower revenue than Hypermarkets but achieved the strongest profit margin.
- **Convenience Stores** recorded the smallest sales contribution.

### 💡 Key Takeaway

Demand analysis shows that the business should not evaluate demand using sales volume alone.

A more complete view should consider **revenue per transaction, basket size, pricing, product mix, geographic demand, channel performance, and inventory availability**.

Understanding these demand patterns can help the business improve **inventory allocation, pricing decisions, product assortment, and sales-channel strategy** while reducing missed sales opportunities caused by stockouts.

# Recommendations

### Strengthen pricing strategy in high-revenue but low-efficiency categories

Categories like Snacks and Beverages generate the highest revenue but show lower profit margins and higher promo dependency. This suggests growth is being supported by discounts rather than pricing strength.
The business should:
1. Gradually reduce excessive discounting in high-demand categories 
2. Introduce targeted pricing instead of blanket promotions 
3. Focus on premium variants or value-based pricing where ASP can be improved 
This will help improve margin quality without sacrificing demand.

### Reduce over-reliance on promotions and improve organic demand

Promo dependency is relatively stable (~10.4%), but certain categories and channels (Snacks, Beverages, Convenience stores) are significantly more promotion-driven.
The business should:
1. Shift from constant promotions to seasonal or strategic campaigns 
2. Build brand loyalty through product differentiation 
3. Encourage repeat purchase behavior without discounts 
This will reduce revenue volatility and improve profitability stability.

### Improve demand recovery in underperforming categories and brands

Brands such as BrandE and categories like Home Care consistently underperform in revenue contribution.
Recommended actions:
1. Reassess product positioning and pricing strategy 
2. Identify gaps in product assortment or market fit 
3. Increase marketing focus on low-performing segments with growth potential 
The goal is not just discounting these categories but improving their market relevance.

### Optimize supply chain allocation rather than just reducing stockouts

Stockouts have slightly decreased over time, but performance differences across countries show that availability is not always aligned with demand.
For example:
1. Netherlands has low stockouts and high stock levels but the lowest sales 
2. Italy has high demand but also higher stockout counts 
The business should:
1. Improve demand forecasting at country and store level 
2. Reallocate inventory based on actual demand patterns rather than uniform distribution 
3. Prioritize high-performing markets like Italy for stronger supply coverage 

### Address inefficiencies in low-performing markets (especially Netherlands)

The Netherlands shows a unique pattern:
1. Lowest net sales 
2. Very low stockouts 
3. High average stock on hand 
4. High promo dependency 
This indicates demand issues rather than supply issues.
Recommended actions:
1. Conduct market-level demand analysis to understand weak adoption 
2. Review product-market fit and pricing strategy in that region 
3. Reduce excess stock allocation and redirect inventory to stronger markets 

### Improve conversion efficiency (AOV and basket size growth)

The 2022 performance peak was driven by higher AOV, ASP, and items per transaction, not volume growth. In 2023, all of these declined, leading to weaker revenue.
The business should:
1. Encourage bundle offers that increase basket size without heavy discounting 
2. Optimize product combinations to increase items per transaction 
3. Focus on upselling and cross-selling strategies 
This directly targets the main driver of revenue performance.

### Strengthen high-performing channels while improving weaker ones

Hypermarkets dominate revenue, while convenience stores contribute very little but show higher promotional reliance.
Actions:
1. Invest more in hypermarket partnerships and shelf visibility 
2. Improve assortment strategy in e-commerce for higher margin products 
3. Re-evaluate convenience channel strategy due to low efficiency 

### Improve inventory planning using demand signals

Stockouts are relatively stable, but they still affect key categories like Snacks and Beverages.
To improve:
1. Use historical demand trends (monthly spikes in May, August, December) for better forecasting 
2. Increase safety stock for high-demand periods 
3. Align inventory levels with category-level demand variability 

### Focus on margin quality, not just revenue growth

Even though revenue remains stable, profit margin differences across categories and brands show that not all growth is efficient.
The business should:
1. Prioritize categories with stable high margins like Dairy 
2. Reduce dependency on low-margin promotional sales 
3. Track margin-adjusted growth instead of revenue-only growth 

### Final Strategic Summary

The overall business story shows that:
1. Growth is driven more by customer spending behavior than volume 
2. Promotions support sales but do not create sustainable growth 
3. Supply chain performance is generally stable, but not perfectly aligned with demand 
4. Certain markets and categories require structural improvement rather than operational fixes 
The most important shift required is:
Moving from promotion-driven and volume-focused growth to value-driven and efficiency-focused growth.






