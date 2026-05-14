# DAX Measures Documentation
This document contains the important DAX measures used in the Supply Chain Performance Optimization Dashboard developed using Power BI.

The measures are categorized based on business function and analytical purpose.

Financial KPIs
Total Revenue
Total Revenue =
SUM(supply_chain_data_001[revenue])

Purpose:
Calculates total revenue generated across all supply chain transactions.

Total Logistics Cost
Total Logistics Cost =
SUM(supply_chain_data_001[Logistics Cost])

Purpose:
Calculates the overall logistics expenditure.

Avg Logistics Cost
Avg Logistics Cost =
AVERAGE(supply_chain_data_001[Logistics Cost])

Purpose:
Measures average logistics cost per transaction.

Revenue Efficiency
Revenue Efficiency =
DIVIDE([Total Revenue], [Total Logistics Cost], 0)

Purpose:
Measures revenue generated for every unit of logistics cost incurred.

Revenue Per Unit
Revenue Per Unit =
DIVIDE([Total Revenue], [Total Units Sold], 0)

Purpose:
Measures average revenue earned per unit sold.

Adjusted Cost Score
Adjusted Cost Score =
VAR BaseCost = [Avg Logistics Cost]
VAR DefectPenalty = [Avg Defect Rate] * 50

RETURN
BaseCost + DefectPenalty

Purpose:
Combines logistics cost and defect rate impact into a weighted operational cost score.

Inventory & Supply Chain KPIs
Fill Rate
Fill Rate =
DIVIDE(
    SUM(supply_chain_data_001[Stock levels]),
    SUM(supply_chain_data_001[Units Sold]),
    0
)

Purpose:
Measures inventory fulfillment efficiency.

Avg Stock Gap
Avg Stock Gap =
AVERAGE(supply_chain_data_001[Stock Gap])

Purpose:
Calculates average stock shortage levels.

Critical Understock SKUs
Critical Understock SKUs =
COUNTROWS(
    FILTER(
        supply_chain_data_001,
        supply_chain_data_001[Stock Gap] > 400
    )
)

Purpose:
Counts SKUs with critically high stock shortages.

Supplier & Quality KPIs
Avg Defect Rate
Avg Defect Rate =
AVERAGE(supply_chain_data_001[Defect Rate Clean])

Purpose:
Calculates average product defect rate.

Supplier Quality Rank
Supplier Quality Rank =
RANKX(
    ALLSELECTED(DimSupplier[Supplier name]),
    [Avg Defect Rate],
    ,
    ASC,
    DENSE
)

Purpose:
Ranks suppliers based on defect rate performance.

Inspection Metrics
Inspection Fail Rate
Inspection Fail Rate =
DIVIDE([Failed Inspection], [Total SKUs], 0)

Purpose:
Measures percentage of failed inspections.

Revenue at Risk
Revenue at Risk =
CALCULATE(
    [Total Revenue],
    supply_chain_data_001[Inspection Status] = "Fail"
)

Purpose:
Calculates revenue associated with failed inspections.

Logistics Metrics
Avg Shipping Days
Avg Shipping Days =
AVERAGE(supply_chain_data_001[Shipping Days])

Purpose:
Calculates average shipping duration.

Avg Manufacturing Lead Time
Avg Manufacturing Lead Time =
AVERAGE(supply_chain_data_001[Manufacturing Lead Time])

Purpose:
Measures average manufacturing lead time.

Advanced Analytical Measures
Top City
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

Purpose:
Identifies the highest revenue-generating city.

Worst City
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

Purpose:
Identifies the city with the highest shipping delays.

Visual Enhancement Measures

The following measures are used for dashboard storytelling and conditional formatting support:

Dynamic Title
Shipping Title
Stock Gap Title
Defect Rate CF
Defect Rate Color Base
