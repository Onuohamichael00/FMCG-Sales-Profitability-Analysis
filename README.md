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





