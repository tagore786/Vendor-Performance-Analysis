# Vendor-Performance-Analysis

<img width="1803" height="1004" alt="Workflow" src="https://github.com/user-attachments/assets/af927774-8e08-4c87-979a-0f1dfebbb5ae" />

---

## Problem Statement

This project analyzes vendor performance for a wholesale/retail business. The goal is to find which vendors and brands are profitable, where pricing or inventory issues are hurting the business, and how to fix them using data.

---

## What This Project Does

- Identifies top-performing and underperforming vendors
- Highlights slow-moving or stuck inventory
- Detects pricing inconsistencies
- Analyzes how bulk purchases affect margins
- Answers core business questions with clear data

---

## Project Highlights

- Identify top-performing and underperforming vendors
- Highlight stuck or slow-moving inventory
- Spot pricing mismatches
- Understand how bulk purchases impact margins
- Answer actual business questions through clean, structured analysis

---

## Why This Matters

Businesses constantly ask:
- Which vendors are boosting profits — and which are draining them?
- Where are we losing money — in pricing, inventory, or buying decisions?
- How can we optimize vendor relationships?

This project tackles those questions head-on using data and dashboards.

---

## How It Works

1. **Ingest Data**  
   Load CSVs into SQLite using Python. Handles large files and logs each step.
2. **Clean + Analyze**  
   Use SQL and Pandas to explore, join, and clean the data.
3. **Generate Insights**  
   Calculate KPIs like margins, sales, stuck inventory, and vendor-level performance.
4. **Visualize**  
   Build a Power BI dashboard that’s easy to read and ready for business users.

---

## What’s in the Data?

- Opening and closing inventory
- Vendor purchases and pricing
- Product-level sales data
- Vendor invoice details

All datasets are loaded into a single SQLite database.

---

## Key Files

| File                            | Purpose                                                   |
|---------------------------------|-----------------------------------------------------------|
| `Ingest_db.ipynb`              | Loads CSVs into SQLite with logging and error handling    |
| `EDA.ipynb`                    | Cleans and explores data using Pandas and SQL             |
| `Vendor Performance Analysis.ipynb`     | Analyzes vendor/brand-level performance                   |
| `inventory.db`                 | Final database used for querying and analysis             |
| `vendor sales summary.csv`              | Merged and cleaned data used in Power BI                  |
| `Workflow.png`                 | Project flow diagram                                      |
| `Dashboard.jpg`                | Preview of interactive Power BI dashboard                 |

---

## 🔍 Business Questions Answered

 - Which vendors contribute most to profit and total sales?  
 - Which vendors have poor performance and need reevaluation?  
 - How much capital is locked in unsold inventory?  
 - What is the average profit margin across brands and vendors?  
 - How do actual purchase prices compare with sales prices?  
 - Are there patterns in low-performing brands?

---

## 📊 Key Metrics (From Dashboard)

| Metric                | Value         |
|------------------------|---------------|
| **Total Sales**        | \$441.41M     |
| **Total Purchase**     | \$307.34M     |
| **Gross Profit**       | \$134.07M     |
| **Profit Margin**      | 38.72%        |
| **Unsold Capital**     | \$2.71M       |

---

## 📈 Statistical & Analytical Insights

📌 **Vendor Segmentation** using sales and profit data  
📌 **Profit Margin Distribution** analysis across brands  
📌 **95% Confidence Intervals** for top and bottom vendors  
📌 Outlier detection using histograms and scatter plots  
📌 Grouped analysis by `VendorName`, `Brand`, `PurchasePrice`, and `SalesPrice`

---

## 📉 Low Performance Detected

### Vendors with <1% Contribution:
- Dunn Wine Brokers  
- Circa Wines  
- Park Street Imports  
- Highland Wine Merchants  
- Alisa Carr Beverages  

### Brands with Low Profit Margins:
- Identified using scatterplot: `Total Sales vs Avg Profit Margin`
- Segregated by `TargetBrand` classification (Yes/No)

---

## Example Code Snippets

**Data Ingestion**
```python
import pandas as pd
import sqlite3

conn = sqlite3.connect('inventory.db')
df = pd.read_csv('data/sales.csv')
df.to_sql('sales', conn, if_exists='replace', index=False)
```

**Cleaning & Merging**
```python
df_sales = pd.read_sql('SELECT * FROM sales', conn)
df_purchases = pd.read_sql('SELECT * FROM purchases', conn)
merged = pd.merge(df_sales, df_purchases, on='product_id', how='left')
merged = merged.dropna()
merged['date'] = pd.to_datetime(merged['date'])
```

**Analysis**
```python
vendor_sales = merged.groupby('vendor_name')['sales_amount'].sum().reset_index()
top_vendors = vendor_sales.sort_values(by='sales_amount', ascending=False).head(5)
```

**Export for Power BI**
```python
merged.to_csv('final_table.csv', index=False)
```

---

## Dashboard Preview

<img width="1403" height="790" alt="Screenshot 2025-09-03 224924" src="https://github.com/user-attachments/assets/0bc366e3-dac2-42b1-84f6-57e440e1f26b" />

Interactive Power BI dashboard covering sales, purchases, margins, vendor health, and more.

---

## 📷 Dashboard Highlights

- 💡 KPI Cards: Sales, Purchase, Profit, Margin
- 🔼 Bar Charts: Top Vendors and Brands by Sales
- 🔽 Low Performer Lists
- 🧠 Profit Margin vs Sales Scatter for Target Analysis

---

## 🔧 Tools & Technologies

| Category         | Tools & Libraries                         |
|------------------|-------------------------------------------|
| Programming      | Python, SQLite                            |
| Data Extraction  | SQL queries via `sqlite3`                 |
| Data Analysis    | pandas, numpy, matplotlib, seaborn        |
| Statistics       | Scipy, Confidence Interval estimation     |
| Dashboard        | Power BI                                  |

---

## 🧪 Methodology

1. **Connected** to SQLite database and extracted tables: `purchases`, `purchase_prices`, `sales`, and `vendor_invoice`
2. **Created unified summary table** combining sales, purchases, freight costs, and prices
3. **Performed EDA** including:
   - Vendor-wise purchase & sales aggregation
   - Calculation of gross profit and profit margin
4. **Used statistical methods**:
   - Mean & CI (confidence intervals) for vendor segments
   - Margin distribution for brand classification
5. **Built dashboard** using Power BI with KPI cards, bar plots, donut charts, and scatter visualizations

---

## How to Run

1. Clone the repo  
   ```bash
   git clone https://github.com/eharshit/End-to-End-Vendor-Insights
   ```
2. Install dependencies  
   ```bash
   pip install pandas numpy
   ```
3. Launch Jupyter Notebook  
   ```bash
   jupyter notebook
   ```
4. Run the notebooks in this order:
   - `Ingestion.ipynb`
   - `Exploratory Data Analysis.ipynb`
   - `Vendor Performance.ipynb` (optional deep dive)
5. Open Power BI and connect to `final_table.csv`

---

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you want to improve.

---
