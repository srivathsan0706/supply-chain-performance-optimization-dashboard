[Star_Schema_Documentation.md](https://github.com/user-attachments/files/27761773/Star_Schema_Documentation.md)
# 🌟 Star Schema Documentation

> **Supply Chain Performance Optimization Dashboard** — Power BI Data Model

The model separates transactional supply chain data into a central fact table connected to multiple dimension tables for filtering, categorization, and business analysis.

---

## 📁 Table of Contents

- [🗄️ Central Fact Table](#️-central-fact-table)
- [📐 Dimension Tables](#-dimension-tables)
- [📏 Measures Table](#-measures-table)
- [🔗 Relationship Structure](#-relationship-structure)
- [🧱 Data Modeling Approach](#-data-modeling-approach)
- [⚠️ Current Modeling Limitations](#️-current-modeling-limitations)
- [🚀 Future Enhancements](#-future-enhancements)
- [⚙️ Power BI Features Used](#️-power-bi-features-used)

---

## 🗄️ Central Fact Table

### `supply_chain_data_001`

The primary fact table containing all operational and transactional supply chain data.

#### 📊 Key Metrics Included

| Metric | Description |
|---|---|
| `Revenue` | Total income from transactions |
| `Logistics Cost` | End-to-end logistics spend |
| `Manufacturing Cost` | Production cost per unit |
| `Shipping Cost` | Cost of shipment per transaction |
| `Shipping Days` | Transit duration |
| `Manufacturing Lead Time` | Time from order to production completion |
| `Units Sold` | Quantity sold per transaction |
| `Stock Levels` | Available inventory |
| `Stock Gap` | Difference between demand and available stock |
| `Defect Rate` | Product quality failure percentage |
| `Inspection Status` | Pass / Fail inspection outcome |

#### 🎯 Business Purpose

Used for KPI calculations, supply chain performance analysis, revenue tracking, logistics monitoring, inventory analysis, and supplier quality evaluation.

---

## 📐 Dimension Tables

### `DimProduct`

Contains product-related attributes for SKU-level and category reporting.

| Column | Type |
|---|---|
| `SKU` | Product identifier |
| `Product Category` | Category grouping |

> Used for product category analysis, SKU-level reporting, and product performance tracking.

---

### `DimSupplier`

Contains supplier identity information for quality benchmarking.

| Column | Type |
|---|---|
| `Supplier Name` | Name of the supplier |

> Used for supplier quality analysis, defect rate comparison, and supplier ranking.

---

### `DimLocation`

Contains geographical location data for regional analysis.

| Column | Type |
|---|---|
| `Location` | City or region name |

> Used for regional revenue analysis, location-wise inventory tracking, and logistics performance monitoring.

---

### `DimRoute`

Contains transportation route definitions.

| Column | Type |
|---|---|
| `Routes` | Route identifier or description |

> Used for route-level logistics analysis and shipping performance tracking.

---

### `DimMode`

Contains transportation mode classifications.

| Column | Type |
|---|---|
| `Transportation Modes` | Mode of transport (Air / Sea / Road) |

> Used for transport mode analysis and shipping efficiency evaluation.

---

### `DimCarrier`

Contains carrier-level information for logistics benchmarking.

| Column | Type |
|---|---|
| `Carrier` | Carrier or logistics provider name |

> Used for carrier performance analysis and logistics benchmarking.

---

## 📏 Measures Table

### `MeasuresTable`

A dedicated DAX measures table, kept separate from transactional and dimension tables for clean model organization.

#### ✅ Benefits

- Improved model organization and readability
- Easier measure management at scale
- Better dashboard maintainability
- Cleaner DAX authoring experience

#### 📌 Example Measures Stored

| Measure | Purpose |
|---|---|
| `Revenue Efficiency` | Revenue per unit of logistics cost |
| `Avg Defect Rate` | Average supplier defect rate |
| `Adjusted Cost Score` | Weighted cost + defect penalty score |
| `Avg Logistics Cost` | Mean logistics spend per transaction |

---

## 🔗 Relationship Structure

The model follows a **one-to-many** relationship structure between all dimension tables and the central fact table, with **single-direction filtering**.

```
DimProduct   ──────────┐
DimSupplier  ──────────┤
DimLocation  ──────────┼──► supply_chain_data_001
DimRoute     ──────────┤
DimMode      ──────────┤
DimCarrier   ──────────┘
```

| Relationship | Type | Filter Direction |
|---|---|---|
| DimProduct → Fact | One-to-Many | Single |
| DimSupplier → Fact | One-to-Many | Single |
| DimLocation → Fact | One-to-Many | Single |
| DimRoute → Fact | One-to-Many | Single |
| DimMode → Fact | One-to-Many | Single |
| DimCarrier → Fact | One-to-Many | Single |

---

## 🧱 Data Modeling Approach

The dashboard follows a **star schema-inspired** dimensional modeling approach:

```
         DimProduct
             │
 DimCarrier──┤
             │
 DimMode ────┼──── supply_chain_data_001 ◄──── MeasuresTable
             │           (Fact)
 DimRoute ───┤
             │
DimLocation──┤
             │
 DimSupplier─┘
```

| Layer | Role |
|---|---|
| **Fact Table** | Stores all transactional metrics and KPI source data |
| **Dimension Tables** | Store descriptive business attributes for slicing |
| **Measures Table** | Centralizes all DAX logic separately from data tables |

This structure improves reporting clarity, dashboard scalability, DAX calculation efficiency, and filtering performance.

---

## ⚠️ Current Modeling Limitations

> The current model is optimized for **portfolio-level analytical reporting** and dashboard development.

Some descriptive attributes still exist within the fact table for simplified modeling and faster dashboard development. Full normalization is scoped for future iterations.

---

## 🚀 Future Enhancements

| Enhancement | Description |
|---|---|
| 📅 Date Dimension Table | Dedicated `DimDate` for time intelligence |
| ⏱️ Advanced Time Intelligence | YoY, MoM, rolling averages |
| 🧹 Fact Table Normalization | Move remaining attributes to dimension tables |
| 📈 Forecasting Models | Predictive demand and cost modeling |
| 📦 Inventory Optimization | Min/max stock threshold analysis |
| 🏆 Supplier Benchmarking | Multi-metric supplier scorecards |

---

## ⚙️ Power BI Features Used

| Feature | Usage |
|---|---|
| DAX Measures | All KPI and analytical calculations |
| KPI Calculations | Revenue, cost, defect, and logistics metrics |
| Context-Based Filtering | Slicer and cross-filter interactions |
| Ranking Functions | `RANKX` for supplier and location rankings |
| Conditional Formatting | Defect rate and cost visual alerts |
| Dynamic Titles | Context-aware chart labels |
| Drillthrough Reporting | SKU and supplier deep-dive pages |

---

<div align="center">

*Built for the Supply Chain Performance Optimization Dashboard — Power BI*

</div>
