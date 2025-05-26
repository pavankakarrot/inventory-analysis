# Inventory Analysis & Optimization Dashboard

A comprehensive end-to-end inventory management analysis for a spirits distribution business. We clean, process, analyze and visualize six source files to deliver data-driven stock recommendations, vendor insights, and supply-chain metrics.

---

## Table of Contents

1. [Business Context](#business-context)  
2. [Data Sources](#data-sources)  
3. [Data Cleaning & Processing](#data-cleaning--processing)  
4. [Analysis Methodology](#analysis-methodology)  
5. [Visualization (Tableau)](#visualization-tableau)  
6. [Key Business Insights](#key-business-insights)  
7. [Challenges & Learnings](#challenges--learnings)  
8. [Repository Structure](#repository-structure)  
9. [How to Run](#how-to-run)

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
3. **Impute City from Store**
   ```python
     mapping = inv[['Store','City']].drop_duplicates().set_index('Store')['City'].to_dict()
inv['City'].fillna(inv['Store'].map(mapping), inplace=True)```
4. **Compute Durations**
  ```python
    purchases['SupplyDuration'] = (purchases['ReceivingDate'] - purchases['PODate']).dt.days
purchases['PaymentDuration'] = (purchases['PayDate'] - purchases['InvoiceDate']).dt.days```
