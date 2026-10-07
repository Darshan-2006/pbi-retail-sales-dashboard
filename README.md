# Retail Sales Power BI Dashboard

**End-to-end Power BI project analysis** — data model, DAX measures, report pages, and design patterns.

[![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-yellow.svg)]()
[![Model](https://img.shields.io/badge/Model-Star%20Schema-blue)]()
[![Pages](https://img.shields.io/badge/Report%20Pages-16-success)]()
[![Visuals](https://img.shields.io/badge/Visuals-68-orange)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Overview

This repository documents a complete Power BI solution (`PBI_P1.pbix`) built for **retail sales analysis**. The project demonstrates:

- Dimensional modelling (star schema)
- Extensive DAX measure development (aggregations, time intelligence, filter context)
- Multi-page interactive reporting
- Slicers, cards, charts, and tables for operational & analytical views

| Item                    | Detail                          |
|-------------------------|---------------------------------|
| Source file             | `PBI_P1.pbix`                   |
| Power BI version        | 1.28 (Created from Cloud 2025.10) |
| Theme                   | CY25SU10                        |
| Report pages            | 16                              |
| Total visuals           | 68                              |
| Model tables            | 5 (1 fact + 4 dimensions)       |
| Explicit DAX measures   | 35+                             |

---

## Repository Structure

```
pbi-retail-sales-dashboard/
├── PBI_P1.pbix          # Original Power BI file
├── README.md            # This analysis
├── LICENSE
└── .gitignore
```

---

## 1. Data Model (Star Schema)

The model follows a classic **star schema** with one fact table and four dimensions:

```
                    ┌─────────────────┐
                    │  Dim Calendar   │
                    │  Date, Year,    │
                    │  Quarter, Month │
                    └────────┬────────┘
                             │
┌─────────────────┐   ┌──────┴──────┐   ┌─────────────────┐
│  Dim Customer   │───│  Fact Sales │───│  Dim Product    │
│  CustomerID     │   │  Sales      │   │  ProductID      │
│  CustomerName   │   │  Profit     │   │  ProductName    │
│  Region, City   │   │  Quantity   │   │  Category       │
└─────────────────┘   │  CostPrice  │   │  Brand          │
                      │  UnitPrice  │   └─────────────────┘
                      │  Discount   │
                      │  Gross Sales│
┌─────────────────┐   │  Discounted │
│  Dim Store      │───│  Amount     │
│  StoreID        │   └─────────────┘
│  StoreName      │
│  Channel        │
│  SalespersonID  │
└─────────────────┘
```

### Dimension Columns

| Table         | Key Columns                                      |
|---------------|--------------------------------------------------|
| Dim Customer  | CustomerID, CustomerName, Region, City           |
| Dim Product   | ProductID, ProductName, Category, Brand          |
| Dim Store     | StoreID, StoreName, Channel, SalespersonID       |
| Dim Calendar  | Date, Year, Quarter, Month                       |

### Fact Columns (from usage)

Sales, Profit, Quantity, CostPrice, UnitPrice, Discount, Gross Sales, Discounted Amount

---

## 2. DAX Measures Catalogue

Measures are organised by purpose. Names below are taken directly from the report model.

### Core Aggregations
| Measure | Purpose |
|---------|---------|
| Total Sales | Primary revenue measure |
| Total Profit | Primary profitability measure |
| Total Orders | Order count |
| Total Customers | Distinct customer count |
| Total Product | Product count |
| Total Gross Sales | Pre-discount revenue |
| Total Discounted Amount | Discount value given |

### Statistical / Min-Max
| Measure | Purpose |
|---------|---------|
| Maximum Profit / Minimum Profit | Profit extremes |
| Max CostPrice / Min UnitPrice | Cost & price bounds |
| Max Discount | Highest discount applied |
| Avg Gross Sales / Avg Discounted Amount | Averages |
| Min / Max Gross Sales, Discounted Amount | Range analysis |

### Filtered / Segmented Measures
| Measure | Purpose |
|---------|---------|
| Total Sales (Brand A) | Brand-specific sales |
| Total Sales (S1) / Min Sales (S1) | Store-specific |
| Max Profit (Online) | Online channel max profit |
| Online Sales | Online channel sales |
| Total Quantity (South) | Region-filtered quantity |
| Avg Quantity (Chennai) | City-level average |
| Max Profit (Chennai) / Min Profit (Delhi) | City-level extremes |
| Avg Sales (Kolkata) | City-level average |
| No of Customers (Bangalore) | City-level customer count |

### Filter Context / ALL / ALLEXCEPT Patterns
| Measure | Technique Demonstrated |
|---------|------------------------|
| Total Sales (ALL) | Remove all filters |
| Total Profit (All Store) | ALL on store dimension |
| Total Profit AE / Total Profit AllExcept | ALLEXCEPT pattern |
| Total Customer Region (AllExcept) | ALLEXCEPT on region |
| Total Sales Region (%) / Total Sales Region (%) RF | % of total with REMOVEFILTERS |
| Total Profit (%) / Total Profit (%) RF | Profit share of total |
| Total Quantity % BY City / … RF | City contribution % |

### Time Intelligence
| Measure | Pattern |
|---------|---------|
| Total YTD (Sales) / Total Ytd (Profit) | Year-to-date |
| Total MTD (Sales) | Month-to-date |
| Total QTD (Sales) / TotalQTD | Quarter-to-date |
| DatesYTD / DatesYTD (Profit) | DATESYTD variant |
| DatesMTD (Sales/Profit) | DATESMTD |
| DatesQTD (Sales/Profit) | DATESQTD |
| Previous Year (Py) | SAMEPERIODLASTYEAR / DATEADD |
| Previous Month (PM) / Previous Month (Profit) | Prior month comparison |

> The measure set shows deliberate practice of **filter context**, **time intelligence**, and **percentage-of-total** patterns — core DAX skills for analytical reports.

---

## 3. Report Pages (16)

| # | Page Name | Focus | Key Visuals |
|---|-----------|-------|-------------|
| 1 | Page 1 | Exploratory charts | Table, column, pie, slicer, clustered column |
| 2 | Page 2 | Region performance | Pie (Sales by Region), clustered column (Profit by Region) |
| 3 | Page 3 | Customer dimension | Customer table + Brand & Region slicers |
| 4 | Page 4 | Product dimension | Product table + Category slicer |
| 5 | Page 5 | Store dimension | Store table, Channel slicer, Total Profit card |
| 6 | Page 6 | Measure validation | Multiple tables & cards (Brand A, S1 sales, etc.) |
| 7 | Page 7 | KPI cards | Max/Min/Avg cards for profit, discount, quantity, customers |
| 8 | Page 8 | ALL filter demo | Cards & table comparing filtered vs ALL sales/profit |
| 9 | Page 9 | Profit vs All Store | Table with Total Profit vs Total Profit (All Store) |
| 10 | Page 10 | Region % contribution | Sales region % and REMOVEFILTERS variants |
| 11 | Page 11 | Profit % & quantity % | City/region contribution percentages |
| 12 | Page 12 | ALLEXCEPT demos | Total Profit AE, AllExcept, Customer Region |
| 13 | Page 13 | Time intelligence (YTD/MTD/QTD) | 6 line charts – DATESYTD/MTD/QTD for Sales & Profit |
| 14 | Page 14 | Prior period comparison | Line charts – Previous Year, Previous Month, MTD/QTD/YTD |
| 15 | **Overview** | Executive dashboard | KPI cards, 5 slicers, region charts, brand/category tables |
| 16 | **Time Series Analysis** | Trend dashboard | KPI cards, slicers, YoY/MoM lines, category mix, online sales |

### Flagship Pages

**Overview (Page 15)**  
- KPI strip: Total Sales, Customers, Products, Profit, Orders  
- Slicers: Region, Category, City, Month, Year  
- Clustered columns: Sales & Profit by Region  
- Tables: Brand × City, Brand × Category with Sales & Profit  

**Time Series Analysis (Page 16)**  
- Same KPI strip + slicers  
- Line charts: Previous Year vs YTD Sales; Previous Month vs YTD Profit  
- Category contribution table (Sales % / Profit %)  
- Column chart: Online Sales by Quarter  

---

## 4. Visual Inventory

| Visual Type            | Count | Usage |
|------------------------|-------|-------|
| Table (tableEx)        | 17    | Dimension lists, measure comparisons, % tables |
| Slicer                 | 17    | Region, Category, Brand, City, Channel, Month, Year |
| Card                   | 13    | KPI highlighting |
| Line Chart             | 11    | Time series & period comparisons |
| Clustered Column Chart | 4     | Region / category comparisons |
| Column Chart           | 2     | Online sales by quarter, cost by brand |
| Pie Chart              | 2     | Region & store share |
| Text Box               | 2     | Dashboard titles |

**Total: 68 visuals across 16 pages**

---

## 5. Techniques Demonstrated

### Data Modelling
- Star schema with clear fact/dimension separation  
- Calendar dimension for time intelligence  
- Relationships implied between Fact Sales and all dimensions  

### DAX Patterns
1. **Simple aggregations** — SUM, COUNT, DISTINCTCOUNT style measures  
2. **Filter modification** — CALCULATE + ALL / ALLEXCEPT / REMOVEFILTERS  
3. **Percentage of total** — DIVIDE(measure, CALCULATE(measure, ALL(...)))  
4. **Time intelligence** — TOTALYTD / DATESYTD, DATESMTD, DATESQTD, SAMEPERIODLASTYEAR  
5. **Segmented measures** — hard-filtered by Brand, Store, City, Channel, Region  
6. **Min / Max / Average** statistical measures  

### Report Design
- Progressive pages: from raw dimension views → measure testing → polished executive dashboards  
- Consistent slicer set on flagship pages (Region, Category, City, Month, Year)  
- Mix of analytical (line charts, % tables) and operational (KPI cards, dimension tables) views  

### Theme & Settings
- Base theme: **CY25SU10**  
- Type detection & relationship import enabled  
- Created from Power BI Service (Cloud) release 2025.10  

---

## 6. How to Open & Explore

1. Download / clone this repository  
2. Open `PBI_P1.pbix` in **Power BI Desktop** (latest version recommended)  
3. Review the **Model** view to inspect relationships and the star schema  
4. Open **Overview** and **Time Series Analysis** pages for the finished dashboards  
5. Inspect measures in the Fields pane under **Fact Sales** to study DAX patterns  

```bash
# After cloning
# Open PBI_P1.pbix with Power BI Desktop
```

---

## 7. Suggested Next Steps

- Rename exploratory pages (Page 1–14) to descriptive titles for production use  
- Add a dedicated **Data Dictionary** page documenting each measure’s DAX  
- Implement row-level security (RLS) by Region or Store if required  
- Publish to Power BI Service and schedule refresh  
- Consider a mobile layout for the Overview page  

---

## License

MIT License — see [LICENSE](LICENSE).

---

*Analysis derived from the Power BI report layout, diagram model, metadata, and measure references embedded in `PBI_P1.pbix`.*
