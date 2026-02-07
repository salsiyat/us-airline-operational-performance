# Data Download Guide
U.S. Airline Operational Performance Analysis (2022–2024)

This document provides step-by-step instructions for downloading all required datasets
for the MSDS 420 final project. All datasets are public and must be downloaded exactly
as described to ensure consistency across the team.

---

## Dataset 1: BTS On-Time Performance Data (PRIMARY)

**Source:**  
U.S. Department of Transportation – Bureau of Transportation Statistics (BTS)  
https://www.transtats.bts.gov/DL_SelectFields.aspx?gnoyr_VQ=FGJ&QO_fu146_anzr=b0-gvzr

**Coverage:**  
- All U.S. airlines  
- Years: 2022, 2023, 2024  
- Grain: One row per flight  
- File size: ~3–4 GB per year  

### Step 1: Open the BTS Download Page
Go to the URL above and wait for the field selection page to load.

### Step 2: Select Fields (REQUIRED)
Select all of the following fields:

**Time Fields**
- Year
- Quarter
- Month
- DayofMonth
- DayOfWeek
- FlightDate

**Airline Fields**
- Reporting_Airline
- DOT_ID_Reporting_Airline
- Tail_Number
- Flight_Number_Reporting_Airline

**Origin Airport**
- OriginAirportID
- Origin
- OriginCityName
- OriginState
- OriginStateName

**Destination Airport**
- DestAirportID
- Dest
- DestCityName
- DestState
- DestStateName

**Departure Performance**
- CRSDepTime
- DepTime
- DepDelay
- DepDelayMinutes
- DepDel15

**Arrival Performance**
- CRSArrTime
- ArrTime
- ArrDelay
- ArrDelayMinutes
- ArrDel15

**Cancellation / Diversion**
- Cancelled
- CancellationCode
- Diverted

**Delay Causes**
- CarrierDelay
- WeatherDelay
- NASDelay
- SecurityDelay
- LateAircraftDelay

**Flight Details**
- CRSElapsedTime
- ActualElapsedTime
- AirTime
- Distance

### Step 3: Apply Filters
- Year: Select **ONE year only**
- Carrier: Leave blank (all airlines)
- Geography: All

### Step 4: Download
- Format: CSV
- Click **Download**
- Save files as:
  - `OnTime_2022.csv`
  - `OnTime_2023.csv`
  - `OnTime_2024.csv`

Upload files to:
