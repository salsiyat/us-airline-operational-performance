# Download Guide – Airport Traffic Data
This folder stores airport Traffic data

---

## Source
https://www.transtats.bts.gov/TRAFFIC/

---

### Steps
Scroll down to the "Customize Table" section. You'll see: From: Year: 2023 Month: Jan To: Year: 2025 Month: Dec

Geographic Area: Select: ☑ System

Schedule Type: Select: ☑ Scheduled (regular flights)

Service Class: Select: ☑ Passenger

---

Operating Statistics (we have to download the data separately for each metric)

☑ Passenger Enplanements --> Traffic_Enplanements_2023_2025.csv

☑ Available Seat Miles --> Traffic_ASM_2023_2025.csv

☑ Departures Performed (Flights) --> Traffic_Departures_2023_2025.csv
   
### Expected Output
- **File:** `Airport_Traffic_2024.csv`
- **Rows:** ~10,000 (500 airports × 12 months × 3 years)
- **Columns:** Year, Quarter, Month, Airport_Code, City, State, Departures_Performed, Arrivals_Performed, Total_Operations
