This document contains the important DAX measures used in the Supply Chain Performance Optimization Dashboard developed using Power BI.

The measures are categorized based on business function and analytical purpose.

Financial KPIs
Total Revenue
Total Revenue = SUM(supply_chain_data_001[revenue])

Purpose:
Calculates total revenue generated across all supply chain transactions.


Total Logistics Cost
Total Logistics Cost = SUM(supply_chain_data_001[Logistics Cost])

Purpose:
Calculates the overall logistics expenditure.


Avg Logistics Cost
Avg Logistics Cost = AVERAGE(supply_chain_data_001[Logistics Cost])

Purpose:
Measures average logistics cost per transaction.


Avg Manufacturing Cost
Avg Manufacturing Cost = AVERAGE(supply_chain_data_001[Manufacturing Cost])

Purpose:
Calculates average manufacturing cost across products.


Revenue Efficiency
Revenue Efficiency = DIVIDE([Total Revenue], [Total Logistics Cost], 0)

Purpose:
Measures revenue generated for every unit of logistics cost incurred.


Revenue Per Unit
Revenue Per Unit = DIVIDE([Total Revenue], [Total Units Sold], 0)

Purpose:
Measures average revenue earned per unit sold.


Adjusted Cost Score
Adjusted Cost Score =
VAR BaseCost = [Avg Logistics Cost]
VAR DefectPenalty = [Avg Defect Rate] * 50
RETURN BaseCost + DefectPenalty

Purpose:
Combines logistics cost and defect rate impact into a weighted operational cost score.


Cost Per Shipping Day
Cost Per Shipping Day =
DIVIDE([Avg Logistics Cost], [Avg Shipping Days], 0)

Purpose:
Measures logistics cost efficiency relative to shipping duration.


Inventory & Supply Chain KPIs
Total Units Sold
Total Units Sold = SUM(supply_chain_data_001[Units Sold])

Purpose:
Calculates total product units sold.


Total Stock Gap
Total Stock Gap = SUM(supply_chain_data_001[Stock Gap])

Purpose:
Measures total inventory shortage across locations.


Avg Stock Gap
Avg Stock Gap = AVERAGE(supply_chain_data_001[Stock Gap])

Purpose:
Calculates average stock shortage levels.


Fill Rate
Fill Rate =
DIVIDE(
    SUM(supply_chain_data_001[Stock levels]),
    SUM(supply_chain_data_001[Units Sold]),
    0
)

Purpose:
Measures inventory fulfillment efficiency.


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


Total SKUs
Total SKUs = COUNTROWS(supply_chain_data_001)

Purpose:
Measures total SKU count in the dataset.


Supplier & Quality KPIs
Avg Defect Rate
Avg Defect Rate =
AVERAGE(supply_chain_data_001[Defect Rate Clean])

Purpose:
Calculates average product defect rate.


Best Supplier Defect Rate
Best Supplier Defect Rate =
MINX(
    VALUES(supply_chain_data_001[Supplier name]),
    [Avg Defect Rate]
)

Purpose:
Identifies the lowest supplier defect rate.


Defect Rate vs Best
Defect Rate vs Best =
VAR Best =
    CALCULATE(
        [Avg Defect Rate],
        DimSupplier[Supplier name] = "Supplier 1"
    )
RETURN
    [Avg Defect Rate] - Best

Purpose:
Compares supplier defect rate against the benchmark supplier.


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
Failed Inspection
Failed Inspection =
COALESCE(
    CALCULATE(
        COUNTROWS(supply_chain_data_001),
        supply_chain_data_001[Inspection Status] = "Fail"
    ),
    0
)

Purpose:
Counts failed product inspections.


Passed Inspection
Passed Inspection =
COALESCE(
    CALCULATE(
        COUNTROWS(supply_chain_data_001),
        supply_chain_data_001[Inspection Status] = "Pass"
    ),
    0
)

Purpose:
Counts successful inspections.


Pending Inspection
Pending Inspection =
COALESCE(
    CALCULATE(
        COUNTROWS(supply_chain_data_001),
        supply_chain_data_001[Inspection Status] = "Pending"
    ),
    0
)

Purpose:
Counts pending inspection records.


Inspection Fail Rate
Inspection Fail Rate =
DIVIDE([Failed Inspection], [Total SKUs], 0)

Purpose:
Measures percentage of failed inspections.


Inspection Pass Rate
Inspection Pass Rate =
DIVIDE(
    [Passed Inspection],
    [Passed Inspection] + [Failed Inspection] + [Pending Inspection],
    0
)

Purpose:
Measures successful inspection percentage.


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


Shipping Efficiency
Shipping Efficiency =
AVERAGE(supply_chain_data_001[Shipping Days])

Purpose:
Tracks shipping performance efficiency.


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


Top City by Stock Gap
Top City by Stock Gap =
MAXX(
    TOPN(
        1,
        VALUES(DimLocation[Location]),
        [Avg Stock Gap],
        DESC
    ),
    DimLocation[Location]
)

Purpose:
Identifies the city with the highest inventory shortage.


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


Top Category in Top City
Top Category in Top City =
VAR SelectedCity =
    CALCULATE(
        [Top City],
        REMOVEFILTERS(DimProduct)
    )

RETURN
CALCULATE(
    TOPN(
        1,
        VALUES(DimProduct[Product Category]),
        [Total Revenue],
        DESC
    ),
    DimLocation[Location] = SelectedCity
)

Purpose:
Identifies the highest-performing product category within the top revenue-generating city.


Visual Enhancement Measures

These measures are primarily used for dynamic titles, conditional formatting, and dashboard storytelling.

Dynamic Title
Shipping Title
Stock Gap Title
Defect Rate CF
Defect Rate Color Base

Purpose:
Used for dashboard visual enhancement and conditional formatting support.
