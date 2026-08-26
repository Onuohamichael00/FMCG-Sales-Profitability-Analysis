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
