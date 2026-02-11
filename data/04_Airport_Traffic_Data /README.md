# Download Guide – Airport Traffic Data
This folder stores airport traffic data.

---

## Source
Created from the merged `01_OnTime_Performance` dataset (2023–2025) using Python.

---

## Steps

1. Load the merged On-Time Performance dataset (`OnTime_2023_2025.csv`) using pandas.

2. Count departures by grouping:
   - YEAR
   - QUARTER
   - MONTH
   - ORIGIN
   - ORIGIN_CITY_NAME
   - ORIGIN_STATE_NM

3. Count arrivals by grouping:
   - YEAR
   - QUARTER
   - MONTH
   - DEST
   - DEST_CITY_NAME
   - DEST_STATE_NM

4. Rename arrival columns to match the origin structure.

5. Merge departures and arrivals using an outer join.

6. Replace missing values with 0.

7. Create calculated column:
   - Total_Operations = Departures_Performed + Arrivals_Performed

8. Export the final dataset as CSV.

---

## Expected Output

- **File:** `Airport_Traffic_2023_2025.csv`
- **Grain:** One row per airport per month
- **Rows:** ~6000
- **Columns:**  
  Year, Quarter, Month, Airport_Code, City, State,  
  Departures_Performed, Arrivals_Performed, Total_Operations
