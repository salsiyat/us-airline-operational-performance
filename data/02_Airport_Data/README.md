# Download Guide – Airport Reference & Traffic Data

This folder stores airport reference data and airport traffic statistics.

---

## PART A: Airport Reference Data

### Source
https://www.transtats.bts.gov/TRAFFIC/

### Steps
Scroll down to the "Customize Table" section. You'll see:
From: Year: 2023 Month: Jan
To: Year: 2025 Month: Dec

Geographic Area:
Select: ☑ Domestic  ☑ System 

Schedule Type: 
Select: ☑ Scheduled (regular flights)

Service Class:
Select: ☑ Passenger

Operating Statistics (we have to download the data separately for each metric)

☑ Passenger Enplanements --> Traffic_Enplanements_2023_2025.csv

☑ Available Seat Miles --> Traffic_ASM_2023_2025.csv

☑ Departures Performed (Flights) --> Traffic_Departures_2023_2025.csv

Click Submit

Download the Data
You'll see a new table with your customized data.
Look for "CSV" link at the top of the table --> Click "CSV" to download.
Save as: Airport_Traffic_2023_2025.csv

---

## PART B: Airport Traffic Data (T-100)

### Source
https://www.transtats.bts.gov/Data_Elements.aspx?Qn6n=N

### Steps
1. Open the link
2. Navigate to **T-100 Domestic Market (All Carriers)**
3. Select fields:
   - Airport
   - Year
   - Month
   - Passengers (Enplaned)
4. Time period:
   - Years: 2023–2025
5. Format: CSV
6. Download and save as: `Airport_Traffic_2022_2024.csv`
