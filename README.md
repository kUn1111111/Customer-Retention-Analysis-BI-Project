# Customer Retention Analysis – BI Project

This project analyzes customer retention trends using AdventureWorks Internet Sales data, measuring ongoing customer activity and visualizing retention performance in Power BI.

---

## Project Goals
- **Calculate Active Customers per Month**
- **Build a reusable SQL View (`vw_CustomerRetention`)**
- **Load the data into Power BI**
- Create:
  - Retention Rate KPI
  - Line Chart for monthly retention trends
  - Dashboard page with clean design and filters

---

## Data Sources

AdventureWorks tables used:
- **FactInternetSales**
- **DimDate**

---

## SQL View – `vw_CustomerRetention`

Aggregates all active customers per month by purchase date.

```sql
CREATE VIEW vw_CustomerRetention AS
SELECT
    d.FullDateAlternateKey AS MonthDate,
    COUNT(DISTINCT fs.CustomerKey) AS ActiveCustomers
FROM FactInternetSales fs
JOIN DimDate d 
    ON fs.OrderDateKey = d.DateKey
GROUP BY d.FullDateAlternateKey;
```

- Imported as a **Fact Table** into Power BI

---

## Data Modeling (Power BI)

Relationship configured:
- `DimDate[FullDateAlternateKey]` (1) → `vw_CustomerRetention[MonthDate]` (*)
    - **DimDate**: Date dimension (lookup table)
    - **vw_CustomerRetention**: Fact table

---

## DAX Measures

**Retention Rate**
```dax
Retention Rate = 
DIVIDE(
    [Active Customers],
    CALCULATE([Active Customers], ALL(DimDate))
)
```

**Active Customers**
```dax
Active Customers = SUM(vw_CustomerRetention[ActiveCustomers])
```
_(based on view column)_

---

## Dashboard Components

1. **KPI – Retention Rate**
   - **Indicator:** Retention Rate
   - **Target:** Month-over-Month benchmark or fixed threshold
   - **Direction:** High is Good
   - **Colors:**
     - Green: Retention > 70%
     - Yellow: 40–70%
     - Red: < 40%

2. **Line Chart – Monthly Retention Trend**
   - **X-axis:** Year-Month (calculated column)
     ```dax
     YearMonth = FORMAT(DimDate[FullDateAlternateKey], "YYYY-MM")
     ```
   - **Y-axis:** Retention Rate
   - **Filters:** Year slicer for usability

---

## Screenshots

KPI Cards Example  
![KPI Discount](https://bit.ly/BI-Project-KPI-Discount)

Dashboard Overview  
![Dashboard Overview](https://bit.ly/BI-Project-Dashboard-Overview)

Line Chart Example  
![Line Chart Example](https://bit.ly/BI-Project-LineCart)

---

## What This Project Demonstrates

- SQL data transformation using views
- Data modeling best practices (Star Schema)
- Analytical KPI creation
- Power BI dashboard design
- Understanding of customer retention behaviors

---

## Skills Gained

- SQL Aggregations & Joins
- DAX Calculated Columns & Measures
- BI Data Modeling
- KPI Creation
- Time Intelligence
- Dashboard Design
