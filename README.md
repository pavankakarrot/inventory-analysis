# Inventory Analysis & Optimization Dashboard

A comprehensive end-to-end inventory management analysis for a spirits distribution business. We clean, process, analyze and visualize six source files to deliver data-driven stock recommendations, vendor insights, and supply-chain metrics.

---

![Alt txt](https://github.com/pavankakarrot/inventory-analysis/blob/main/Final_Dashboard.png)

## Table of Contents

1. [Business Context](#business-context)  
2. [Data Sources](#data-sources)  
3. [Data Cleaning & Processing](#data-cleaning--processing)  
4. [Analysis Methodology](#analysis-methodology)  
5. [Visualization (Tableau)](#visualization-tableau)  
6. [Key Business Insights](#key-business-insights)  
7. [Challenges & Learnings](#challenges--learnings) 

---

## Business Context

The spirit distribution company faced:
- **Overstock & stockouts** across hundreds of SKUs  
- **Variable supply and payment cycles** complicating cash flow  
- **Suboptimal vendor utilization** leading to margin leakage  

Our goal was to deliver a data-driven dashboard that:
1. Recommends optimal stock levels for top products  
2. Analyzes vendor performance by volume and cost  
3. Tracks supply vs. payment durations  
4. Highlights cyclical sales patterns

---

## Data Sources

| File                                | Description                                   |
|-------------------------------------|-----------------------------------------------|
| `2017PurchasePricesDec.csv`         | Unit prices by vendor on Dec 2017             |
| `BegInvFINAL12312016.csv`           | Beginning inventory as of 12/31/2016          |
| `EndInvFINAL12312016.csv`           | Ending inventory as of 12/31/2016             |
| `InvoicePurchases12312016.csv`      | Invoice details for purchases in 2016         |
| `PurchasesFINAL12312016.csv`        | Purchase orders and receiving dates           |
| `SalesFINAL12312016.csv`            | Sales orders and quantities                   |

All raw CSVs live under `/data/raw/` in the repo.

---

## Data Cleaning & Processing

We use a Python notebook (`Inve_analysis.ipynb`) to:

1. **Load & Concatenate** CSVs with `pandas`  
2. **Handle Missing Values**  
   ```python
   # Drop rows missing critical fields
   for col in ['Description','Size','Volume']:
       df = df[df[col].notna()]

## Analysis Methodology

1. **Best-Selling Products**  
   - Aggregated total sales quantity by `Brand` and `Description`.  
   - Ranked products to identify top performers (e.g., Smirnoff 80 Proof with 28.5K units).  

2. **Vendor Performance**  
   - Calculated total purchase volume and cost per vendor.  
   - Highlighted top 7 vendors by volume and by cost to uncover concentration risks.  

3. **Supply vs. Payment Durations**  
   - Converted `PODate`, `ReceivingDate`, `InvoiceDate`, and `PayDate` to datetime.  
   - Computed `SupplyDuration` = ReceivingDate – PODate, and `PaymentDuration` = PayDate – InvoiceDate.  
   - Plotted trends and distributions to measure efficiency (avg supply = 7.6 days, avg payment = 35.6 days).  

4. **Recommended Stock Levels**  
   - Calculated average daily sales:  
     ```python
     avg_daily_sales = total_sales_quantity / total_days_sold
     ```  
   - Defined safety buffer based on supply duration variance.  
   - Formula:  
     ```
     RecommendedStock = avg_daily_sales × (mean_supply_duration + safety_days)
     ```  
   - Generated stock recommendations for top 10 SKUs.

---

## Visualization (Tableau)

- **Dashboard Layout**  
  - Arranged in three sections:  
    1. **Inventory Trends** (Best Sellers, Price & Quantity Over Time)  
    2. **Vendor & Supply Analysis** (Top Vendors by Volume/Cost, Supply vs. Payment Durations)  
    3. **Stock Recommendations** (Distribution plots and bar chart of recommended levels)

- **Key Tableau Features Used**  
  - **Level-of-Detail (LOD) Expressions** for fixed calculations (e.g., Daily Sales Rate).  
  - **Dual-Axis Charts** to overlay trends (e.g., supply vs. payment durations).  
  - **Parameterized Filters** to dynamically view by brand, vendor, or timeframe.  
  - **Table Calculations** for running totals and moving averages.

- **Interactivity**  
  - Hover tooltips with detailed metrics.  
  - Dropdown selectors for product categories and date ranges.  
  - Highlight actions to drill down from summary to detail.

---

## Key Business Insights

- **Top Products Drive Revenue**  
  - *Smirnoff 80 Proof* leads with **28.5K** units sold and a recommended stock of **5K** to meet demand.  
  - Weekly sales seasonality peaks every 7 days, informing restock frequency.

- **Vendor Concentration**  
  - **DIAGEO NORTH AMERICA INC** accounts for **\$5.5M** in volume and **\$3.9M** in cost.  
  - Top three vendors represent **47%** of total purchase spend, highlighting negotiation leverage opportunities.

- **Supply Chain Efficiency**  
  - Average supply duration: **7.6 days** — consistent across multiple intervals.  
  - Average payment cycle: **35.6 days** — opportunity to optimize payment terms for cash flow benefits.

- **Inventory Optimization**  
  - Safety buffer of **2–3 days** added to mean supply duration improves fill rates.  
  - Identified **1.5M** unit gap between current stock and optimal levels across top SKUs, reducing stockouts and overstock risk.

---

## Challenges & Learnings

- **Data Integration Complexity**  
  - **Multiple date formats** required robust parsing logic in Python.  
  - Merging six distinct CSV files necessitated careful key matching and null handling.

- **Advanced Tableau Techniques**  
  - Crafting **LOD expressions** took iterative testing to get accurate fixed-level calculations.  
  - Implementing **dual-axis** and **table calculations** improved storytelling but added complexity.

- **Balancing Precision vs. Simplicity**  
  - Determining the right **safety buffer** involved analyzing supply duration variance.  
  - Simplifying the dashboard layout for end-users without sacrificing depth was key.

- **Business Impact Realization**  
  - Translating technical metrics into **actionable stock recommendations** reinforced the value of data-driven decisions.  
  - Understanding vendor dynamics and cash flow cycles enabled targeted negotiations and process improvements.






