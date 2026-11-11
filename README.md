# ecommerce-sales-profitability-analysis
E-commerce sales, profitability, and product category analysis using Python, Pandas, and Power BI.
# E-Commerce Sales & Profitability Analysis

**Python | Pandas | Data Cleaning | Data Validation | Power BI | Business Analytics**

## Project Overview

This project explores how data analysis can support business decisions in an e-commerce environment.

The analysis focuses on product category profitability, sales performance, and potential opportunities for assortment expansion in the U.S. market between 2021 and 2025.

The project follows an end-to-end analytical workflow, from understanding and validating raw data to developing business recommendations and presenting findings in Power BI.

**A major focus of this project was data quality.** During exploration, I identified inconsistencies between transaction-level and item-level financial records. Investigating and resolving these discrepancies became an essential part of preparing a reliable analytical dataset.

### Project Objectives

- Understand the structure and relationships within the dataset.
- Investigate missing values and financial inconsistencies.
- Reconcile order-level and item-level financial records.
- Evaluate product category sales and profitability.
- Identify categories with positive sales and unit growth.
- Develop data-driven recommendations for potential assortment expansion.

---

## Business Problem

A hypothetical U.S.-based e-commerce retailer operates across 15 product categories and wants to identify opportunities to improve profitability.

Management is considering expanding its product assortment but needs to understand which categories contribute most to current financial performance and which may offer potential for profitable growth.

### Main Business Question

**Which product categories should the company prioritize for profitable assortment expansion?**

To address this question, I investigated:

1. Which categories generate the highest gross sales and total profit?
2. Which categories have the highest profit margins?
3. How do product costs, markups, and discounts differ across categories?
4. Which categories experienced sales and unit growth between 2021 and 2025?
5. Which categories combine positive growth with strong profitability?

The objective was not simply to rank categories but to evaluate multiple performance indicators before making business recommendations.

---

## Data Source & Authenticity Assessment

**Source:** Kaggle — E-Commerce Sales & Customer Analytics dataset

The dataset included transaction, order-item, product catalog, and customer-related information.

However, the original source of the transactions and the data collection methodology were not clearly documented.

During the analysis, I identified several characteristics suggesting that the dataset was likely synthetic or artificially generated.

These included:

- Unusually consistent transaction patterns across multiple years.
- Financial inconsistencies between order-level and item-level records.
- Simplified tax calculations and financial relationships.
- Patterns that appeared more consistent with generated data than naturally occurring commercial transactions.

Although these observations raised concerns about the dataset's authenticity, its original generation process could not be independently verified.

**Data Transparency Note:** This project uses a likely synthetic dataset. The purpose is to demonstrate data validation, analytical reasoning, business analysis, and reporting skills rather than make claims about actual U.S. e-commerce market conditions.

---

## Tools & Technologies

| Tool | Purpose |
|---|---|
| Python | Data analysis and processing |
| Pandas | Data exploration, cleaning, joins, aggregation, and validation |
| Jupyter Notebook | Reproducible analysis and documentation |
| Power BI | Interactive dashboards and visual reporting |
| DAX | Financial measures and calculated performance indicators |
| CSV / Excel | Source data and data inspection |

---

## Data Understanding, Cleaning & Validation

### 1. Understanding the Dataset

I began by examining the available files and identifying how the different datasets could be connected.

The main transaction dataset contained **138,116 records and 46 columns**.

Additional datasets provided order-item details, product information, and customer-related attributes.

Initial exploration included:

- Reviewing column names and data types.
- Checking dataset dimensions and unique identifiers.
- Investigating missing values and duplicate records.
- Examining order statuses and return-related fields.
- Understanding date coverage and transaction distribution.
- Identifying relationships between orders, products, and customers.

Instead of immediately removing missing values, I investigated whether missingness could be explained by the underlying transaction process.

For example, delivery-related information was primarily unavailable for orders that had not been completed, while return-related fields were generally populated only for returned orders.

This distinction was important because not every missing value represented a data quality error.

### 2. Investigating Financial Calculations

I examined the relationships between financial fields, including:

- Gross Sales
- Discount Amount
- Net Sales
- Product Cost
- Shipping Cost
- Tax Amount
- Profit

I tested whether the recorded financial values were consistent with the calculations implied by the dataset.

During this process, I identified that the dataset's profit calculations did not necessarily represent conventional accounting profit.

In particular, the recorded financial relationships included tax amounts in a way that could increase calculated profit.

I therefore treated the dataset's `profit` field as **recorded profit**, rather than assuming it represented a company's actual net profit.

I also investigated negative-profit transactions and evaluated how discount rates were associated with order profitability.

The analysis showed that higher discount rates were associated with an increased frequency of negative-profit orders.

### 3. Identifying Inconsistencies Between Orders and Order Items

One of the most significant findings emerged when I compared financial totals in the main order dataset against aggregated values from the order-items dataset.

I grouped individual order-item records by `order_id` and compared their total gross sales and profit with the corresponding values in the main orders table.

**Validation results:**

| Metric | Result |
|---|---:|
| Total orders | 138,116 |
| Orders with financial discrepancies | 11,226 |
| Percentage of affected orders | 8.13% |
| Additional order-item records identified | 31,591 |

These discrepancies required further investigation before the item-level data could be used for product category profitability analysis.

### 4. Investigating the Source of the Discrepancies

Rather than immediately deleting inconsistent records, I examined how the discrepancies occurred.

The investigation included:

- Comparing individual orders with their corresponding item-level records.
- Examining the number of product lines per order.
- Calculating cumulative gross sales and cumulative profit within each order.
- Comparing cumulative item-level totals with recorded order-level financial totals.
- Testing whether recorded order totals matched a subset of their associated order items.

For all **11,226 affected orders**, I found a point in the original item sequence where cumulative gross sales and cumulative profit simultaneously matched the recorded order totals.

This pattern suggested that additional item records existed beyond the item sequence represented by the order-level financial totals.

The investigation identified **31,591 additional order-item records** that were not reflected in the corresponding order-level financial totals.

This finding provided a basis for constructing a reconciled item-level dataset.

### 5. Reconciling the Data

To prepare the dataset for category-level analysis, I retained the order-item records consistent with the recorded order-level financial totals.

The original source files were preserved, while the reconciled dataset was created separately.

**Reconciliation results:**

| Metric | Records |
|---|---:|
| Original order-item records | 397,569 |
| Reconciled order-item records | 365,978 |
| Excluded additional item records | 31,591 |

After reconciliation, I repeated the financial validation.

**Final validation results:**

- Gross Sales mismatches: **0**
- Profit mismatches: **0**
- Orders with missing item totals: **0**

This confirmed consistency between the reconciled item-level records and the recorded order-level financial totals, within the validation tolerance used.

**Important limitation:** Reconciliation established mathematical consistency with the order-level dataset. Without access to the original transaction system, it was not possible to independently determine which source records represented authentic business transactions.

### 6. Preparing the Analytical Dataset

After validation and reconciliation, I merged order-item records with product catalog information and added order dates and category attributes.

The final business analysis was restricted to records labeled as **USA** and denominated in **USD**.

This scope avoided directly aggregating monetary values recorded in different currencies.

The prepared U.S. dataset contained **218,797 product line items** across **15 product categories**.

I then used Pandas to calculate the financial and growth metrics required for business analysis and Power BI reporting.

---

## Exploratory Data Analysis & Business Investigation

After preparing the data, I investigated category-level business performance.

### Product Category Profitability

I calculated:

- Total Gross Sales
- Total Recorded Profit
- Profit Margin
- Product Cost Ratio
- Product Markup
- Discount Rate

An important finding was that categories generating the most total profit did not necessarily have the highest profit margins.

For example, Electronics generated the highest total profit, while Grocery had the highest recorded profit margin.

Further investigation showed that Electronics had a substantially higher product cost ratio and lower relative markup than Grocery.

Discount rates, however, were relatively similar across categories.

This suggested that differences in product price-to-cost relationships were more closely associated with variations in recorded profit margins than differences in aggregate discount rates.

### Sales and Unit Growth Analysis

To evaluate potential assortment expansion opportunities, I compared category-level performance between 2021 and 2025.

I analyzed:

- Changes in annual gross sales.
- Changes in units sold.
- Annual recorded profit trends.
- Annual profit margin stability.

I also examined year-by-year results for selected categories to distinguish sustained increases from fluctuations between the starting and ending years.

This helped determine whether higher sales values were accompanied by increased sales volume.

---

## Power BI Dashboard

The Power BI report translates the analytical results into a business-oriented dashboard designed to support category performance evaluation.

### Page 1 — Sales & Profitability Overview

Provides a financial overview of the U.S. e-commerce dataset.

**Key Performance Indicators:**

| KPI | Value |
|---|---:|
| Total Gross Sales | $113.45M |
| Total Recorded Profit | $41.92M |
| Recorded Profit Margin | 36.95% |

The page includes:

- Total Profit by Product Category
- Profit Margin by Product Category
- Year Filter
- Key Business Insight

The primary finding is that **high total profit does not necessarily indicate high relative profitability**.

### Page 2 — Assortment Expansion Opportunities

This page focuses on category growth and potential business opportunities.

Visualizations include:

- Annual Gross Sales by Selected Categories
- Sales Growth by Product Category
- Sales Growth vs. Units Growth
- Category Opportunity Matrix

The Opportunity Matrix combines sales growth, profit margin, and total recorded profit to support category comparisons.

---

## Key Findings & Business Recommendations

### 1. Expansion Opportunities — Books & Media and Pet Supplies

**Books & Media**

- Gross Sales Growth: **+10.34%**
- Units Growth: **+9.20%**
- Recorded Profit Margin: **49.25%**

Books & Media demonstrated the strongest growth in sales value and units sold among the 15 categories.

Unit sales increased in every year from 2021 to 2025, while recorded profit also increased over the full period and profit margins remained relatively stable.

**Pet Supplies**

- Gross Sales Growth: **+4.19%**
- Units Growth: **+2.38%**
- Recorded Profit Margin: **44.76%**

Pet Supplies also demonstrated positive overall growth, although annual performance was less consistent.

**Recommendation:** Prioritize Books & Media for further assortment expansion evaluation and consider Pet Supplies as a secondary candidate.

### 2. High Profitability — Grocery

- Recorded Profit Margin: **51.54%**
- Gross Sales Growth: **−0.88%**
- Units Growth: **−2.64%**

Grocery achieved the highest recorded profit margin among the 15 categories but experienced a slight decline in sales value and sales volume.

**Recommendation:** Investigate the reasons for declining sales volume and evaluate opportunities to maintain profitability before expanding the category.

### 3. Total Profit Leader — Electronics

- Total Recorded Profit: **approximately $7.00M**
- Recorded Profit Margin: **28.65%**
- Gross Sales Growth: **−0.12%**

Electronics contributed the highest total recorded profit but had the lowest profit margin among the categories.

The analysis also showed that Electronics had the highest product cost ratio (60.14%) and the lowest aggregate markup (66.29%).

**Recommendation:** Review product-level costs, pricing, and margins to identify possible profitability improvements.

---

## Business Conclusion

The analysis demonstrates why product category performance should not be evaluated using a single metric.

Categories with high total profit may have relatively low profit margins, while categories with high margins may not demonstrate growth in sales volume.

Based on the historical patterns in this dataset, **Books & Media emerged as the strongest candidate for further assortment expansion evaluation**, followed by Pet Supplies.

However, these findings are preliminary and do not establish that expanding these categories would necessarily increase future profit.

Additional analysis of inventory, operating expenses, product-level performance, and customer demand would be required before making an actual investment decision.

---

## Project Limitations

Several limitations should be considered when interpreting the results:

- **Data authenticity:** The dataset is likely synthetic, and the original transaction source could not be independently verified.
- **Financial definitions:** Recorded profit was calculated using the dataset's financial formulas and may not represent conventional accounting net profit.
- **Data reconciliation:** The reconciled item-level dataset matches recorded order totals but cannot establish the authenticity of the underlying transactions.
- **Historical analysis:** Growth calculations are based on 2021–2025 data and do not predict future performance.
- **Business context:** Inventory levels, additional operating costs, market conditions, and other factors affecting assortment decisions were not available.

These limitations mean that recommendations should be treated as hypothetical business scenarios rather than actionable conclusions about the real U.S. e-commerce market.

---

## Project Files

| File | Description |
|---|---|
| `01_data_exploration.ipynb` | Python exploration, financial validation, reconciliation, and business analysis |
| `ECommerce_Profitability_Analysis.pdf` | Two-page Power BI dashboard |
| `images/` | Dashboard preview images |

## Dashboard Preview

<img width="935" height="533" alt="Sales   Profitability Overview" src="https://github.com/user-attachments/assets/bec2b8ec-a44d-45ca-a541-7f5b245a68e0" />
<img width="637" height="730" alt="Assortment  Expansion Opportunities" src="https://github.com/user-attachments/assets/1999badd-93d4-4380-8e54-a6fc9de367ff" />


**Project Type:** Independent Data Analytics Portfolio Project

**Primary Focus:** Data Understanding, Data Quality Assessment, Financial Validation, Business Analytics, and Business Intelligence.
