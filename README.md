# SQL Assignment — Pet Clinic Analytics

A 26-question SQL assignment analyzing a veterinary/pet-owner database: owners, pets, procedures performed, and procedure costs. The most comprehensive of the SQL practice assignments — covers a full range of query techniques in one project rather than one concept at a time.

**Covers:** `INNER`/`LEFT` joins (including `COALESCE` for missing matches), aggregate + `CASE` pivoting (counting procedure types per pet), `HAVING` on aggregates, correlated subqueries (spend above average), multi-tier `CASE` labeling (age groups, spender tiers), `RANK()` / `DENSE_RANK()` partitioned by category, `LAG()`/`LEAD()` for adjacent-row comparisons, and a CTE.

## Example

```sql
-- label owners by total spend
SELECT Owner_name, Total_Cost,
  CASE WHEN Total_Cost < 100 THEN 'Low Spender'
       WHEN Total_Cost BETWEEN 100 AND 500 THEN 'Moderate Spender'
       ELSE 'High Spender' END AS Price_Labelling
FROM ( /* subquery: total cost per owner */ ) AS CostSummary;

-- next/previous pet alphabetically
SELECT Name AS Current_Pet,
  LAG(Name) OVER (ORDER BY Name) AS Previous_Pet,
  LEAD(Name) OVER (ORDER BY Name) AS Next_Pet
FROM pets ORDER BY Name;
```

## Files

- `PetOwners.csv`, `Pets.csv`, `ProceduresDetails.csv`, `ProceduresHistory.csv`
- `SQL Assignment 2.pdf` — assignment brief
