[DAX_Measures_Documentation.md](https://github.com/user-attachments/files/27761196/DAX_Measures_Documentation.md)
# 📊 DAX Measures Documentation

> **Supply Chain Performance Optimization Dashboard** — Power BI

This document catalogs all DAX measures used in the dashboard, organized by business function and analytical purpose.

---

## 📁 Table of Contents

- [💰 Financial KPIs](#-financial-kpis)
- [📦 Inventory & Supply Chain KPIs](#-inventory--supply-chain-kpis)
- [🏭 Supplier & Quality KPIs](#-supplier--quality-kpis)
- [🔍 Inspection Metrics](#-inspection-metrics)
- [🚚 Logistics Metrics](#-logistics-metrics)
- [🧠 Advanced Analytical Measures](#-advanced-analytical-measures)
- [🎨 Visual Enhancement Measures](#-visual-enhancement-measures)

---

## 💰 Financial KPIs

### Total Revenue

```dax
Total Revenue =
SUM(supply_chain_data_001[revenue])
```

> Calculates total revenue generated across all supply chain transactions.

---

### Total Logistics Cost

```dax
Total Logistics Cost =
SUM(supply_chain_data_001[Logistics Cost])
```

> Calculates the overall logistics expenditure.

---

### Avg Logistics Cost

```dax
Avg Logistics Cost =
AVERAGE(supply_chain_data_001[Logistics Cost])
```

> Measures average logistics cost per transaction.

---

### Revenue Efficiency

```dax
Revenue Efficiency =
DIVIDE(
    [Total Revenue],
    [Total Logistics Cost],
    0
)
```

> Measures revenue generated for every unit of logistics cost incurred.

---

### Revenue Per Unit

```dax
Revenue Per Unit =
DIVIDE(
    [Total Revenue],
    [Total Units Sold],
    0
)
```

> Measures average revenue earned per unit sold.

---

### Adjusted Cost Score

```dax
Adjusted Cost Score =
VAR BaseCost      = [Avg Logistics Cost]
VAR DefectPenalty = [Avg Defect Rate] * 50

RETURN
    BaseCost + DefectPenalty
```

> Combines logistics cost and defect rate impact into a weighted operational cost score.

---

## 📦 Inventory & Supply Chain KPIs

### Fill Rate

```dax
Fill Rate =
DIVIDE(
    SUM(supply_chain_data_001[Stock levels]),
    SUM(supply_chain_data_001[Units Sold]),
    0
)
```

> Measures inventory fulfillment efficiency.

---

### Avg Stock Gap

```dax
Avg Stock Gap =
AVERAGE(supply_chain_data_001[Stock Gap])
```

> Calculates average stock shortage levels.

---

### Critical Understock SKUs

```dax
Critical Understock SKUs =
COUNTROWS(
    FILTER(
        supply_chain_data_001,
        supply_chain_data_001[Stock Gap] > 400
    )
)
```

> Counts SKUs with critically high stock shortages (gap > 400 units).

---

## 🏭 Supplier & Quality KPIs

### Avg Defect Rate

```dax
Avg Defect Rate =
AVERAGE(supply_chain_data_001[Defect Rate Clean])
```

> Calculates average product defect rate across suppliers.

---

### Supplier Quality Rank

```dax
Supplier Quality Rank =
RANKX(
    ALLSELECTED(DimSupplier[Supplier name]),
    [Avg Defect Rate],
    ,
    ASC,
    DENSE
)
```

> Ranks suppliers based on defect rate performance (lower defect rate = better rank).

---

## 🔍 Inspection Metrics

### Inspection Fail Rate

```dax
Inspection Fail Rate =
DIVIDE(
    [Failed Inspection],
    [Total SKUs],
    0
)
```

> Measures the percentage of failed inspections out of total SKUs.

---

### Revenue at Risk

```dax
Revenue at Risk =
CALCULATE(
    [Total Revenue],
    supply_chain_data_001[Inspection Status] = "Fail"
)
```

> Calculates revenue associated with failed inspection records.

---

## 🚚 Logistics Metrics

### Avg Shipping Days

```dax
Avg Shipping Days =
AVERAGE(supply_chain_data_001[Shipping Days])
```

> Calculates average shipping duration across all transactions.

---

### Avg Manufacturing Lead Time

```dax
Avg Manufacturing Lead Time =
AVERAGE(supply_chain_data_001[Manufacturing Lead Time])
```

> Measures average manufacturing lead time.

---

## 🧠 Advanced Analytical Measures

### Top City

```dax
Top City =
MAXX(
    TOPN(
        1,
        VALUES(DimLocation[Location]),
        [Total Revenue],
        DESC
    ),
    DimLocation[Location]
)
```

> Identifies the highest revenue-generating city dynamically.

---

### Worst City

```dax
Worst City =
MAXX(
    TOPN(
        1,
        VALUES(DimLocation[Location]),
        [Avg Shipping Days],
        DESC
    ),
    DimLocation[Location]
)
```

> Identifies the city with the highest average shipping delays.

---

## 🎨 Visual Enhancement Measures

The following measures support **dashboard storytelling** and **conditional formatting**:

| Measure Name        | Purpose                                      |
|---------------------|----------------------------------------------|
| `Dynamic Title`     | Context-aware title for visuals              |
| `Shipping Title`    | Dynamic label for shipping-related charts    |
| `Stock Gap Title`   | Dynamic label for stock gap visuals          |
| `Defect Rate CF`    | Conditional formatting trigger for defect KPI|
| `Defect Rate Color Base` | Base color logic for defect rate visuals |

---

<div align="center">

*Built for the Supply Chain Performance Optimization Dashboard — Power BI*

</div>
